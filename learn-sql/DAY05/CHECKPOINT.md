# DAY05 — Checkpoint

## Questions

**1. What is the difference between `VARCHAR(100)` and `TEXT`?**

<details>
<summary>Answer</summary>
`VARCHAR(100)` has a maximum length of 100 characters; `TEXT` has no practical length limit. Performance is identical.
</details>

**2. Which data type should you use to store a product price?**

<details>
<summary>Answer</summary>
`NUMERIC(10, 2)` — exact precision needed for currency.
</details>

**3. What does `TIMESTAMPTZ` store that `TIMESTAMP` does not?**

<details>
<summary>Answer</summary>
`TIMESTAMPTZ` stores the timezone offset along with the timestamp; `TIMESTAMP` does not.
</details>

**4. What is the `SERIAL` data type an alias for?**

<details>
<summary>Answer</summary>
`SERIAL` creates an `INTEGER` column with a default value from an auto-incrementing sequence.
</details>

**5. Write a CREATE TABLE statement for a `reviews` table with: auto-incrementing id, product_id (integer), rating (numeric 2,1), and content (text).**

<details>
<summary>Answer</summary>
```sql
CREATE TABLE reviews (
    id SERIAL PRIMARY KEY,
    product_id INTEGER,
    rating NUMERIC(2,1),
    content TEXT
);
```
</details>
