# DAY02 — Checkpoint

## Questions

**1. Which clause filters rows before they are returned?**

<details>
<summary>Answer</summary>
`WHERE`
</details>

**2. What does `WHERE salary BETWEEN 30000 AND 50000` include? (Are the boundaries inclusive?)**

<details>
<summary>Answer</summary>
Yes, `BETWEEN` is inclusive — it includes 30000 and 50000.
</details>

**3. True or False: In PostgreSQL, string comparisons in WHERE are case-insensitive by default.**

<details>
<summary>Answer</summary>
False. They are case-sensitive by default.
</details>

**4. What is the difference between `WHERE a OR b AND c` and `WHERE a OR (b AND c)`?**

<details>
<summary>Answer</summary>
`AND` has higher precedence than `OR`, so `a OR b AND c` is parsed as `a OR (b AND c)`. They are the same. But `(a OR b) AND c` is different — it requires either a or b to be true AND c to be true.
</details>

**5. Write a query that returns products with a price greater than 100 and a category of 'Electronics'.**

<details>
<summary>Answer</summary>
```sql
SELECT * FROM products WHERE price > 100 AND category = 'Electronics';
```
</details>
