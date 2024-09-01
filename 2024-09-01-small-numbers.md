---
title: Some small numbers
categories: [programming]
---

From time to time, some performance challenge comes up, being a
workflow at work, or an external challenge like the 1BillionRow thing:
https://www.youtube.com/watch?v=9-S_nZ5gzGE.

And then I remember that, the same way it's
"[notmuch](https://notmuchmail.org/)" mail, it's probably notmuch data
anyway. My first heuristic is always to think if excel would be able
to handle the amount of data, or if grep/ripgrep would deatl with it.

So, I was doing some back-of-napkin numbers about csv's, `duckdb`, and the
storage of tables and comparing them to plain `ripgrep`.

Zsh Helpers:
```
alias -g FORI='| while read i ; do '
alias -g IROF='; done '
```

```
seq 1000000 FORI
  echo "$i, aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa" >>table.csv
IROF
```

```
361312 -rw-r--r--@   1 rgrau  staff   165M Aug  2 10:11 table.csv
  1048 -rw-r--r--@   1 rgrau  staff   524K Aug  2 18:54 table.ddb
```

So, highly optimized backends compress 1 million rows of very predictable data to basically nothing.

Again, with less predictable data:

```
rm table.csv table.ddb
seq 1000000 FORI
  echo "$i,$(uuidgen), $(uuidgen), $(uuidgen) " >>table.csv
IROF
```
That took a few hours to generate(!!)

Duckdb importing of the 115Mb file
```
D .timer on
D create table t1 as SELECT * from 'table.csv';
Run Time (s): real 1.184 user 2.655083 sys 0.068267
```

ls -las table.*
```
262152 -rw-r--r--@   1 rgrau  staff   115M Aug  3 03:38 table.csv
131608 -rw-r--r--@   1 rgrau  staff    64M Aug  5 10:50 table.ddb
```
Not bad.

anyway, how long would it take for ripgrep to find the last line of the file?

```
time rg "$(tail -n1 table.csv | cut -f2 -d,)" table.csv

1000001:1e+06,CB834048-DB59-48C2-8F33-CEB377CC0C54, 95CC3936-812D-4AB1-8DB7-F7F3BED7973F, 1B5DD86C-777B-44BB-A404-33F540BE255D

rg "$(tail -n1 table.csv | cut -f2 -d,)" table.csv
0.03s user 0.02s system 67% cpu 0.079 total
```
