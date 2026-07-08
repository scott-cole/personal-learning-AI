# DAY07 — Checkpoint

## Questions

**1. In a library database, which table holds the relationship between members and books?**

<details>
<summary>Answer</summary>
The `loans` table (also called a junction or associative table). It contains foreign keys to both `members` and `books`.
</details>

**2. What SQL keyword would you use to find books that have never been borrowed?**

<details>
<summary>Answer</summary>
`LEFT JOIN` with `IS NULL` check: `SELECT books.* FROM books LEFT JOIN loans ON books.id = loans.book_id WHERE loans.id IS NULL;`
</details>

**3. Why does the `isbn` column need a UNIQUE constraint?**

<details>
<summary>Answer</summary>
ISBNs are globally unique identifiers for books. No two books should share the same ISBN.
</details>

**4. How would you calculate the number of days a book has been borrowed?**

<details>
<summary>Answer</summary>
```sql
SELECT CURRENT_DATE - borrowed_at AS days_borrowed FROM loans WHERE returned_at IS NULL;
```
Or using `AGE()`: `SELECT AGE(CURRENT_DATE, borrowed_at) FROM loans WHERE returned_at IS NULL;`
</details>

**5. Write a query to count how many books each author has in the catalogue, ordered by count descending.**

<details>
<summary>Answer</summary>
```sql
SELECT author, COUNT(*) AS book_count FROM books GROUP BY author ORDER BY book_count DESC;
```
</details>
