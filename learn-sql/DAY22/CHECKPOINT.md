# DAY22 — Checkpoint

## Questions

**1. What is the default index type in PostgreSQL?**

<details>
<summary>Answer</summary>
B-tree.
</details>

**2. What trade-off do indexes introduce?**

<details>
<summary>Answer</summary>
Indexes speed up reads (SELECT) but slow down writes (INSERT/UPDATE/DELETE) because the index must be maintained on every data change.
</details>

**3. What is a partial index?**

<details>
<summary>Answer</summary>
An index that only includes a subset of rows, defined by a WHERE clause. It's smaller and faster for queries matching that condition.
</details>

**4. Why might a query not use an existing index?**

<details>
<summary>Answer</summary>
If the table is small (sequential scan is cheaper), or the query returns a large percentage of rows, or the column's data distribution makes the index inefficient, or the query uses a function on the column without a matching expression index.
</details>

**5. Write a CREATE INDEX statement for a composite index on `orders(customer_id, order_date)`.**

<details>
<summary>Answer</summary>
```sql
CREATE INDEX idx_orders_customer_date ON orders (customer_id, order_date);
```
</details>
