# DAY15 — Checkpoint

## Questions

**1. What is a scalar subquery?**

<details>
<summary>Answer</summary>
A subquery that returns exactly one row and one column (a single value).
</details>

**2. In a `FROM` subquery, does the derived table need an alias?**

<details>
<summary>Answer</summary>
Yes. `FROM (SELECT ...) AS alias` — the alias is required.
</details>

**3. What is a correlated subquery?**

<details>
<summary>Answer</summary>
A subquery that references columns from the outer query. It runs once for each row in the outer query.
</details>

**4. Write a query using a subquery to find employees who earn more than the average salary.**

<details>
<summary>Answer</summary>
```sql
SELECT * FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```
</details>

**5. TRUE or FALSE: A subquery in `WHERE IN` can return multiple columns.**

<details>
<summary>Answer</summary>
False. A subquery with `IN` must return a single column (unless used with row constructors).
</details>
