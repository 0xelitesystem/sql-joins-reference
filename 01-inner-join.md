# INNER JOIN

An INNER JOIN returns only the rows where the join condition matches on both tables; rows with no match on either side are dropped. It is the default join and the one you want when you only care about records that exist in both tables.

## The example tables

Two small tables, `users` and `orders`:

```
users                 orders
id | name             id | user_id | amount
---+------            ---+---------+-------
1  | Ana              10 | 1       | 50
2  | Ben              11 | 1       | 20
3  | Cara             12 | 3       | 90
                      13 | 9       | 40
```

Note Ben (id 2) has no orders, and order 13 refers to user_id 9 who does not exist in `users`.

## The query and result

```sql
SELECT users.name, orders.amount
FROM users
INNER JOIN orders ON users.id = orders.user_id;
```

Result:

```
name | amount
-----+-------
Ana  | 50
Ana  | 20
Cara | 90
```

Ana appears twice because she has two matching orders. Ben is gone (no orders), and order 13 is gone (no matching user). Only rows that pair up on `id = user_id` survive.

## When to use it

Use INNER JOIN when a row without a partner is meaningless to your question, for example "list every order with its customer name." If you also need customers who have never ordered, or orders with a broken user reference, INNER JOIN is the wrong choice and you want an outer join instead, covered next. The word INNER is optional; writing just `JOIN` means INNER JOIN in standard SQL. Order does not matter for an INNER JOIN: swapping `users` and `orders` gives the same rows, which is not true for the outer joins covered later.
