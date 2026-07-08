# DAY06 — Checkpoint

## Questions

**1. What is the difference between `NULL` and `0`?**

<details>
<summary>Answer</summary>
`NULL` is the absence of a value; `0` is a numeric value. They are not equivalent.
</details>

**2. Why does `WHERE column = NULL` never return rows?**

<details>
<summary>Answer</summary>
Because `NULL` is not equal to anything, not even itself. `= NULL` evaluates to `NULL` (unknown), not `TRUE`. You must use `IS NULL`.
</details>

**3. What does `COALESCE(NULL, 5, 10)` return?**

<details>
<summary>Answer</summary>
`5` — it returns the first non-NULL value.
</details>

**4. What does `NULLIF(0, 0)` return?**

<details>
<summary>Answer</summary>
`NULL` — because the two arguments are equal.
</details>

**5. Write a query that returns all rows from `orders` where the `shipped_date` column is missing (NULL).**

<details>
<summary>Answer</summary>
```sql
SELECT * FROM orders WHERE shipped_date IS NULL;
```
</details>
