# DAY27 — Checkpoint

## Questions

**1. How do window functions differ from GROUP BY?**

<details>
<summary>Answer</summary>
`GROUP BY` collapses rows into groups (one row per group). Window functions compute values across rows but preserve each individual row.
</details>

**2. What does `ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC)` do?**

<details>
<summary>Answer</summary>
Assigns a unique number to each employee within their department, ordered by salary descending. The highest-paid employee in each department gets number 1.
</details>

**3. What is the difference between RANK and DENSE_RANK?**

<details>
<summary>Answer</summary>
Both assign the same rank to ties. `RANK` leaves gaps after ties (1, 1, 3); `DENSE_RANK` does not (1, 1, 2).
</details>

**4. Write a query using LAG to compare each day's sales with the previous day.**

<details>
<summary>Answer</summary>
```sql
SELECT sale_date, total,
    LAG(total, 1) OVER (ORDER BY sale_date) AS prev_day,
    total - LAG(total, 1) OVER (ORDER BY sale_date) AS change
FROM daily_sales;
```
</details>

**5. What does NTILE(4) do?**

<details>
<summary>Answer</summary>
Divides the rows into 4 approximately equal buckets (quartiles), assigning a bucket number from 1 to 4.
</details>
