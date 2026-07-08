# DAY03 — ORDER BY, LIMIT, OFFSET

## Anecdote: ORDER BY is Sorting Mail

Imagine you're a postal worker in a small town. Every morning, a sack of unsorted letters lands on your desk — envelopes from all over, addressed to every street. You need to deliver them. Do you walk the same route six times because the letters are in random order? Of course not. You **sort** them by street name first, then by house number.

`ORDER BY` is that sorting step. It takes the messy pile of rows and arranges them exactly how you want before they're handed to you.

`LIMIT` is the post office truck — it can only carry 10 parcels, so you only take the first 10 after sorting.

`OFFSET` is skipping the first few houses because those streets were already delivered yesterday.

## ORDER BY

Sort results by one or more columns:

```sql
-- Ascending (default, oldest first)
SELECT * FROM books ORDER BY year;

-- Explicit ascending
SELECT * FROM books ORDER BY year ASC;

-- Descending (newest first)
SELECT * FROM books ORDER BY year DESC;
```

## Multiple Sort Columns

When two rows have the same value in the first sort column, the second column breaks the tie:

```sql
-- Sort by year, then alphabetically by title within the same year
SELECT * FROM books ORDER BY year ASC, title ASC;

-- Mixed directions
SELECT * FROM books ORDER BY year DESC, title ASC;
```

## ORDER BY with WHERE

`WHERE` runs **before** `ORDER BY`:

```sql
SELECT * FROM books WHERE year > 1970 ORDER BY year;
```

## LIMIT

Restrict how many rows are returned:

```sql
-- The 3 most recent books
SELECT * FROM books ORDER BY year DESC LIMIT 3;

-- Any 2 books (no ordering — unpredictable)
SELECT * FROM books LIMIT 2;
```

## OFFSET

Skip a number of rows before returning results:

```sql
-- Skip the first 2, return the next 3
SELECT * FROM books ORDER BY year LIMIT 3 OFFSET 2;
```

This is how pagination works: page 1 = `LIMIT 10 OFFSET 0`, page 2 = `LIMIT 10 OFFSET 10`, etc.

## LIMIT with OFFSET Shorthand

PostgreSQL also supports the SQL standard shorthand:

```sql
SELECT * FROM books ORDER BY year LIMIT 3 OFFSET 2;  -- standard
SELECT * FROM books ORDER BY year LIMIT 3, 2;         -- MySQL style, NOT PostgreSQL
```

In PostgreSQL, `LIMIT 3 OFFSET 2` is correct. The comma syntax is MySQL-only.

## Practical Example: Pagination

```sql
-- Page 1: first 5 books (oldest first)
SELECT * FROM books ORDER BY year LIMIT 5 OFFSET 0;

-- Page 2: next 5 books
SELECT * FROM books ORDER BY year LIMIT 5 OFFSET 5;
```

## Order of Execution (Mental Model)

```
FROM -> WHERE -> SELECT -> ORDER BY -> LIMIT / OFFSET
```

This is why you can't use `SELECT` aliases in `WHERE` but you can in `ORDER BY`:

```sql
-- This works:
SELECT title AS book_title FROM books ORDER BY book_title;

-- This does NOT work:
SELECT title AS book_title FROM books WHERE book_title = 'Dune';
-- Error! WHERE runs before SELECT aliases exist.
```

## Summary

| Clause | Purpose | Example |
|--------|---------|---------|
| `ORDER BY col` | Sort ascending | `ORDER BY year` |
| `ORDER BY col DESC` | Sort descending | `ORDER BY year DESC` |
| `ORDER BY a, b` | Sort by a, tie-break by b | `ORDER BY year, title` |
| `LIMIT n` | Return at most n rows | `LIMIT 5` |
| `OFFSET n` | Skip first n rows | `OFFSET 10` |
