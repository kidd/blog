---
title: Write dumb code
categories: [programming]
---

Here's an interesting thread on "write dumb code" and why it's not always
clear what "dumb code" is: https://news.ycombinator.com/item?id=39503777.
Another example https://news.ycombinator.com/item?id=40434766.

Of course, the subject is more complex than that, and very subjective
(ha, ha).

It reminded me of the "[stages of
denial](https://beyondloom.com/blog/denial.html)" post, and some of
the comments also refer to this idea that using simpler language
features doesn't always lead to simpler code and more understandable
code.

I have 2 modes of programming:

- stretch the abstractions and tools I have at hand, and map
  everything I can to these concepts

- Once I can't attain the constraint anymore, I go "all out" and look
  for a very powerful tool that can allow me to solve these, and
  potentially much more related problems.

But I try to not have many layers. I like to have 2. A bit like
suckless bash and C.

Related to the second point, it reminds of an Alan Kay Quote (from the [second thread above](https://news.ycombinator.com/item?id=40435485).):

    one of the things that's worked the best the last three, or four hundred years, is you get simplicity by finding a slightly more sophisticated building block to build your theories out of. its when you go for a simple building block that anybody understand through common sense, that is when you start screwing yourself right and left, because it just might not be able to ramify through the degrees of freedom and scaling you have to go through, and it's this inability to fix the building blocks that is one of the largest problems that computing has today in large organizations. people just won't do it

UNRELATED INSIGHT:
TCP/IP, on the other hand, gets more robust when higher in the scale,
and even it's built from flaky fallible parts, you build more robust
stuff from it. It's like an arch.

Climbing too fast in the ladder of abstractions can make code complex,
but knowing if that complexity is desired or it's avoidable by keeping
using simpler tools is where your taste as a coder lies.

There are tools that are more powerful than others. Clojure vs bash.

Tasks have an inherent complexity.

If a task lies in the lowest level, then it's ok to use the least
powerful tool. But this case is kinda stupid anyway.

If a task lies somewhere up, you sometimes can stretch the least
powerful tool, use some hack, and some ninja knowledge to get the task
solved, or OTOH, you can use the powerful tool to comfortably solve
that task. Ubiquity, dependencies, boring tech. All these things
matter. And it's again, your decision as a coder to pick the low gear
vs the high gear.

I'm a big fan of both, so sometimes I try to build everything with
just 1 abstraction, or 0 (arrays, conses, files) and stretch this to
the limit, because then, all the hacks, stretches, and knowledge is
useable in other places (see array languages for an extreme on that)
building a useful body of knowledge in a limited universe. Whereas
sometimes, using a super powerful tool for everything leads to having
to cover for many "what ifs" that do not worry you atm. Also, the
danger of ending up with a [higly specialized
tool](https://www.reddit.com/r/specializedtools/).

On the other hand, if you limit your abstraction to 0, the only thing
you can do is to pile rock blocks on top of the other, and build a
pyramid.
