# DAY03 — Drill: Sort and Paginate

## Setup

Use the `library` database with the books table from DAY01.

## Tasks

### 1. List all books sorted by year (newest first)

### 2. List all books sorted by shelf location, then by year (oldest first)

### 3. Show the 3 oldest books

### 4. Show books 3 and 4 when sorted by year (oldest first) — use LIMIT and OFFSET

### 5. Combine WHERE, ORDER BY, and LIMIT: books on shelf 'A-12' sorted by year, show the most recent 2

### 6. Paginate: write a query that would return page 2 if each page has 2 items (sorted by year ASC)

## Example Data

```
 id |        title         | author_id | year | shelf
----+----------------------+-----------+------+-------
  1 | Dune                 |         1 | 1965 | A-12
  2 | Neuromancer          |         2 | 1984 | B-07
  3 | Snow Crash           |         3 | 1992 | A-03
  4 | The Left Hand of Darkness |    4 | 1969 | C-09
  5 | Children of Dune     |         1 | 1976 | A-12
```

## Expected Output for Task 3

```
 id |        title         | author_id | year | shelf
----+----------------------+-----------+------+-------
  1 | Dune                 |         1 | 1965 | A-12
  4 | The Left Hand of Darkness |    4 | 1969 | C-09
  5 | Children of Dune     |         1 | 1976 | A-12
```

## Hints

- `OFFSET` without `LIMIT` works but is unusual
- Remember: `LIMIT` goes **after** `ORDER BY`
- For task 6, if page size is 2, page 2 = skip 2, take 2
