# DAY24 — Checkpoint

## Questions

**1. What is the difference between `UNIQUE` and `PRIMARY KEY`?**

<details>
<summary>Answer</summary>
Both enforce uniqueness, but `PRIMARY KEY` also implies `NOT NULL` and a table can have only one primary key. A table can have multiple `UNIQUE` columns, which allow NULLs.
</details>

**2. Why are constraints better than application-level validation alone?**

<details>
<summary>Answer</summary>
Constraints are enforced at the database level regardless of which application or client connects. They cannot be bypassed, making data integrity guaranteed.
</details>

**3. What does `CHECK (salary > 0)` do?**

<details>
<summary>Answer</summary>
It prevents inserting or updating rows where salary is zero or negative.
</details>

**4. Write a CREATE TABLE statement for a `reviews` table with a CHECK constraint ensuring `rating` is between 1 and 5 inclusive.**

<details>
<summary>Answer</summary>
```sql
CREATE TABLE reviews (
    id SERIAL PRIMARY KEY,
    product_id INTEGER,
    rating INTEGER CHECK (rating >= 1 AND rating <= 5),
    comment TEXT
);
```
</details>

**5. What happens when you try to insert a NULL into a column with a NOT NULL constraint?**

<details>
<summary>Answer</summary>
PostgreSQL returns an error: `ERROR: null value in column "xxx" violates not-null constraint`.
</details>
