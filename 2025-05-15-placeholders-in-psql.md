---
title: placeholders in psql
categories: [postgresql]
---

in psql, you can use a combination of `variables` (`\set foo 42`) and `\i` to have some sort of scripting workflow.

I had to migrate (run some sql) on a list of customers. But the process needs to be inspected on the way.

Here's the flow:
```
\i list_remaining_customers.sql -- that lists the ids of the customers that haven't been migrated yet
\set customer_id <paste-from-list>
\i migrate_customer.sql
-- check everything is ok
commit;  -- or rollback;
```

imagine that `migrate_customer.sql` is something like:
```
begin;
update customers set migrated=true where id=:customer_id;
```

After running that, you are still inside the transaction so you can
inspect the world, and decide if you want to commit or rollback.
