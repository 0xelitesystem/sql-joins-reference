# FULL OUTER and CROSS JOIN

A FULL OUTER JOIN keeps every row from both tables, matching them where it can and filling NULLs on whichever side has no match; a CROSS JOIN ignores conditions entirely and pairs every row from one table with every row from the other. One keeps all data, the other multiplies it.

## FULL OUTER JOIN

Using the same `users` and `orders`, FULL OUTER JOIN combines the LEFT and RIGHT results: unmatched users and unmatched orders both appear.

```sql
SELECT users.name, orders.amount
FROM users
FULL OUTER JOIN orders ON users.id = orders.user_id;
```

Result:

```
name | amount
-----+-------
Ana  | 50
Ana  | 20
Ben  | NULL
Cara | 90
NULL | 40
```

Ben (a user with no order) and order 13 (an order with no user) both survive. Use FULL OUTER JOIN to reconcile two sets and see everything that fails to match on either side. Note that MySQL historically lacks FULL OUTER JOIN; you emulate it by UNION of a LEFT and a RIGHT join.

## CROSS JOIN

A CROSS JOIN has no ON condition and produces the Cartesian product: every left row paired with every right row. With 3 users and 2 colors:

```sql
SELECT users.name, colors.color
FROM users
CROSS JOIN colors;
```

3 users times 2 colors gives 6 rows, every name paired with every color. CROSS JOIN is useful on purpose for generating combinations (all sizes times all colors, or a calendar of days times rooms). It is dangerous by accident: forgetting the ON condition on a normal join, or listing tables comma-separated with no WHERE link, silently produces a Cartesian product. Two tables of 10,000 rows become 100,000,000 rows, which looks like a hang or a runaway query. If a result set is unexpectedly huge, a missing join condition is the first thing to check.
