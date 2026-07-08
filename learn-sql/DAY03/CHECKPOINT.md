# DAY03 — Checkpoint

## Questions

**1. What is the default sort order in ORDER BY?**

<details>
<summary>Answer</summary>
ASC (ascending).
</details>

**2. Which clause is evaluated first: WHERE or ORDER BY?**

<details>
<summary>Answer</summary>
`WHERE` is evaluated first.
</details>

**3. Write a query that returns the 5 most expensive products from a `products` table, sorted by price descending.**

<details>
<summary>Answer</summary>
```sql
SELECT * FROM products ORDER BY price DESC LIMIT 5;
```
</details>

**4. What does `LIMIT 10 OFFSET 20` do?**

<details>
<summary>Answer</summary>
Skips the first 20 rows and returns the next 10 rows.
</details>

**5. True or False: ORDER BY column_name DESC NULLS LAST is the default for descending order.**

<details>
<summary>Answer</summary>
False. The default for DESC is NULLS FIRST in PostgreSQL. (NULLS LAST is the default for ASC.)
</details>
