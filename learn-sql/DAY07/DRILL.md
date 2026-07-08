# DAY07 — Drill: Build the Library Project

## Task

Build a complete library database from scratch. Write all SQL into `library_project.sql`.

## Requirements

### Tables

**members**
| Column | Type | Constraints |
|--------|------|-------------|
| id | SERIAL | PRIMARY KEY |
| first_name | VARCHAR(50) | NOT NULL |
| last_name | VARCHAR(50) | NOT NULL |
| email | VARCHAR(100) | UNIQUE, NOT NULL |
| joined_at | DATE | DEFAULT CURRENT_DATE |

**books**
| Column | Type | Constraints |
|--------|------|-------------|
| id | SERIAL | PRIMARY KEY |
| title | VARCHAR(200) | NOT NULL |
| author | VARCHAR(100) | NOT NULL |
| genre | VARCHAR(50) | |
| published_year | INTEGER | |
| isbn | VARCHAR(13) | UNIQUE |

**loans**
| Column | Type | Constraints |
|--------|------|-------------|
| id | SERIAL | PRIMARY KEY |
| member_id | INTEGER | FK → members(id) |
| book_id | INTEGER | FK → books(id) |
| borrowed_at | DATE | NOT NULL DEFAULT CURRENT_DATE |
| due_date | DATE | NOT NULL |
| returned_at | DATE | |

### Sample Data

Insert at least 5 members, 8 books, and 10 loans. Mix returned and unreturned loans.

### Queries to Write

Write and run these queries:

1. All books sorted by title:
   ```
    title      |   author    | genre
   ------------+-------------+---------
   1984        | George Orwell | Fiction
   Dune        | Frank Herbert | Sci-Fi
   ...
   ```

2. Currently borrowed books (returned_at IS NULL) with member names

3. Most borrowed book — title and borrow count

4. Each member and their borrow count, ordered by count descending

5. Overdue books: WHERE due_date < CURRENT_DATE AND returned_at IS NULL

6. Books never borrowed (use LEFT JOIN and IS NULL)

7. Members who joined in the last 30 days

## Hints

- Create tables in order of dependencies (no forward references)
- Insert members and books before loans
- Use `RETURNING *` after inserts to verify
- For query 6: `LEFT JOIN loans ON books.id = loans.book_id WHERE loans.id IS NULL`
