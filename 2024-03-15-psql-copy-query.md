---
title: psql copy query
categories: [postgresql]
---

Did you know that you can send the output of a query to a file with `\o`?

Just run `\o /tmp/out.txt` in a psql console, and all the queries
after that will be redirected to that file.

If you want to reset it back, `\o` will reset to stdout.

As most metacommands, if you stick them at the end of a query, before
the `;`, it will only apply to that query.

That's cool in itself, but.... there's more!

`select 1 \o |pbcopy;` that will copy the result of the query to your
clipboard. What's really happening is that we pipe the output to
`pbcopy`. So there's 2 cool things here: the fact that you can pipe
the output to a shell command, and the fact that `pbcopy` and `xclip`
exist.
