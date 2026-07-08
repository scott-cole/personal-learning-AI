# DAY19 — Checkpoint

## Questions

**1. What does `DATE_TRUNC('month', TIMESTAMP '2024-03-15 14:30:00')` return?**

<details>
<summary>Answer</summary>
`2024-03-01 00:00:00` — it truncates to the first of the month.
</details>

**2. How do you add 7 days to a DATE column in PostgreSQL?**

<details>
<summary>Answer</summary>
`date_column + INTERVAL '7 days'` or `date_column + 7` (adding integer to DATE adds days).
</details>

**3. What does `EXTRACT(DOW FROM date)` return?**

<details>
<summary>Answer</summary>
Day of week as an integer: 0 = Sunday, 1 = Monday, ..., 6 = Saturday.
</details>

**4. Write a query that shows the current date and time.**

<details>
<summary>Answer</summary>
```sql
SELECT NOW();
-- or
SELECT CURRENT_TIMESTAMP;
```
</details>

**5. Write a query using `TO_CHAR` to format a date as 'DD/MM/YYYY'.**

<details>
<summary>Answer</summary>
```sql
SELECT TO_CHAR(CURRENT_DATE, 'DD/MM/YYYY');
```
</details>
