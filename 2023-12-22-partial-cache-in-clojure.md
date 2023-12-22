---
title: Partial cache in clojure
categories: [clojure]
---

I didn't think it's so uncommon to want to memoize a function, but
only for some results, but it turns out Clojure doesn't have a clear
way to do so.

In the past I've implemented it in Lua and Perl (at least), and the
pattern seems quite useful for algorithms where we look for fixed
points, and we just want to keep iterating while a datastructure is
still evolving to completion, (changing nils to final values in each
iteration).


After looking at clojure.core.cache, clojure.core.cache.wrapped, and
clojure.core.memoize, I didn't find a way to crack open the libraries
in a way that allowed me to customize how the updating of the cache
worked.

I discovered a few interesting things though:

- You can "nest" cache types: https://ask.clojure.org/index.php/12567/multi-threaded-cache-stampede-in-core-cache

- args-fn: You can tune the relevant parameters of the function to memoization via metadata `args-fn`.

- There's a CacheProtocol, which is the escape hatch to implement a custom sophisticated cache.

- video: https://www.youtube.com/watch?app=desktop&v=a_jQBWpxfQU

- There's a java lib called Caffeine, and a Clojure binding [Caffeine-memoize](https://github.com/strojure/caffeine-memoize)

Here is a first DIY approach, that combines a ttl with only caching
certain values.  This is still suceptible to cache stampedes, but the approach still holds.

```clojure
(defn checker [f]
  (let [c (atom {})]
    (fn [& args]
      (let [res (get @c args)
            res (or (and res
                         (t/< (t/minus (t/instant) (t/seconds 30)) (:ts res))
                         res)
                    {:res (apply f args)
                     :ts  (t/instant)})]
        (when (:res res)
          (swap! c assoc args res))
        (:res res)))))
```


Here's an approach using `clojure.core.cache.wrapped`. In this case,
I'm evicting the values from the cache just after inserting them.

```clojure
(def C (cache/ttl-cache-factory {} :ttl 6000))
(defn cached-fun [s r]
  (let [r (time (cache/lookup-or-miss C r (fn [r] (Thread/sleep s) r)))]
    (when (false? r)
      (cache/evict C r))
    r))
```
