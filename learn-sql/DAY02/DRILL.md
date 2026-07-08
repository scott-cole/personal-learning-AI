# DAY02 — Drill: Filter the Library

## Setup

Use the `library` database from DAY01 (or re-run the create/insert statements).

## Tasks

Write SQL queries for each of the following:

### 1. Books published before 1970

```sql
-- Expected: Dune (1965), The Left Hand of Darkness (1969)
```

### 2. Books on shelf 'B-07' or 'C-09'

### 3. Books whose title starts with 'The'

### 4. Books with year BETWEEN 1975 AND 1990

### 5. Books by Frank Herbert AND published after 1970

### 6. All books NOT by William Gibson

### 7. Books on shelf 'A-12' with year > 1970

### 8. Books with year = 1965 OR year = 1992, but only show title and year

## Expected Output for Task 5

```
 id |     title      | author_id | year | shelf
----+----------------+-----------+------+-------
  5 | Children of Dune |        1 | 1976 | A-12
```

## Hints

- Use single quotes for strings: `WHERE author = 'Frank Herbert'`
- Use `LIKE 'The%'` for title starting with "The"
- Task 6 needs `<>` operator
- Task 8 selects specific columns after filtering
