# DAY29 — Checkpoint

## Questions

**1. What is the difference between EXPLAIN and EXPLAIN ANALYZE?**

<details>
<summary>Answer</summary>
`EXPLAIN` shows the estimated query plan (doesn't run the query). `EXPLAIN ANALYZE` runs the query and shows actual execution statistics.
</details>

**2. What does a "Seq Scan" indicate?**

<details>
<summary>Answer</summary>
A sequential scan — PostgreSQL is reading every row in the table. On large tables, this often indicates a missing index.
</details>

**3. What does the cost value like `cost=0.00..12.00` represent?**

<details>
<summary>Answer</summary>
Arbitrary cost units, not milliseconds. The first number is startup cost, the second is total cost. Lower is better.
</details>

**4. How can you update the statistics used by the PostgreSQL query planner?**

<details>
<summary>Answer</summary>
Run `ANALYZE table_name;` or `VACUUM ANALYZE;`.
</details>

**5. Why might PostgreSQL choose a Seq Scan even when an index exists?**

<details>
<summary>Answer</summary>
If the query returns a large percentage of rows (e.g., > 10-20%), a sequential scan is faster than the random I/O of an index scan. Or if the table is very small.
</details>
