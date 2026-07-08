# DAY01 — What is SQL? Databases, Tables, Basic SELECT *

## Anecdote: SQL is a Library Catalogue

Imagine you walk into a massive library. Millions of books, but no sign, no computer, no helpful librarian — just floor-to-ceiling shelves. How do you find anything? You can't. That's a database without SQL.

Now imagine that same library has a **card catalogue**. Every book is recorded: its title, author, shelf location, publication year. You walk up to the catalogue, flip through the drawers, and instantly know exactly where to go. That catalogue is SQL.

A **database** is the library building itself. **Tables** are the shelves or sections. **Rows** are individual books. **Columns** are the facts about each book (title, author, etc.). And `SELECT *` is your way of saying *"show me everything on this shelf."*

SQL (Structured Query Language) is the language you speak to the catalogue. It's been around since the 1970s and is the standard way to talk to relational databases. PostgreSQL is one of the most powerful open-source SQL databases — and the one we'll use throughout this course.

## What is a Database?

A **database** is a structured collection of data. Think of it as a set of spreadsheets that can talk to each other. PostgreSQL is a **relational database management system (RDBMS)** — it stores data in tables that relate to one another through shared columns.

## What is a Table?

A **table** is like a spreadsheet:

```
 id |  title   | author     | year | shelf
----+----------+------------+------+-------
  1 | Dune     | Frank Herbert | 1965 | A-12
  2 | Neuromancer | William Gibson | 1984 | B-07
```

- **Columns** define the type of data (id, title, author, etc.)
- **Rows** are individual records (one book)

## Basic SELECT *

`SELECT` is the most common SQL command. It retrieves data from a table. The asterisk `*` means "all columns."

```sql
SELECT * FROM books;
```

This returns every row and every column from the `books` table — like dumping the entire card catalogue onto the counter.

You can also select specific columns:

```sql
SELECT title, author FROM books;
```

## Creating a Table

Before you can SELECT, you need a table. Here's how you create one in PostgreSQL:

```sql
CREATE TABLE books (
    id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    author VARCHAR(100) NOT NULL,
    year INTEGER,
    shelf VARCHAR(10)
);
```

Don't worry about the data types yet — we'll cover those on DAY05. For now, just know that `SERIAL` auto-increments, `VARCHAR` holds text, and `INTEGER` holds whole numbers.

## Inserting Sample Data

```sql
INSERT INTO books (title, author, year, shelf) VALUES
('Dune', 'Frank Herbert', 1965, 'A-12'),
('Neuromancer', 'William Gibson', 1984, 'B-07'),
('Snow Crash', 'Neal Stephenson', 1992, 'A-03'),
('The Left Hand of Darkness', 'Ursula K. Le Guin', 1969, 'C-09');
```

## Your First Queries

```sql
-- Show everything
SELECT * FROM books;

-- Show only titles and authors
SELECT title, author FROM books;

-- Show all books sorted by year (we'll learn ORDER BY properly on DAY03)
SELECT * FROM books ORDER BY year;
```

## Comment Syntax

In SQL, comments start with `--`:

```sql
-- This is a single-line comment
```

## The `psql` Client

If you're using the `psql` command-line tool:

```sql
-- Connect to a database
\c mydatabase

-- List all tables
\dt

-- Describe a table structure
\d books

-- Quit psql
\q
```

## Summary

| Concept | Description |
|---------|-------------|
| Database | A structured collection of data (the library) |
| Table | A set of rows and columns (a shelf section) |
| Row | A single record (a book) |
| Column | A single attribute (title, author) |
| `SELECT * FROM table` | Retrieve all columns and rows |
| `SELECT col1, col2 FROM table` | Retrieve specific columns |
| `CREATE TABLE` | Define a new table structure |
| `INSERT INTO` | Add data to a table |
