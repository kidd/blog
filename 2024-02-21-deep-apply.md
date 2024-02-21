---
title: Deep applying functions to maps
categories: [clojure]
---

I was looking for a way to apply functions to places on a tree based
on a tree of functions, so I could "merge-with-apply".

The nicest thing I could wip is by using a `deep-merge-with` that I found on the internet and `apply` itself.

```clojure
(defn deep-merge-with
  "Like merge-with, but merges maps recursively, applying the given fn
  only when there's a non-map at a particular level.
  (deep-merge-with + {:a {:b {:c 1 :d {:x 1 :y 2}} :e 3} :f 4}
                     {:a {:b {:c 2 :d {:z 9} :z 3} :e 100}})
  -> {:a {:b {:z 3, :c 3, :d {:z 9, :x 1, :y 2}}, :e 103}, :f 4}"
  [f & maps]
  (apply
    (fn m [& maps]
      (if (every? map? maps)
        (apply merge-with m maps)
        (apply f maps)))
     maps))

(deep-merge-with #(apply %1 %&)
                 {:a {:b inc :c dec}}
                 {:a {:b 1 :c 2 :d "a"}})
; => {:a {:b 2, :c 1, :d "a"}}
```

At this point, my function only works with unary functions, but it's
already a nice hack.

I imagine we'd need some sort of `reduce` to keep the map of the
functions always present in the next iterations, but this is already quite cool.

Something I learned here is that in anonymous lambdas, `%&` is not
"all arguments" but "rest of arguments", so if there's a `%1` in the
body of the function, `%&` means [%2 ...].

## unidimensional version
A simpler-to-understand variation is to use plain vectors and then imagine it lifted by the `deep-merge`:

```clojure
(map #(apply %1 [%2]) [inc dec] [1 1])
; => (2 0)
```

## The Journey to 2-arg and variadic functions

The cool idea would be to have not only unary functions, that act as
some sort of decorators, like the ones above, but to be able to really
merge multiple trees into one, but merging each different node with a
different function.


The problem is that deep-merge applies the function `f` to the "accum"
and the new node (thinking like a reducer). The function `f` is the
one that stays constant over the applications.

So I briefly thought that maybe something with `apply` and applying
`deep-merge-with` I'd keep the "tree of functions" always present.


```clojure
(deep-merge-with (partial deep-merge-with #(apply %1 %&) {:a {:b + :c -}})
                 {:a {:b 1 :c 2 :d "a"}}
                 {:a {:b 1 :c 2}})
```

But it didn't work. Because the function `f` is applied to the "accum"
and "new" but it has no idea where it lives in the tree, so it can't
figure out what's the function to apply.


Next thing: Try to build a tree of values, and later do an apply over
the accumulated values. That works for variadic functions.

The way to get the result is also "multilayer" as in, it applies
`deep-merge-with` twice, which makes it cool, and it also has 2 clear
phases, which makes it easier to understand in batches, and doesn't go
into a CPS style mindfuck, but exposes a quite dull strategy.


```clojure
(deep-merge-with #(apply %1 %2)
                 {:a {:b + :c -}}
                 (deep-merge-with vector
                                  {:a {:b 1 :c 2 :d "a"}}
                                  {:a {:b 1 :c 2}}))
; => {:a {:b 2, :c 0, :d "a"}}
```

In my head I imagine a stack of papers with trees drawn into them, and
the operations working in the vertical axis. A bit Like John Schole's
APL game of life.

Unfortunately, this doesn't quite work yet for more than 2 trees:
```clojure
(deep-merge-with #(apply %1 %2)
                 {:a {:b + :c -}}
                 (deep-merge-with vector
                                  {:a {:b 1 :c 2 :d "a"}}
                                  {:a {:b 1 :c 2}}
                                  {:a {:b 1 :c 2}}))
; => Boom
```

The reason being that `(vector 1 1) => [1 1]` , but it's not
"reduceable": `(vector [1 1] 1) => [[1 1] 1]`, and that messes up
everything.

After playing with `conj`,`into`, `vec`, `vector`, `flatten`,
`concat`, and versions of them with `partial` and/or `apply`, and
failing, I created an ad-hoc function that returns a vector of 1
dimension always.

```clojure
(deep-merge-with
 #(apply %1 %2)
 {:a {:b + :c -}}
 (deep-merge-with #(if (coll? %1)
                     (conj %1 %2)
                     [%1 %2])
                  {:a {:b 1 :c 2 :d "a"}}
                  {:a {:b 1 :c 2}}
                  {:a {:c 2}}))
; => {:a {:b 2, :c -2, :d "a"}}
```

Ugly af, but it works now! And using `reduce` in the top level one
instead of `apply`, we get to make it work with 2 arg functions too.

```clojure
(defn sum [a b]
  (+ a b))
(deep-merge-with
 #(reduce %1 %2)
 {:a {:b sum :c -}}
 (deep-merge-with #(if (coll? %1)
                     (conj %1 %2)
                     [%1 %2])
                  {:a {:b 1 :c 2 :d "a"}}
                  {:a {:b 1 :c 2}}
                  {:a {:b 1 :c 2}}))
; => {:a {:b 3, :c -2, :d "a"}}
```

yay!
