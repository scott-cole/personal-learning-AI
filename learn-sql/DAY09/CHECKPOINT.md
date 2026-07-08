# DAY09 — Checkpoint

## Questions

**1. What does INNER JOIN return when there is no matching row in the second table?**

<details>
<summary>Answer</summary>
The row is excluded from the result set — INNER JOIN only returns rows with matches in both tables.
</details>

**2. What clause specifies how two tables are matched in a JOIN?**

<details>
<summary>Answer</summary>
The `ON` clause: `INNER JOIN table2 ON table1.col = table2.col`
</details>

**3. Can you JOIN more than two tables in a single query?**

<details>
<summary>Answer</summary>
Yes, you can chain multiple JOINs: `FROM a JOIN b ON ... JOIN c ON ...`
</details>

**4. What is the difference between `JOIN` and `INNER JOIN`?**

<details>
<summary>Answer</summary>
There is no difference. `JOIN` is shorthand for `INNER JOIN`.
</details>

**5. Write a query that INNER JOINs `orders` and `customers` to show each order's ID, customer name, and total.**

<details>
<summary>Answer</summary>
```sql
SELECT o.id, c.name, o.total
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id;
```
</details>
