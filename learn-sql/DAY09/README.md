# DAY09 — INNER JOIN

## Anecdote: INNER JOIN is a Venn Diagram Overlap

Imagine two circles on a Venn diagram. Circle A is "all books in the catalogue." Circle B is "all authors in the database." The overlapping area — where a book has a matching author AND the author has matching books — is an `INNER JOIN`.

An `INNER JOIN` returns only the rows where there's a match **in both tables**. If a book has no author assigned, it's excluded. If an author has no books, they're excluded. Only the intersection survives.

## Basic INNER JOIN Syntax

```sql
SELECT *
FROM books
INNER JOIN authors ON books.author_id = authors.id;
```

The `ON` clause specifies how to match rows between the tables.

## What You Get Back

The result is a virtual table with columns from both sides:

```
 book.id | book.title | author_id | author.id | author.name | author.birth_year
---------+------------+-----------+-----------+-------------+------------------
    1    | Dune       |     1     |     1     | Frank Herbert | 1920
```

If columns have the same name, use table aliases to disambiguate:

```sql
SELECT
    books.title,
    authors.name AS author
FROM books
INNER JOIN authors ON books.author_id = authors.id;
```

## Table Aliases

```sql
SELECT b.title, a.name AS author
FROM books b
INNER JOIN authors a ON b.author_id = a.id;
```

Short aliases (`b`, `a`) keep queries readable.

## Joining 3 Tables

```sql
SELECT
    b.title,
    a.name AS author,
    l.borrowed_at
FROM books b
INNER JOIN authors a ON b.author_id = a.id
INNER JOIN loans l ON b.id = l.book_id;
```

## JOIN Without INNER

`JOIN` without `INNER` is the same as `INNER JOIN`:

```sql
SELECT * FROM books JOIN authors ON books.author_id = authors.id;
```

## What INNER JOIN Excludes

```sql
-- Book with author_id = NULL: excluded from INNER JOIN
-- Author with no books: excluded from INNER JOIN

SELECT b.title, a.name
FROM books b
LEFT JOIN authors a ON b.author_id = a.id;
-- LEFT JOIN would include the book even with NULL author
```

## Common Pattern: Many-to-Many

```sql
CREATE TABLE books_authors (
    book_id INTEGER REFERENCES books(id),
    author_id INTEGER REFERENCES authors(id),
    PRIMARY KEY (book_id, author_id)
);

SELECT b.title, a.name
FROM books b
INNER JOIN books_authors ba ON b.id = ba.book_id
INNER JOIN authors a ON ba.author_id = a.id;
```

## Summary

| Concept | Description |
|---------|-------------|
| `INNER JOIN` | Returns rows where match exists in BOTH tables |
| `ON` | Defines the matching condition |
| Table alias | Short name like `b` for `books` |
| Excluded | Rows without matches on either side |
