# Common mistakes

The two mistakes that cause the most join bugs are fan-out (a join silently multiplying your rows) and mishandling NULLs (an outer join quietly turned back into an inner one, or NULLs breaking a comparison). Both are subtle because the query runs fine and returns wrong numbers rather than an error.

## Fan-out (row multiplication)

If you join on a column that is not unique on the other side, each left row matches many right rows, and your result multiplies. Suppose you join `orders` to `order_items` to sum order amounts, but an order has 3 items; the order's amount now appears 3 times, and `SUM(orders.amount)` triples. This is fan-out, and it silently inflates counts and totals. Guard against it by knowing the cardinality of each join (is it one-to-one, one-to-many, or many-to-many?), by aggregating before joining when needed, and by using `COUNT(DISTINCT ...)` or `SUM` over the correct table. If a total suddenly looks too big, suspect a many-to-many join.

## NULL handling

Two NULL values are not equal in SQL; `NULL = NULL` is not true, it is unknown. So a join condition can never match on NULL, and any comparison against NULL in a WHERE clause drops the row. To test for missing matches after an outer join, you must write `IS NULL`, not `= NULL`.

## The outer-join-into-inner trap

A classic mistake: you write a LEFT JOIN to keep unmatched rows, then add a condition on the right table in the WHERE clause:

```sql
SELECT u.name, o.amount
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE o.amount > 30;
```

Because unmatched rows have `o.amount` as NULL, and `NULL > 30` is unknown, the WHERE filter removes exactly the rows the LEFT JOIN was keeping, turning it back into an inner join. If you need the condition to apply only to matched rows while still keeping unmatched ones, move it into the ON clause instead of the WHERE clause. This single mistake accounts for a large share of "my LEFT JOIN is dropping rows" confusion.
