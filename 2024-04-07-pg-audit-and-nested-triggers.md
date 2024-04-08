----
title: pg_audit and nested triggers to materialize a version table
categories: [postgresql]
---

I've been a happy pg_audit user for a while now, and got familiar
enough with jsonb that it doesn't bother me too much.

Also, I do not have enough volume so that dumb plans become prohibitive.


But recently, for some concrete work, I needed to materialize versions of
entities into a "X_version" table.

This is the good old [temporal
tables](https://hypirion.com/musings/implementing-system-versioned-tables-in-postgres).

What do we do then? We have solutions to our subproblems, but the
trick is how to mix them into one thing.

Here's what I got to:

On one hand, I create the temporal table in a similar way to the link above explains.

```clojure
(defn- create-version-table [table-name]
  (format "
     create table if not exists %1$s_version as
       select gen_random_uuid() as version_id, now() as version_created_at,* from %1$s with no data;

     alter table %1$s_version add primary key (version_id);
     alter table %1$s_version alter column version_id set default gen_random_uuid();
     alter table %1$s_version alter column version_id set not null;

     alter table %1$s_version alter column version_created_at set default now();
     alter table %1$s_version alter column version_created_at set not null;

     create index if not exists %1$s_version_id_version_created_at_idx on %1$s_version (id, version_created_at);
" table-name))
```

The nice trick here is that `create table ... as select ... with no
data` allows us to create a table with the same fields as another
table, just copying the schema. Given how verbose would be to do it in
a normal DDL statement, this was a big big hit.

Another NiceThing(TM) is that if we want to add more fields to it, we
can just add more output columns to the query. `gen_random_uuid() as
version_id` tells postgresql that this column is a uuid, and the
name. So `create table` has all the required info.

After that, we manually set the `not null` constraints and indices.

To tie it up, we use pg_audit library from supabase as our trigger
provider. You'll see. In the project I'm already using supabase's
`pg_audit` lib, that sets insert/update/delete triggers on a given
table and inserts rows in a single table called
`audit.record_version`. Each row has a `record::jsonb`,
`old_record::jsonb` and other (less important for us).

So I thought I could piggieback on it, and rely on `pg_audit` to
insert records. Myself, I'd have to only to create a trigger for
inserts on `audit.record_version` and that'd be it!

```sql
CREATE TRIGGER version_tables_insert_trigger
AFTER INSERT ON audit.record_version
FOR EACH ROW
EXECUTE PROCEDURE copy_inserts_into_version();
```

Next up, create the stored procedure that will populate the versioned table.

```sql
CREATE OR REPLACE FUNCTION copy_inserts_into_version()
   RETURNS TRIGGER
 AS $$
 BEGIN
   if 'table1' = NEW.table_name then
     insert into table1_version select gen_random_uuid(),NEW.ts,* from jsonb_populate_record(null::table1, NEW.record) ;
   -- elsif 'table2' = NEW.table_name then
   --   insert into table2_version select gen_random_uuid(),NEW.ts,* from jsonb_populate_record(null::table2, NEW.record) ;
   end if;
   RETURN NEW;
 END;
 $$ LANGUAGE plpgsql;
```

Some NiceStuff(TM) here too. `pg_audit` creates a row for each edit in
the audited table. That table has (among others) a `ts` field, a
`record` jsonb and an `old_record` jsonb.

For our purposes we're good just with `record` and `ts`. We will use
`insert into foobar_version select ... ` so we don't have to insert
manually field by field. The nice nice thing is that we can "explode"
a jsonb into a proper postgresql type with `jsonb_populate_record`.
We take the original table name and we cast a null into it to get a
null record of that shape. We smash `NEW.record` into it, and we use
that as the source of our `select`.  That select will need to prepend
a random uuid, and also the timestamp.

The usage of `jsonb_populate_record` is in an [implicit lateral
join](https://stackoverflow.com/questions/36127801/jsonb-populate-record-jsonb-populate-recordset-should-return-a-table).
