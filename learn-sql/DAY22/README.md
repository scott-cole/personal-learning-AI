# DAY22 — Indexes: The Book's Table of Contents

## Anecdote: Indexes are a Book's Table of Contents

You're holding a 1000-page textbook. You need to find the section on "Window Functions." You could start at page 1 and flip through every page until you reach it — that's a **sequential scan**. It works, but it's slow.

Alternatively, you flip to the **table of contents** at the front, find "Window Functions" listed on page 847, and turn directly there. That's an **index scan** — instant lookup.

A database **index** is exactly a table of contents. It's a separate data structure (a B-tree, by default) that maps column values to their physical locations on disk. Without an index, PostgreSQL reads every row in the table (sequential scan). With a suitable index, it jumps directly to the matching rows.

Indexes make reads fast but writes slightly slower (the index must be updated on every INSERT/UPDATE/DELETE).

## Creating an Index

```sql
CREATE INDEX idx_books_year ON books (year);
```

This creates a B-tree index on the `year` column. Queries filtering or sorting by `year` will now use the index.

## B-Tree Index (Default)

The default index type. Works for:
- `=`, `>`, `<`, `>=`, `<=`
- `BETWEEN`, `IN`
- `ORDER BY`
- `LIKE 'prefix%'` (but not `LIKE '%suffix'`)

```sql
CREATE INDEX idx_books_title ON books (title);
```

## Unique Index

Automatically created for PRIMARY KEY and UNIQUE constraints. You can create one explicitly:

```sql
CREATE UNIQUE INDEX idx_books_isbn ON books (isbn);
```

## Composite Index (Multiple Columns)

```sql
CREATE INDEX idx_books_author_year ON books (author_id, year);
```

Column order matters: put the most selective column first (the one with the most distinct values).

## Partial Index (Index on a Subset)

```sql
CREATE INDEX idx_books_recent ON books (year) WHERE year > 2000;
```

The index only contains rows where year > 2000. Smaller index, faster queries on recent books.

## Index-Only Scan

If all needed columns are in the index, PostgreSQL never touches the table:

```sql
CREATE INDEX idx_books_title_year ON books (title, year);

-- This query might use an index-only scan:
SELECT title, year FROM books WHERE title = 'Dune';
```

## Checking Index Usage

```sql
-- Before running a query, check the plan:
EXPLAIN SELECT * FROM books WHERE year = 1965;

-- Sequential scan (slow):
-- Seq Scan on books  (cost=0.00..12.00 rows=1 width=100)

-- After adding index:
CREATE INDEX idx_books_year ON books (year);

EXPLAIN SELECT * FROM books WHERE year = 1965;
-- Index Scan using idx_books_year  (cost=0.00..4.00 rows=1 width=100)
```

## When NOT to Index

- Small tables (sequential scan is fine)
- Columns with few distinct values (boolean, status with 3 values)
- Columns rarely used in WHERE or JOIN
- Tables with heavy writes (index maintenance cost)

## Index Maintenance

```sql
-- Remove an index
DROP INDEX idx_books_year;

-- Rebuild (rarely needed)
REINDEX INDEX idx_books_year;

-- List all indexes
SELECT * FROM pg_indexes WHERE tablename = 'books';
```

## Summary

| Index Type | Use Case | Syntax |
|-----------|----------|--------|
| B-tree (default) | General purpose, equality + range | `CREATE INDEX ON t (col)` |
| Unique | Enforce uniqueness | `CREATE UNIQUE INDEX ON t (col)` |
| Composite | Multi-column queries | `CREATE INDEX ON t (a, b)` |
| Partial | Index only subset of rows | `CREATE INDEX ON t (col) WHERE condition` |
| Expression | Index a function result | `CREATE INDEX ON t (LOWER(col))` |
