# DAY13 — Checkpoint

## Questions

**1. What is the difference between WHERE and HAVING?**

<details>
<summary>Answer</summary>
`WHERE` filters rows BEFORE grouping; `HAVING` filters groups AFTER aggregation.
</details>

**2. In the query `SELECT department, COUNT(*) FROM employees GROUP BY department`, why can't you add `employee_name` to the SELECT?**

<details>
<summary>Answer</summary>
Because `employee_name` is not in the `GROUP BY` clause and not wrapped in an aggregate function. SQL cannot determine which employee name to show per group.
</details>

**3. What does `HAVING COUNT(*) > 5` do?**

<details>
<summary>Answer</summary>
It filters out groups that have 5 or fewer rows, keeping only groups with more than 5 rows.
</details>

**4. Write a query that shows each product category and its average price, but only for categories where the average price is above 50.**

<details>
<summary>Answer</summary>
```sql
SELECT category, AVG(price) AS avg_price
FROM products
GROUP BY category
HAVING AVG(price) > 50;
```
</details>

**5. TRUE or FALSE: You can use an aggregate function in a WHERE clause.**

<details>
<summary>Answer</summary>
False. Aggregates cannot be used in `WHERE` (they run before grouping). Use `HAVING` for aggregate conditions.
</details>
