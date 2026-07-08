# DAY04 — Drill: CRUD a Library

## Setup

Use the `library` database. Re-create the books table if needed:

```sql
DROP TABLE IF EXISTS books;
CREATE TABLE books (
    id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    author VARCHAR(100) NOT NULL,
    year INTEGER,
    shelf VARCHAR(10)
);
```

## Tasks

### 1. Insert three new books

Add the following books:

| title | author | year | shelf |
|-------|--------|------|-------|
| Foundation | Isaac Asimov | 1951 | E-01 |
| The Martian | Andy Weir | 2011 | E-02 |
| Project Hail Mary | Andy Weir | 2021 | E-02 |

Use `RETURNING *` to confirm.

### 2. Update a book's shelf

Move "The Martian" to shelf 'F-01'. Return the updated row.

### 3. Update multiple books at once

All books by Andy Weir have a typo in the year. Add 10 years to both.

### 4. Delete a specific book

Delete "Foundation". Return the deleted row's title.

### 5. Insert and delete in sequence

Insert a temp book, then delete it. Use RETURNING to verify.

### 6. Hard mode: Upsert

Insert a book, but if the title already exists, update the year:

```sql
INSERT INTO books (title, author, year, shelf)
VALUES ('Dune', 'Frank Herbert', 2025, 'A-12')
ON CONFLICT (title) DO UPDATE SET year = EXCLUDED.year
RETURNING *;
```

(Requires a UNIQUE constraint on title — create one with `ALTER TABLE books ADD CONSTRAINT unique_title UNIQUE (title);`)

## Hints

- Always check your WHERE clause before hitting Enter
- Use `RETURNING *` to verify what changed
- For multiple updates in task 3, use `SET year = year + 10`
