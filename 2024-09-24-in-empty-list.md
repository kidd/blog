---
title: sql "in empty list"
categories: sql,clojure
---

If you try `select * from foo where id in ()`, you'll get the error `ERROR:  42601: syntax error at or near ")"`.

And that happens because sql doesn't know what `()` is. it could be an
empty list, or an empty record, or the output of a subquery.

So, in clojure, this is a pattern we use to guard toucan from jumping into the empty pool:
```clojure
(let [addon-products (when (not-empty addon-types)
                       (tdb/select Product
                                   :product-type [:in addon-types]
                                   :active true))]
```

But it sucks in multiple ways. So I wanted to get a shorter way. Here's one:

```clojure
(seq
    (and (seq product-types)
    (tdb/select Product
       :product-type [:in product-types]
       :active true)))
```

But wait, ther's also this way, which I think it's the coolest one.

```clojure
(tdb/select Product
   :product-type [:in (or (seq product-types) [nil])]
   :active true))
```

Rings a bell that `in` in sql has weird performance characteristics
when nils are involved. In praticular, `not in` has weird properties
when nils are allowed (or part of the query, can't remember).

But `explain analyze` tells us that postgres is able to shortcut the
query when it sees `in (null)`. Fantastic!


```
explain analyze select * from product where id in (null);

------------------------------------------------------------------------------------
 Result  (cost=0.00..0.00 rows=0 width=0) (actual time=0.008..0.009 rows=0 loops=1)
   One-Time Filter: false
 Planning Time: 0.149 ms
 Execution Time: 0.235 ms
(4 rows)
```


As a bonus, here's how you'd do the case of filtering in usual UI, where "empty filter" means "no filter"

```clojure
(let [product-types []]
 (m/mapply tdb/select Product
           :active true
           (when (seq product-types) {:product-type [:in product-types]})))
```
