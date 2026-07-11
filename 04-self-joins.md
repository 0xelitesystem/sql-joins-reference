# Self joins

A self join is an ordinary join where a table is joined to itself, using table aliases to tell the two copies apart. There is no special SELF JOIN keyword; you just list the same table twice with different names. It is how you relate rows within a single table, such as employees to their managers.

## The example table

An `employees` table where each row points at its manager, who is another row in the same table:

```
employees
id | name  | manager_id
---+-------+-----------
1  | Ana   | NULL
2  | Ben   | 1
3  | Cara  | 1
4  | Dan   | 2
```

Ana has no manager, Ben and Cara report to Ana (id 1), and Dan reports to Ben (id 2).

## The query and result

Join the table to itself, aliasing one copy as the employee (`e`) and one as the manager (`m`):

```sql
SELECT e.name AS employee, m.name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id;
```

Result:

```
employee | manager
---------+--------
Ana      | NULL
Ben      | Ana
Cara     | Ana
Dan      | Ben
```

The aliases `e` and `m` are essential; without them the database cannot tell which `id` or `name` you mean. A LEFT join is used here so Ana (no manager) still appears; an INNER join would drop her.

## When to use it

Reach for a self join whenever rows in one table relate to other rows in the same table: employees and managers, categories and parent categories, or a message and the message it replies to. You can also self join to compare rows, for example finding pairs of products with the same price. The pattern is always the same: two aliases of one table and a join condition that links them.
