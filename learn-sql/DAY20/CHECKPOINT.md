# DAY20 — Checkpoint

## Questions

**1. What is the difference between `COALESCE(x, y)` and `CASE WHEN x IS NOT NULL THEN x ELSE y END`?**

<details>
<summary>Answer</summary>
They are functionally identical. `COALESCE` is just a shorthand for this common CASE pattern.
</details>

**2. In a searched CASE expression, when does `ELSE` run?**

<details>
<summary>Answer</summary>
`ELSE` runs when none of the `WHEN` conditions evaluate to TRUE.
</details>

**3. Write a CASE expression that categorizes ages: under 18 as 'Minor', 18-65 as 'Adult', over 65 as 'Senior'.**

<details>
<summary>Answer</summary>
```sql
CASE
    WHEN age < 18 THEN 'Minor'
    WHEN age <= 65 THEN 'Adult'
    ELSE 'Senior'
END
```
</details>

**4. Can CASE be used in a WHERE clause?**

<details>
<summary>Answer</summary>
Yes. `WHERE CASE WHEN condition THEN true ELSE false END` — though it's often clearer to use regular boolean logic.
</details>

**5. What does `NULLIF(0, 0)` return?**

<details>
<summary>Answer</summary>
`NULL` — because both arguments are equal.
</details>
