# DAY15 — Subqueries: Nesting Queries Inside Queries

## Anecdote: Subqueries are Russian Nesting Dolls

A Russian nesting doll (matryoshka) opens to reveal a smaller doll inside. That doll opens to reveal an even smaller one. Each doll is complete on its own, but it lives inside a larger one.

A **subquery** is the same: a complete SQL query nested inside another SQL query. The inner query runs first, produces a result, and the outer query uses that result.

Just as you can have 3, 5, or 7 layers of dolls, you can have subqueries in `SELECT`, `FROM`, `WHERE`, `HAVING`, and even inside other subqueries.

## Subqueries in WHERE (Most Common)

Find books by authors born after 1950:

```sql
SELECT title FROM books
WHERE author_id IN (
    SELECT id FROM authors WHERE birth_year > 1950
);
```

The inner query runs first: `SELECT id FROM authors WHERE birth_year > 1950` returns `{2, 3}`.
Then the outer query becomes: `SELECT title FROM books WHERE author_id IN (2, 3)`.

## Scalar Subquery (Returns One Value)

```sql
-- Find books with price above average
SELECT title, price FROM products
WHERE price > (SELECT AVG(price) FROM products);
```

The inner query returns a single number (e.g., 392.50). The outer query uses it in a comparison.

## Row Subquery (Returns One Row)

```sql
-- Find the product with the highest price
SELECT * FROM products
WHERE (category, price) = (
    SELECT category, MAX(price) FROM products GROUP BY category LIMIT 1
);
```

## Table Subquery in FROM

You can put a subquery in the `FROM` clause — it becomes a derived table:

```sql
SELECT category, avg_price
FROM (
    SELECT category, AVG(price) AS avg_price
    FROM products
    GROUP BY category
) AS category_stats
WHERE avg_price > 200;
```

The derived table must have an alias (`category_stats`).

## Subqueries in SELECT

A scalar subquery can appear in the SELECT list:

```sql
SELECT
    title,
    price,
    (SELECT AVG(price) FROM products) AS avg_price,
    price - (SELECT AVG(price) FROM products) AS difference
FROM products;
```

## Correlated Subqueries

The inner query references the outer query:

```sql
-- Find products whose price is above the average for their category
SELECT p1.title, p1.category, p1.price
FROM products p1
WHERE p1.price > (
    SELECT AVG(p2.price)
    FROM products p2
    WHERE p2.category = p1.category
);
```

For each product, the inner query computes the average price for that product's category. This runs once per row — can be slow on large tables.

## Subquery vs JOIN

Many subqueries can be rewritten as JOINs:

```sql
-- Same result with subquery:
SELECT * FROM books WHERE author_id IN (SELECT id FROM authors WHERE birth_year > 1950);

-- Same result with JOIN:
SELECT books.* FROM books
INNER JOIN authors ON books.author_id = authors.id
WHERE authors.birth_year > 1950;
```

## Summary

| Subquery Location | Returns | Example |
|------------------|---------|---------|
| `WHERE col IN (subquery)` | Set of values | `WHERE id IN (SELECT id FROM ...)` |
| `WHERE col = (subquery)` | Scalar (one value) | `WHERE price > (SELECT AVG(...))` |
| `FROM (subquery) AS alias` | Table (derived table) | `FROM (SELECT ...) AS t` |
| `SELECT (subquery)` | Scalar | `SELECT ..., (SELECT ...) AS col` |
| Correlated | Varies | References outer query |
