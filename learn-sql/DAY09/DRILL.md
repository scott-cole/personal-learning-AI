# DAY09 — Drill: Join Books and Authors

## Setup

```sql
CREATE TABLE authors (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    nationality VARCHAR(50)
);

CREATE TABLE books (
    id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    author_id INTEGER REFERENCES authors(id),
    year INTEGER
);

INSERT INTO authors (name, nationality) VALUES
('Frank Herbert', 'American'),
('William Gibson', 'American-Canadian'),
('Ursula K. Le Guin', 'American'),
('Haruki Murakami', 'Japanese');

INSERT INTO books (title, author_id, year) VALUES
('Dune', 1, 1965),
('Neuromancer', 2, 1984),
('The Left Hand of Darkness', 3, 1969),
('Norwegian Wood', 4, 1987),
('Children of Dune', 1, 1976);
-- Note: No books for author_id=NULL, no new author data
```

## Tasks

### 1. Basic INNER JOIN

Write a query that shows each book title alongside its author's name.

### 2. Add nationality to the output

Show book title, author name, and author nationality.

### 3. Filter after join

Show only books by American authors, with title and year.

### 4. 3-table join (add loans)

```sql
CREATE TABLE loans (
    id SERIAL PRIMARY KEY,
    book_id INTEGER REFERENCES books(id),
    member_name VARCHAR(100),
    borrowed_at DATE DEFAULT CURRENT_DATE
);

INSERT INTO loans (book_id, member_name) VALUES
(1, 'Alice'), (2, 'Bob'), (1, 'Charlie');
```

Write a query showing book title, author name, and member name for each loan.

### 5. Count books per author (using GROUP BY with JOIN)

Show each author's name and how many books they have.

## Expected Output for Task 5

```
      name       | count
------------------+-------
 Frank Herbert    |     2
 William Gibson   |     1
 Ursula K. Le Guin|     1
 Haruki Murakami  |     1
```

## Hints

- Use `b` and `a` as aliases
- For task 3, add `WHERE a.nationality = 'American'`
- For task 5: `GROUP BY a.name`
