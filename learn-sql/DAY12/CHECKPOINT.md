# DAY12 — Checkpoint

## Questions

**1. What does `COUNT(*)` return vs `COUNT(column_name)`?**

<details>
<summary>Answer</summary>
`COUNT(*)` counts all rows including those with NULLs in any column. `COUNT(column_name)` counts only rows where that column is NOT NULL.
</details>

**2. What is the result of `AVG(NULL, 10, 20)` conceptually (ignoring NULLs)?**

<details>
<summary>Answer</summary>
15 — AVG ignores NULLs and averages (10 + 20) / 2.
</details>

**3. How do you calculate total revenue in a table with `price` and `quantity` columns?**

<details>
<summary>Answer</summary>
```sql
SELECT SUM(price * quantity) AS total_revenue FROM sales;
```
</details>

**4. What does `SELECT COUNT(DISTINCT category) FROM products` do?**

<details>
<summary>Answer</summary>
Counts the number of unique (non-NULL) categories in the products table.
</details>

**5. Write a query that returns the minimum, maximum, and average salary from an `employees` table.**

<details>
<summary>Answer</summary>
```sql
SELECT MIN(salary), MAX(salary), AVG(salary) FROM employees;
```
</details>
