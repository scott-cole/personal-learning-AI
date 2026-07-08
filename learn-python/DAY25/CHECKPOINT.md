# DAY25 Checkpoint

## Questions

1. What module is used to work with SQLite in Python?
2. Why should you use `?` placeholders instead of f-strings in SQL queries?
3. What does `cursor.fetchone()` return if no rows match?
4. How does using `conn` as a context manager (`with conn:`) affect commits and rollbacks?
5. What does `conn.row_factory = sqlite3.Row` enable?

<details>
<summary>Answers</summary>

1. `sqlite3` (built-in, no installation needed).
2. To prevent SQL injection — placeholders safely escape values before inserting them into the query.
3. `None`.
4. It auto-commits on success and auto-rolls back on exception.
5. It allows accessing columns by name (`row["column_name"]`) instead of by numeric index.

</details>
