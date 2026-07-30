# SQL Joins

A plain-language reference on SQL joins: the operations that combine rows from two or more tables. It covers each join type (INNER, LEFT, RIGHT, FULL OUTER, CROSS, and self joins) with small example tables and the exact result rows, plus the mistakes that bite people most often: accidental row multiplication and NULL handling.

## The core idea

A join combines rows from two tables based on a condition, and the join type decides what happens to rows that have no match. INNER keeps only rows that match on both sides; LEFT keeps every row from the left table and fills missing right-side columns with NULL; RIGHT does the mirror image; FULL OUTER keeps everything from both sides. CROSS pairs every row with every row (no condition), and a self join is just a table joined to itself. Choose the type by asking which unmatched rows you need to keep.

## What is inside

- [01-inner-join.md](01-inner-join.md) matches on both sides, with example tables and results.
- [02-left-and-right-joins.md](02-left-and-right-joins.md) keeping unmatched rows from one side.
- [03-full-outer-and-cross-joins.md](03-full-outer-and-cross-joins.md) keeping everything, and the Cartesian product.
- [04-self-joins.md](04-self-joins.md) joining a table to itself with aliases.
- [05-common-mistakes.md](05-common-mistakes.md) fan-out row multiplication and NULL pitfalls.

## The stance

Joins are simple once you think in terms of "which unmatched rows survive." Most join bugs are not exotic; they are a forgotten join condition producing a Cartesian explosion, a many-to-many join silently multiplying rows, or a filter in the WHERE clause quietly turning an outer join back into an inner one.

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT. Copyright 0xelitesystem 2026.
