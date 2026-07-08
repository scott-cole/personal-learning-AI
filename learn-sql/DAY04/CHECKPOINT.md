# DAY04 — Checkpoint

## Questions

**1. What does the `RETURNING` clause do in INSERT/UPDATE/DELETE?**

<details>
<summary>Answer</summary>
It returns the affected rows (or specified columns) after the operation completes.
</details>

**2. What happens if you run `UPDATE employees SET salary = 50000` without a WHERE clause?**

<details>
<summary>Answer</summary>
Every row in the `employees` table will have its salary set to 50000.
</details>

**3. Write a query to delete the product with id = 42 from a `products` table.**

<details>
<summary>Answer</summary>
```sql
DELETE FROM products WHERE id = 42;
```
</details>

**4. What is the difference between `DELETE FROM orders` and `TRUNCATE TABLE orders`?**

<details>
<summary>Answer</summary>
`DELETE` scans each row and fires triggers; `TRUNCATE` removes all rows in one operation (faster, cannot use WHERE, resets storage). `TRUNCATE` also cannot be rolled back in some contexts.
</details>

**5. Write a single INSERT statement that adds two new authors at once to an `authors` table with columns `name` and `birth_year`.**

<details>
<summary>Answer</summary>
```sql
INSERT INTO authors (name, birth_year) VALUES
('Octavia Butler', 1947),
('N.K. Jemisin', 1972);
```
</details>
