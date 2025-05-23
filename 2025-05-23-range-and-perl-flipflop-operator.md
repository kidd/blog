---
title: Range And Perl Flipflop Operator
categories: [programming]
---

A great helper I use in many shellscripts is `range`:

```
range() {
  # https://perldoc.perl.org/perlop#Range-Operators
  perl -ne "print if ${1:-1}"
}
```

This uses the great range operator. And that operator deserves a few mentions/examples

You can use it to print:

- `range 2..-1`          from a line to till the end
- `range 2..4`           from a line to another line
- `range /begin/..4`     from a matching to another line
- `range /begin/../bar/` from a matching to another line (and flipflop)
- `range /begin/../bar/ && exit` from a matching to another line (and exit)
- `range '1../^$/ && ++$a==5 && exit` print the first 5 paragraphs
