# DAY12 — Aggregate Functions: COUNT, SUM, AVG, MIN, MAX

## Anecdote: Aggregates are a Cashier's End-of-Day Summary

A cashier at a grocery store doesn't care about individual transactions at the end of the day. They care about:
- How many customers came through? (**COUNT**)
- What was the total revenue? (**SUM**)
- What was the average basket size? (**AVG**)
- What was the biggest single purchase? (**MAX**)
- What was the smallest? (**MIN**)

These are **aggregate functions**. They take many rows and boil them down to a single number. Instead of showing you every sale, they summarize.

In SQL, aggregate functions operate on a **column** of values and return a single value.

## COUNT

```sql
-- Count all rows
SELECT COUNT(*) FROM books;

-- Count non-NULL values in a column
SELECT COUNT(shelf) FROM books;  -- Excludes rows where shelf is NULL

-- Count distinct values
SELECT COUNT(DISTINCT author) FROM books;

-- Count with WHERE
SELECT COUNT(*) FROM books WHERE year > 2000;
```

## SUM

```sql
-- Total of all prices
SELECT SUM(price) FROM products;

-- Total with filter
SELECT SUM(quantity) FROM order_items WHERE order_id = 5;
```

## AVG

```sql
-- Average price
SELECT AVG(price) FROM products;

-- Average with rounding
SELECT ROUND(AVG(price), 2) FROM products;
```

## MIN and MAX

```sql
-- Earliest and latest dates
SELECT MIN(hire_date), MAX(hire_date) FROM employees;

-- Cheapest and most expensive
SELECT MIN(price), MAX(price) FROM products;
```

## Multiple Aggregates in One Query

```sql
SELECT
    COUNT(*) AS total_books,
    MIN(year) AS oldest,
    MAX(year) AS newest,
    AVG(year) AS avg_year
FROM books;
```

## NULL Handling

Aggregates generally ignore NULLs:

```sql
-- If quantity has NULLs, SUM still works (ignores them)
SELECT SUM(quantity) FROM order_items;

-- COUNT(column) ignores NULLs; COUNT(*) doesn't
SELECT COUNT(shelf) FROM books;   -- Counts non-NULL shelves
SELECT COUNT(*) FROM books;      -- Counts all rows
```

## SUM with CASE (Conditional Aggregation)

```sql
-- Count books in specific shelves using CASE
SELECT
    COUNT(*) FILTER (WHERE shelf = 'A-12') AS a12_count,
    COUNT(*) FILTER (WHERE shelf LIKE 'B%') AS b_shelf_count
FROM books;
```

PostgreSQL also supports the `FILTER` clause for more readable conditional aggregation.

## DISTINCT with Aggregates

```sql
SELECT
    COUNT(DISTINCT author) AS unique_authors,
    COUNT(*) AS total_books
FROM books;
```

## Summary

| Function | Purpose | NULL Handling |
|----------|---------|---------------|
| `COUNT(*)` | Count all rows | Counts everything |
| `COUNT(col)` | Count non-NULL values | Ignores NULLs |
| `SUM(col)` | Total of values | Ignores NULLs |
| `AVG(col)` | Average of values | Ignores NULLs |
| `MIN(col)` | Smallest value | Ignores NULLs |
| `MAX(col)` | Largest value | Ignores NULLs |
