# DAY17 — Checkpoint

## Questions

**1. What is the difference between UNION and UNION ALL?**

<details>
<summary>Answer</summary>
`UNION` removes duplicate rows from the combined result; `UNION ALL` keeps all rows including duplicates.
</details>

**2. What requirement must two queries satisfy for UNION to work?**

<details>
<summary>Answer</summary>
They must return the same number of columns with compatible data types.
</details>

**3. What does `INTERSECT` return?**

<details>
<summary>Answer</summary>
Rows that appear in BOTH query results.
</details>

**4. Write a query that uses EXCEPT to find products that have never been ordered (using `products` and `order_items` tables).**

<details>
<summary>Answer</summary>
```sql
SELECT id FROM products
EXCEPT
SELECT product_id FROM order_items;
```
</details>

**5. Where does `ORDER BY` go in a UNION query?**

<details>
<summary>Answer</summary>
At the end of the entire combined query, after all UNION clauses.
</details>
