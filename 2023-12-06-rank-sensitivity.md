---
title: Rank Sensitivity in K
categories: [apljk]
---

I wanted to try Advent of Code 23, and of course, it all went down an
interesting rabbit hole even before starting.

One way I found to get data into a program (like __DATA__ in perl) was a snippet in discord:

```
/
2,2,2
1,2,2
3,2,2
2,1,2
\

data:{x:0:`argv 1; 1_(x?,"\\")#x}[]
data
```

and when trying to figure out why tf it worked....


# Rank sensitivity

The `(x?,"\\")#x` part will do a find (`?`) on the `argv 1`, which in
[ngn/k site](https://ngn.codeberg.page/k) gets all the lines of the
program.

I thought `find` was right-atomic, and in oK it is, but in ngnk it is not.

```
rgrau: I'm trying to understand find ? in ngnk. in some cases it feels right atomic (like oK does),
but some other cases it takes the full right argument to do the search, or some cases it seems it only goes down "1 level".

    "one"?"two" / 0N 0N 0 . right atomic?
    ("one";"two")?"two" / 1 . takes full right arg. ok gives 0N 0N 0N
    ("one";"two")?("two"; "one"; "three") / 1 0 0N . 1 level down?

It's very DWIM but it's indistinguishable from magic.  Any trick to fit this in my brain?

chrispsn: This is ‘rank sensitivity’
chrispsn: See https://matrix.to/#/!laJBzNwLcAOMAbAEeQ%3Amatrix.org/%242AMsFKfIBmSk2-opKjzgZj6tXLQdyEfQpRZoSCUhTvM
chrispsn: The difference in rank between x and y determines what find outputs
rgrau: ...magic... 🙂 . thanks!
```

And that lead to:

https://code.kx.com/q/ref/find/#type-specific and
https://news.ycombinator.com/item?id=27850272, which seem to indicate
that `find` will drill down `y` until it looks like that's what you
want to search for. Extremely DWIM.


Back to the initial code, so, `(x?,"\\")#x` will do a reshape of x
cutting it at the position where Find finds "\\". At first I didn't
get why the Enlist of "\\" was needed, but if we look at `argv 1` we
see that single character lines are `,"a"` rather than just `"a"`,
because a single char is not really a list (or something).

Anyway, now that it's clear how we read that csv, we could convert it
to a real 3x3 array with regular k, with

```
`I$''","\'data
/ or
.''","\'data
```
