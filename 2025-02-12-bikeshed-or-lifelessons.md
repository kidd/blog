---
title: Postgres support for trailing commas. Bikeshedding or life lessons?
categories: [postgres,programming]
---

Could be in #random , but it's kinda too technical, and it's sql-y,
but what I like most is the multiple dimensions of the discussion.

https://news.ycombinator.com/item?id=43010365


- The article is about adding trailing comma support to postgres.
- The post is from one of the few Core committers to postgres, so it
  "can't" be irrelevant.
- At the same time, it's a massive bikeshedding thread. Everyone is
  entitled to opinions, and whatnot.
- At the same time, comments talk about love, or hate, or lot of them
  don’t think this is worth the changeover.
- Scattered across the comments, there are the old practitioner's
  tricks to get by in an imperfect world, like leading commas, or
  `where 1=1\n and ...\n and ...`
- Looks like a simple change, but in sql world, you have to worry
  about backward and forward compat, and it's all extra fun.
- meanwhile, duckdb, clickhouse went ahead and just implemented it.
