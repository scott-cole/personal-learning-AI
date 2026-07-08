# DAY28 — CTEs: Sticky Notes for Complex Queries

## Anecdote: CTEs are Sticky Notes for Complex Multi-Step Queries

You're assembling IKEA furniture. The instructions have 47 steps. You could try to do them all in your head, but instead you break them into phases: "Phase 1: build the frame," "Phase 2: attach the drawers," "Phase 3: mount the doors."

A **CTE (Common Table Expression)** — defined with the `WITH` clause — is like writing each phase on a sticky note and posting it on the wall. Each note is a named subquery you can refer to later. The final SELECT at the end pulls everything together.

CTEs make complex queries readable. Instead of nested subqueries 5 levels deep, you write a series of CTEs with clear names like `top_customers`, `monthly_totals`, or `overdue_items`.

## Basic CTE

```sql
WITH recent_books AS (
    SELECT * FROM books WHERE year > 2000
)
SELECT * FROM recent_books WHERE shelf = 'A-12';
```

## Multiple CTEs

```sql
WITH
    book_counts AS (
        SELECT author_id, COUNT(*) AS book_count
        FROM books GROUP BY author_id
    ),
    active_authors AS (
        SELECT a.id, a.name, bc.book_count
        FROM authors a
        JOIN book_counts bc ON a.id = bc.author_id
        WHERE bc.book_count >= 2
    )
SELECT * FROM active_authors ORDER BY book_count DESC;
```

## CTE vs Subquery

These are equivalent:

```sql
-- With CTE:
WITH avg_price AS (
    SELECT AVG(price) AS avg FROM products
)
SELECT * FROM products WHERE price > (SELECT avg FROM avg_price);

-- With subquery:
SELECT * FROM products WHERE price > (SELECT AVG(price) FROM products);
```

CTEs are often clearer for multiple steps.

## Recursive CTEs

A CTE that references itself — essential for hierarchical data:

```sql
WITH RECURSIVE org_chart AS (
    -- Base case: the CEO (no manager)
    SELECT id, name, manager_id, 1 AS level
    FROM employees
    WHERE manager_id IS NULL

    UNION ALL

    -- Recursive step: employees whose manager is in the result
    SELECT e.id, e.name, e.manager_id, oc.level + 1
    FROM employees e
    JOIN org_chart oc ON e.manager_id = oc.id
)
SELECT * FROM org_chart ORDER BY level, id;
```

This produces:
```
 id |  name   | manager_id | level
----+---------+------------+-------
  1 | Alice   |     NULL   |     1
  2 | Bob     |      1     |     2
  3 | Charlie |      1     |     2
  4 | Diana   |      2     |     3
```

## Recursive CTE for Number Sequences

```sql
WITH RECURSIVE numbers AS (
    SELECT 1 AS n
    UNION ALL
    SELECT n + 1 FROM numbers WHERE n < 10
)
SELECT * FROM numbers;
```

## Recursive CTE for Date Ranges

```sql
WITH RECURSIVE dates AS (
    SELECT '2024-01-01'::DATE AS dt
    UNION ALL
    SELECT dt + 1 FROM dates WHERE dt < '2024-01-31'
)
SELECT * FROM dates;
```

## Modifying Data with CTEs

CTEs can be used with INSERT, UPDATE, DELETE:

```sql
WITH deleted AS (
    DELETE FROM orders WHERE status = 'cancelled'
    RETURNING *
)
INSERT INTO orders_archive SELECT * FROM deleted;
```

## CTE Materialization

In PostgreSQL, CTEs are **optimization fences** — they're always materialized (executed once). This can be good (avoid repeated execution) or bad (prevents pushing predicates into the CTE). Since PostgreSQL 12, you can add `NOT MATERIALIZED`:

```sql
WITH cheap_products AS NOT MATERIALIZED (
    SELECT * FROM products WHERE price < 50
)
SELECT * FROM cheap_products WHERE category = 'Electronics';
```

## Summary

| Feature | Description | Example |
|---------|-------------|---------|
| Simple CTE | Named subquery | `WITH t AS (SELECT ...) SELECT * FROM t` |
| Multiple CTEs | Comma-separated | `WITH a AS (...), b AS (...)` |
| Recursive CTE | Self-referencing | `WITH RECURSIVE t AS (...)` |
| Modifying CTE | INSERT/UPDATE/DELETE with CTE | `WITH d AS (DELETE ... RETURNING *)` |
| NOT MATERIALIZED | Inline the CTE (PG 12+) | `WITH t AS NOT MATERIALIZED (...)` |
