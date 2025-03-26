---
title: psql clear and watch
categories: [postgres]
---

If you want to run a query in `psql` repeatedly, you can use `\watch 1`.

But after a while, the screen gets filled with data, and you need to
keep track of the last query that ran by looking at the bottom of the
screen.

I discovered there's a metacommand in psql to clear the screen before
every query:
```
\C `clear`
```

If you combine both you get a similar behaviour to using bash's `watch`.

```
\C `clear`
select 1 \watch 1
```

To reset the `\C` back, you just `\C` without parameters.
