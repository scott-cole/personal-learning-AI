# DAY08 — Checkpoint

## Questions

**1. Can a primary key contain NULL values?**

<details>
<summary>Answer</summary>
No. A primary key must be unique and cannot contain NULL values.
</details>

**2. What does `ON DELETE CASCADE` do?**

<details>
<summary>Answer</summary>
When a parent row is deleted, all child rows referencing it via the foreign key are also automatically deleted.
</details>

**3. Can a table have more than one foreign key?**

<details>
<summary>Answer</summary>
Yes, a table can have multiple foreign keys, each referencing a different table.
</details>

**4. What is a composite primary key?**

<details>
<summary>Answer</summary>
A primary key that spans multiple columns, where the combination of values must be unique.
</details>

**5. Write a CREATE TABLE statement for an `orders` table with a foreign key referencing `customers(id)`.**

<details>
<summary>Answer</summary>
```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INTEGER NOT NULL REFERENCES customers(id),
    order_date TIMESTAMP DEFAULT NOW(),
    total NUMERIC(10,2)
);
```
</details>
