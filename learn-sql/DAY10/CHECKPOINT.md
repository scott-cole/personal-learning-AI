# DAY10 — Checkpoint

## Questions

**1. What is the difference between LEFT JOIN and INNER JOIN?**

<details>
<summary>Answer</summary>
LEFT JOIN returns all rows from the left table even if there's no match in the right table (right columns become NULL). INNER JOIN only returns rows with matches in both tables.
</details>

**2. When would you use RIGHT JOIN instead of LEFT JOIN?**

<details>
<summary>Answer</summary>
Rarely. RIGHT JOIN can always be rewritten as LEFT JOIN by swapping the table order. Some developers prefer one direction for readability.
</details>

**3. What does FULL OUTER JOIN return?**

<details>
<summary>Answer</summary>
All rows from both tables. Rows without a match in the other table have NULLs for the other table's columns.
</details>

**4. How do you find rows in table A that have no match in table B?**

<details>
<summary>Answer</summary>
```sql
SELECT A.* FROM A LEFT JOIN B ON A.id = B.a_id WHERE B.id IS NULL;
```
</details>

**5. Write a query to list all departments and their employee names, including departments with no employees (employees LEFT JOIN departments?).**

<details>
<summary>Answer</summary>
```sql
SELECT d.name AS department, e.name AS employee
FROM departments d
LEFT JOIN employees e ON d.id = e.department_id;
```
Departments is the left/master table.
</details>
