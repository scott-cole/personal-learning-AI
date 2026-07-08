# DAY04 — INSERT, UPDATE, DELETE (CRUD Operations)

## Anecdote: CRUD is a Librarian's Daily Tasks

Meet Maria, a librarian. Her job has four repeating tasks:

1. **Create** — A new book arrives. She stamps it, assigns a shelf, and logs it into the catalogue. (INSERT)
2. **Read** — A patron asks for every book by Ursula K. Le Guin. She looks it up. (SELECT — we already know this)
3. **Update** — A book's shelf location changes because the fiction section was reorganised. She edits the record. (UPDATE)
4. **Delete** — A book is damaged beyond repair. She removes it from the catalogue. (DELETE)

Together these four operations are called **CRUD** — Create, Read, Update, Delete. About 90% of what you do in SQL is CRUD. You already know Read (SELECT). Now let's learn the other three.

## INSERT — Adding New Rows

Insert a single row:

```sql
INSERT INTO books (title, author, year, shelf)
VALUES ('The Hobbit', 'J.R.R. Tolkien', 1937, 'D-01');
```

Insert multiple rows:

```sql
INSERT INTO books (title, author, year, shelf) VALUES
('1984', 'George Orwell', 1949, 'D-02'),
('Brave New World', 'Aldous Huxley', 1932, 'D-03');
```

Omitting columns uses their default values (or NULL):

```sql
-- Only title and author, year and shelf will be NULL
INSERT INTO books (title, author) VALUES ('Unknown', 'Anonymous');
```

## INSERT with RETURNING

PostgreSQL can return the inserted data, including auto-generated values:

```sql
INSERT INTO books (title, author, year, shelf)
VALUES ('Foundation', 'Isaac Asimov', 1951, 'E-01')
RETURNING *;
```

This returns the full row including the auto-generated `id`:

```
 id |   title    |    author     | year | shelf
----+------------+---------------+------+-------
  8 | Foundation | Isaac Asimov  | 1951 | E-01
```

## UPDATE — Modifying Existing Rows

**Always use WHERE with UPDATE** unless you intend to modify every row:

```sql
-- Change the shelf of one specific book
UPDATE books SET shelf = 'Z-99' WHERE title = 'Dune';

-- Change multiple columns
UPDATE books SET year = 2020, shelf = 'NEW' WHERE id = 1;

-- Change all rows (no WHERE — be careful!)
UPDATE books SET shelf = 'GENERAL';
```

## UPDATE with RETURNING

```sql
UPDATE books SET shelf = 'A-01' WHERE id = 1 RETURNING *;
```

## DELETE — Removing Rows

**Always use WHERE with DELETE** unless you want to empty the table:

```sql
-- Delete a specific book
DELETE FROM books WHERE id = 10;

-- Delete all books published before 1900
DELETE FROM books WHERE year < 1900;

-- Delete all rows (table structure remains)
DELETE FROM books;
```

## DELETE with RETURNING

```sql
DELETE FROM books WHERE year < 1950 RETURNING title, year;
```

## TRUNCATE — Fast Delete All

`TRUNCATE` is faster than `DELETE FROM table` because it doesn't scan rows:

```sql
TRUNCATE TABLE books;
```

It also cannot be used with WHERE. It wipes the entire table immediately.

## Summary

| Operation | Command | Danger Level |
|-----------|---------|-------------|
| Create | `INSERT INTO ... VALUES (...)` | Safe |
| Create + return | `INSERT INTO ... VALUES (...) RETURNING *` | Safe |
| Update | `UPDATE ... SET ... WHERE ...` | Medium — forgot WHERE? Oops |
| Delete | `DELETE FROM ... WHERE ...` | High — forgot WHERE? Data gone |
| Fast wipe | `TRUNCATE TABLE ...` | Extreme — no recovery |
