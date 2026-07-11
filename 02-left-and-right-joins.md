# LEFT and RIGHT JOIN

A LEFT JOIN returns every row from the left table plus matching rows from the right, filling the right-side columns with NULL where there is no match; a RIGHT JOIN does the same but keeps every row from the right table. They are how you keep unmatched rows from one side.

## LEFT JOIN

Using the same `users` and `orders` from file 01:

```sql
SELECT users.name, orders.amount
FROM users
LEFT JOIN orders ON users.id = orders.user_id;
```

Result:

```
name | amount
-----+-------
Ana  | 50
Ana  | 20
Ben  | NULL
Cara | 90
```

Ben now appears with a NULL amount because LEFT JOIN keeps him even though he has no order. This is exactly how you answer "list all users and their orders, including users who have ordered nothing." To find only the non-orderers, add `WHERE orders.user_id IS NULL`.

## RIGHT JOIN

RIGHT JOIN is the mirror image: it keeps every row from the right table. If `orders` is on the right, order 13 (user_id 9, no matching user) survives with NULL name:

```sql
SELECT users.name, orders.amount
FROM users
RIGHT JOIN orders ON users.id = orders.user_id;
```

```
name | amount
-----+-------
Ana  | 50
Ana  | 20
Cara | 90
NULL | 40
```

## A note on style

RIGHT JOIN is used less often because any RIGHT JOIN can be rewritten as a LEFT JOIN by swapping the table order, and most people find LEFT easier to read. Pick a direction and stay consistent. The important idea is the same for both: the "kept" side always appears in full, and the other side contributes NULLs where it has no match.
