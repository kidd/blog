---
title: How to duplicate a row and edit a few fields in postgres
categories: [postgres]
---

Remember stackoverflow? that site where we used to look for expert knowledge?"

I found https://dba.stackexchange.com/questions/148908/is-there-a-way-to-clone-a-row-in-a-maintainable-way-with-postgresql and its link to https://dba.stackexchange.com/questions/122120/duplicate-row-with-primary-key-in-postgresql/122144#122144

These 2 links show us how to "duplicate" a row in pg in a nice way that spares us from typing all the fields.


## hstore
One solution, using hstore is
```
INSERT INTO people
SELECT (p #= hstore('id', '3')).* FROM people p WHERE id = 2;
```

It's interesting, the kind of "merge" syntax using `#=` and
`hstore`. The details are a bit convoluted (there's some casting to
text, and cast back to the original types), but still, looks :+1:



## temp table

I prefer this one though.
```
CREATE TEMP TABLE people_tmp ON COMMIT DROP AS
SELECT * FROM people WHERE id = 2;
UPDATE people_tmp SET id = nextval(pg_get_serial_sequence('people', 'id'));
INSERT INTO people TABLE people_tmp;
```

In those links there are more advanced versions (using jsonb, or creating a function to "clone_row", or clone child tables)

Anyway, nice knowledge pill
