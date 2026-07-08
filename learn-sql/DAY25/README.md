# DAY25 — Views: Saved Search Results

## Anecdote: Views are Saved Search Results

You're a detective investigating a case. Every day you run the same complex search across multiple databases: suspects with prior warrants, recent credit card transactions, phone records from the last 72 hours. You've written the query once and it was painful — 8 JOINs, 5 subqueries, and a CASE expression.

Instead of typing that nightmare query every time, you save it. Now you just ask your assistant: "Run the 'active suspects' search." That saved query is a **view**.

A `VIEW` is a stored SQL query that you can SELECT from as if it were a table. It doesn't store data itself — it's just a saved query definition. Every time you query the view, the underlying query runs.

## Creating a View

```sql
CREATE VIEW active_loans AS
SELECT
    m.first_name || ' ' || m.last_name AS member_name,
    b.title AS book_title,
    l.borrowed_at,
    l.due_date
FROM loans l
JOIN members m ON l.member_id = m.id
JOIN books b ON l.book_id = b.id
WHERE l.returned_at IS NULL;
```

Now query it like a table:

```sql
SELECT * FROM active_loans WHERE due_date < CURRENT_DATE;
```

## Views Can Use Other Views

```sql
CREATE VIEW overdue_items AS
SELECT * FROM active_loans WHERE due_date < CURRENT_DATE;
```

## Updating Data Through Views

Simple views (one table, no aggregates, no DISTINCT) are **auto-updatable**:

```sql
CREATE VIEW active_products AS
SELECT * FROM products WHERE status = 'active';

INSERT INTO active_products (name, price) VALUES ('New Item', 10.00);
-- This inserts into the underlying products table
```

## WITH CHECK OPTION

Prevents inserting rows through a view that would then disappear from the view:

```sql
CREATE VIEW expensive_products AS
SELECT * FROM products WHERE price > 100
WITH CHECK OPTION;

-- This FAILS (price 50 would not be visible in the view):
INSERT INTO expensive_products (name, price) VALUES ('Cheap', 50);
```

## Materialized Views

A regular view runs its query every time. A **materialized view** stores the results physically:

```sql
CREATE MATERIALIZED VIEW monthly_sales_summary AS
SELECT
    DATE_TRUNC('month', sale_date) AS month,
    product_id,
    SUM(quantity) AS total_qty,
    SUM(quantity * price) AS total_revenue
FROM sales
JOIN products ON sales.product_id = products.id
GROUP BY month, product_id;

-- Refresh when data changes:
REFRESH MATERIALIZED VIEW monthly_sales_summary;
```

Materialized views are great for slow, expensive queries that don't need real-time results (dashboards, weekly reports).

## Dropping Views

```sql
DROP VIEW IF EXISTS active_loans;
DROP MATERIALIZED VIEW IF EXISTS monthly_sales_summary;
```

## Why Use Views?

- **Simplify complex queries** — hide 10-JOIN monstrosities
- **Security** — grant access to a view without exposing underlying tables
- **Consistency** — everyone uses the same definition of "active customer"
- **Abstraction** — change the underlying tables without changing queries

## Summary

| Feature | Regular View | Materialized View |
|---------|-------------|-------------------|
| Stores data? | No (runs query each time) | Yes (snapshot on disk) |
| Always current? | Yes | No (needs REFRESH) |
| Speed | Slow for complex queries | Fast |
| Use case | Simplification, security | Performance, dashboards |
| Can INSERT/UPDATE? | Sometimes (auto-updatable) | No |
