---
title: Inequality comparisons in lisp
categories: [programming]
---

Let me write some verbose reflexion about a tiny style thing about writing conditions in lisp.

I once read a discussion about "yoda conditions" and "how to make
conditions readable" and the popular knowledge says that lisp is less
susceptible to typos where yoda conditions save you.

Someone said that what he tried to do in all conditions is, instead of
having a rule of "variable first" or "constant first", in `<` or `>`
he tried to always use `<`, and think of it as a gradient that goes
up, seeing the upper line of `<` like a line chart that goes up (📈).


The advantage is that when you see `<`, it's easy to think "growing
series elements".

```clojure
(< -1 0 2 ##Inf)
(t/< yesterday today tomorrow)
```

You can see it as an X axis in a graph, where things grow to the right.

If you know you write things this way, when you read `<` you know your
past self tried to make the parameters readable in this "progression style".

Also, on the timestamp version, it's way better to use `<` than
`after/before`. Thinking in a timeseries axis removes any doubt.
