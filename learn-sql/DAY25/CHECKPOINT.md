# DAY25 — Checkpoint

## Questions

**1. What is a view in SQL?**

<details>
<summary>Answer</summary>
A view is a saved query that you can SELECT from as if it were a table. It doesn't store data itself — it runs the underlying query each time.
</details>

**2. How is a materialized view different from a regular view?**

<details>
<summary>Answer</summary>
A materialized view stores the query results physically on disk. It needs `REFRESH MATERIALIZED VIEW` to update. Regular views run the query every time and always reflect current data.
</details>

**3. What does `WITH CHECK OPTION` do?**

<details>
<summary>Answer</summary>
It prevents INSERT or UPDATE operations on a view that would create rows not visible in the view itself.
</details>

**4. Can you INSERT into a view?**

<details>
<summary>Answer</summary>
Yes — if the view is based on a single table without aggregates, DISTINCT, or GROUP BY (auto-updatable views).
</details>

**5. Write a statement to refresh a materialized view named `monthly_summary`.**

<details>
<summary>Answer</summary>
```sql
REFRESH MATERIALIZED VIEW monthly_summary;
```
</details>
