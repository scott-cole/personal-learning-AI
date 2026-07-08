# DAY18 — Checkpoint

## Questions

**1. What does `CONCAT('a', NULL, 'b')` return in PostgreSQL?**

<details>
<summary>Answer</summary>
`'ab'` — `CONCAT` ignores NULL arguments (unlike the `||` operator which returns NULL).
</details>

**2. How do you remove leading and trailing spaces from a string?**

<details>
<summary>Answer</summary>
`TRIM(string)` or `TRIM(BOTH ' ' FROM string)`.
</details>

**3. What function converts a string to title case?**

<details>
<summary>Answer</summary>
`INITCAP('hello world')` → `'Hello World'`.
</details>

**4. Write a query to find the position of '@' in the string 'alice@email.com'.**

<details>
<summary>Answer</summary>
`SELECT POSITION('@' IN 'alice@email.com');` — returns 6.
</details>

**5. Write a query that replaces all spaces with underscores in a column named `title`.**

<details>
<summary>Answer</summary>
```sql
SELECT REPLACE(title, ' ', '_') FROM books;
```
</details>
