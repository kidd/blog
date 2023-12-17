---
title: Alive programs
categories: []
---

I loved this [tron
escape](https://blog.danielwellman.com/2008/10/real-life-tron-on-an-apple-iigs.html)
story the first time I read it. It gives a different point of view to
"live coding" concepts.


I'm writing clojure in my day to day (which I think it's on the 8/10
interactive language), but I don't think we feel this liveliness
coding in clojure.

The ways we tend to minimize state in web apps make it so that everything is
either stored in the db, or cached for speed purposes, but webapps are
not supposed to hold non-trivial information inbetween requests.

That's not like that in smalltalk, or in CL, and that's why they use
continuation passing web frameworks. Because they are used to this
style of programming.

I've been doing some AoC and `entr` has been of great help doing the
little exercises. Of course, those are fast-start no-state-needed
small scripts so they don't compare to a web app, but what I'm just
saying is that even clojure, for most of my tasks, could be any other
dynamic language with fast reload. It's funny I'm saying this because
I know how different the experience is (worked in CL for a while), but
that's what most of daily tasks look like.

Also, Clojure and its terrible stacktraces, and the non-triviality of
coding while running tests because the repl is kinda shared.
