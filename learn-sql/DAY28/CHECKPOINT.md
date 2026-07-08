# DAY28 — Checkpoint

## Questions

**1. What does CTE stand for and what keyword introduces it?**

<details>
<summary>Answer</summary>
Common Table Expression, introduced with `WITH`.
</details>

**2. What is a recursive CTE used for?**

<details>
<summary>Answer</summary>
Querying hierarchical or tree-structured data (org charts, category trees, bill of materials), or generating sequences.
</details>

**3. What are the two parts of a recursive CTE?**

<details>
<summary>Answer</summary>
The base case (non-recursive term, runs once) and the recursive step (references the CTE itself, runs repeatedly). They are connected by `UNION ALL`.
</details>

**4. Write a CTE named `revenue` that calculates total sales per customer, then SELECT from it to find customers with revenue > 1000.**

<details>
<summary>Answer</summary>
```sql
WITH revenue AS (
    SELECT customer_id, SUM(total) AS total
    FROM orders GROUP BY customer_id
)
SELECT * FROM revenue WHERE total > 1000;
```
</details>

**5. TRUE or FALSE: In PostgreSQL, CTEs are always materialized (executed once) by default.**

<details>
<summary>Answer</summary>
True — CTEs act as optimization fences and are materialized unless `NOT MATERIALIZED` is specified (PG 12+).
</details>
