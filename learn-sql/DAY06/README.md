# DAY06 — NULL Handling: The Empty Box

## Anecdote: NULL is an Empty Box on a Shelf

Picture a warehouse shelf. Most boxes have content inside — books, tools, electronics. But one box is there, in its proper spot, labelled and sealed... and completely empty. It takes up space. It has a label. But it contains nothing.

That's `NULL`.

`NULL` is not zero. It's not an empty string `''`. It's not `'NULL'` as text. It is the **absence of a value** — a box that exists but holds nothing. And just like you can't compare an empty box to a box containing a book, you can't compare `NULL` using normal operators.

`NULL` is contagious — any operation involving `NULL` produces `NULL`.

## Testing for NULL

You can't use `= NULL` or `<> NULL`. These will **always** be unknown (not true, not false). Instead you must use:

```sql
SELECT * FROM books WHERE year IS NULL;
SELECT * FROM books WHERE year IS NOT NULL;
```

This is one of the most common beginner mistakes in SQL. Memorise it now: **never use = NULL, always use IS NULL**.

## NULL in Arithmetic

Any arithmetic with NULL produces NULL:

```sql
SELECT 10 + NULL;    -- NULL
SELECT 10 * NULL;    -- NULL
SELECT CONCAT('Hello', NULL);  -- NULL (in PostgreSQL, actually 'Hello' — CONCAT skips NULL)
```

Actually, PostgreSQL's `CONCAT` and `CONCAT_WS` skip NULLs, which is a special case. But regular `||` concatenation:

```sql
SELECT 'Hello' || NULL || 'World';  -- NULL
```

## COALESCE — Replace NULL with a Default

```sql
SELECT COALESCE(year, 0) AS year_or_zero FROM books;
-- Returns year if not NULL, otherwise 0

SELECT COALESCE(shelf, 'UNKNOWN') FROM books;
```

`COALESCE` takes any number of arguments and returns the **first non-NULL** value:

```sql
SELECT COALESCE(NULL, NULL, 'found', NULL);  -- 'found'
```

## NULLIF — Conditional NULL

`NULLIF(a, b)` returns NULL if `a = b`, otherwise returns `a`. Useful for avoiding division-by-zero:

```sql
-- Avoid division by zero
SELECT 100 / NULLIF(quantity, 0) FROM inventory;

-- Convert empty strings to NULL
SELECT NULLIF(column, '') FROM table;
```

## NULL in WHERE Conditions

NULL behaves differently in WHERE:

```sql
-- These are NOT the same:
SELECT * FROM books WHERE year <> 1984;
-- Excludes books where year IS NULL because NULL <> 1984 is NULL (not TRUE)

SELECT * FROM books WHERE year <> 1984 OR year IS NULL;
-- Includes books where year IS NULL
```

## NULL in Unique Constraints

In PostgreSQL, multiple NULLs are allowed in a UNIQUE column:

```sql
CREATE TABLE test (name VARCHAR UNIQUE);
INSERT INTO test VALUES (NULL);  -- OK
INSERT INTO test VALUES (NULL);  -- Also OK! Multiple NULLs allowed
```

## NULL in ORDER BY

By default, NULLs sort after non-NULLs in ascending order:

```sql
SELECT * FROM books ORDER BY year;
-- NULLs appear last

SELECT * FROM books ORDER BY year NULLS FIRST;
-- Override: NULLs first
```

## Summary

| Concept | Expression | Notes |
|---------|-----------|-------|
| Check NULL | `col IS NULL` | Never use `= NULL` |
| Check not NULL | `col IS NOT NULL` | Never use `<> NULL` |
| First non-NULL | `COALESCE(a, b, c)` | Returns first non-NULL |
| Conditionally NULL | `NULLIF(a, b)` | NULL if a = b, else a |
| Sort NULLs first | `ORDER BY col NULLS FIRST` | Override default |
