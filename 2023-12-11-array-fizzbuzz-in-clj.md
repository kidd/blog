---
title: Array FizzBuzz in Clojure
categories: [apljk]

---

This is a translation of an array-like fizzbuzz to clojure. I think it
maps quite well.


```clojure
(->> (range 1 20)
           (map #(vector % [(= 0 (mod % 3)) (= 0 (mod % 5))]))
           (map #({[true false] "fizz"
                   [false true] "buzz"
                   [true true] "fizzbuzz"
                   [false false] (first %)}
                  (second %))))
```
