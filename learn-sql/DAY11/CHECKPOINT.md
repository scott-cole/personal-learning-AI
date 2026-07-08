# DAY11 — Checkpoint

## Questions

**1. How many rows does a CROSS JOIN of a 5-row table and a 4-row table produce?**

<details>
<summary>Answer</summary>
20 rows (5 × 4).
</details>

**2. What is a self-join?**

<details>
<summary>Answer</summary>
A self-join is when a table is joined with itself, using table aliases to distinguish the two roles.
</details>

**3. Why would you use LEFT JOIN instead of INNER JOIN in a self-join on an employee-manager table?**

<details>
<summary>Answer</summary>
The top-level employee (CEO) has no manager. INNER JOIN would exclude them. LEFT JOIN keeps all employees.
</details>

**4. Write a self-join query to find employees who share the same manager (without showing self-pairs).**

<details>
<summary>Answer</summary>
```sql
SELECT e1.name, e2.name, m.name AS manager
FROM employees e1
JOIN employees e2 ON e1.manager_id = e2.manager_id
JOIN employees m ON e1.manager_id = m.id
WHERE e1.id < e2.id;
```
</details>

**5. TRUE or FALSE: CROSS JOIN requires an ON clause.**

<details>
<summary>Answer</summary>
False. CROSS JOIN does not use an ON clause — it produces all possible combinations.
</details>
