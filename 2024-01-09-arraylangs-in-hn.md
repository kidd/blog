---
title: arraylangs in hn
categories: [apljk]
---

A couple of links on HN this week about Array Programming Languages.

# [Origins of J](https://news.ycombinator.com/item?id=38866622)

Yet another one of the dissections of the J incunabulum. Some links to
the B explanation, and one that I didn't know about:
https://github.com/kparc/bcc/blob/master/d/sidenotes.md . Someone that
worked with atw (Arthur Whitney) takes a dive into "understanding atw
style". Very very cool.


# [k on pdp11](https://news.ycombinator.com/item?id=38912406)

Ktye's site has a bunch of k resources and implementations. This is
one that I don't fully grasp ¯\_(ツ)_/¯.

Ktye's site is, similar to nsl, or the old fravia's (30years ago), a
rabbit hole from which you're not going out easily. Something I found in this dive:

today: https://ktye.github.io/kdoc.htm#intro

    the source code looks like go but it isn't. it's a structural assembly language that
    also happens to satisfy the go compiler.
    a separate compiler parses the source and writes a k table intermediate representation.
    compilers written in k transform the IR to other languages: c go wasm wasi.
	the fact that the source can also be directly compiled with a go compiler simplifies
	bootstrapping testing and development by no small amount.

:)

# Array Programming and Lisp

And an unrelated link, but I recently reread the lisp style guide from
Norvig and Pitman, and it's always worth a
[link](https://www.cs.umd.edu/~nau/cmsc421/norvig-lisp-style.pdf).  I
went there after reading this [Array Languages for Lisp
Programmers](http://archive.vector.org.uk/art10500180) classic.  The
trigger was the lambda with default `x,y,z` params, that reminded me
of Norvig's reader lambda `#L`.`
