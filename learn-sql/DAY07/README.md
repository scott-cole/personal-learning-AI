# DAY07 — Project: Design and Query a Library Database

## Week 1 Project

You've spent six days learning the foundations of SQL. Now it's time to build something real from scratch — a complete library database.

This project simulates a real-world task: given a description of what's needed, you design the schema, write the queries, and produce reports.

## Scenario

Your local community library has hired you to build a database. They need to track:

- **Members** — who borrows books (name, membership date, contact info)
- **Books** — what's in the catalogue (title, author, publication year, genre)
- **Loans** — who borrowed what and when (member, book, borrow date, due date, return date)

## Your Mission

### Step 1: Design the schema

Create three tables with appropriate data types, primary keys, and relationships. Think about:

- What columns does each table need?
- What data types should each column be?
- How do the tables relate?
- What should be NOT NULL?
- What should have defaults?

Here's a starting outline:

```sql
CREATE TABLE members (
    id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    joined_at DATE DEFAULT CURRENT_DATE
);

-- You design books and loans tables
```

### Step 2: Insert sample data

Insert at least:
- 5 members
- 8 books
- 10 loans (mix of returned and still-out books)

### Step 3: Write these queries

1. List all books with their authors, sorted by title
2. List all currently borrowed books (not returned yet) with member names
3. Find the most borrowed book (the one that appears most in loans)
4. Show each member and how many books they've borrowed
5. Find overdue books (due date passed and not returned)
6. Show books that have never been borrowed
7. List members who joined in the last 30 days

### Step 4: Challenge queries

8. Find the member(s) who borrowed the most books
9. Show the average number of days books are kept before return
10. List books currently on loan with the member's name and how many days they've had it

## Hints

- Use `LEFT JOIN` for books that have never been borrowed (query 6)
- Use `COUNT(*)` and `GROUP BY` for frequency queries (3, 4)
- Use `CURRENT_DATE` for date arithmetic
- The loans table should have foreign keys referencing both members and books

## What to Submit

For this project day, write all your SQL into a single `.sql` file called `library_project.sql`. Make sure it runs from top to bottom without errors.

When you're done, `\i library_project.sql` in psql to test.
