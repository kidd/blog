---
title: sqlite gets jsonb!
categories: [sql]
---

Sqlite recently got [jsonb support](https://www.sqlite.org/changes.html#version_3_45_0).

From my pg experience: in pg, jsonb doesn't allow for dupes, doesn't
keep whitespace, and doesn't keep order (showing the internal
reordering they do based on key length), so I assume sqlite now got
faster when dealing with json.


```
$ psql -qXc $'select \'{"aaaa":1, "b":2,     "b":3}\'::jsonb'
        jsonb
---------------------
 {"b": 3, "aaaa": 1}
(1 row)

$ psql -qXc $'select \'{"aaaa":1, "b":2,     "b":3}\'::json'
             json
------------------------------
 {"aaaa":1, "b":2,     "b":3}
(1 row)
```
