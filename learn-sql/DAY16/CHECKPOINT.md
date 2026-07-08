# DAY16 — Checkpoint

## Questions

**1. What does `EXISTS` check for in a subquery?**

<details>
<summary>Answer</summary>
It checks whether the subquery returns at least one row (regardless of the column values).
</details>

**2. How is `= ANY (subquery)` different from `IN (subquery)`?**

<details>
<summary>Answer</summary>
They are equivalent for most use cases. `= ANY` is more flexible because it works with comparison operators (>, <, etc.).
</details>

**3. What does `x > ALL (subquery)` mean?**

<details>
<summary>Answer</summary>
x must be greater than every single value returned by the subquery.
</details>

**4. Write a query using EXISTS to find all departments that have at least one employee.**

<details>
<summary>Answer</summary>
```sql
SELECT * FROM departments d
WHERE EXISTS (SELECT 1 FROM employees e WHERE e.department_id = d.id);
```
</details>

**5. TRUE or FALSE: `NOT EXISTS (subquery)` is equivalent to `LEFT JOIN ... IS NULL`.**

<details>
<summary>Answer</summary>
True — they achieve the same result (finding rows with no match), though `NOT EXISTS` can be more efficient in some cases.
</details>
