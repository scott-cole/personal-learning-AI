# DAY01 — Drill: Explore a Library Database

## Setup

Create a database called `library`:

```sql
CREATE DATABASE library;
\c library
```

## Task

You are building a catalogue for a small library. Create the `books` table and a new `authors` table, insert data, and run SELECT queries.

### Step 1: Create tables

```sql
CREATE TABLE authors (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    birth_year INTEGER
);

CREATE TABLE books (
    id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    author_id INTEGER,
    year INTEGER,
    shelf VARCHAR(10)
);
```

### Step 2: Insert data

```sql
INSERT INTO authors (name, birth_year) VALUES
('Frank Herbert', 1920),
('William Gibson', 1948),
('Neal Stephenson', 1959),
('Ursula K. Le Guin', 1929);

INSERT INTO books (title, author_id, year, shelf) VALUES
('Dune', 1, 1965, 'A-12'),
('Neuromancer', 2, 1984, 'B-07'),
('Snow Crash', 3, 1992, 'A-03'),
('The Left Hand of Darkness', 4, 1969, 'C-09'),
('Children of Dune', 1, 1976, 'A-12');
```

### Step 3: Write and run these queries

1. Select all columns from `books`.
2. Select only `title` and `year` from `books`.
3. From `authors`, select all names.
4. Select all books and order them by `year` (ascending).
5. Select all books that are on shelf `'A-12'`.

## Expected Output

For query 5, you should get:

```
 id |     title      | author_id | year | shelf
----+----------------+-----------+------+-------
  1 | Dune           |         1 | 1965 | A-12
  5 | Children of Dune |       1 | 1976 | A-12
```

## Hints

- Use `SELECT * FROM books WHERE shelf = 'A-12';` for query 5.
- String comparisons in PostgreSQL use single quotes.
- Semicolons terminate SQL statements — don't forget them.
