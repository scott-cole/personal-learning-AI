# DAY20 — CASE Expressions and Conditional Logic

## Anecdote: CASE is a Choose-Your-Own-Adventure Book

Remember choose-your-own-adventure books? "If you enter the cave, turn to page 42. If you climb the mountain, turn to page 87. If you go back to town, turn to page 12."

`CASE` is exactly that: a series of conditions checked in order. The first matching condition wins. It's SQL's way of doing "if-else if-else" logic.

Unlike programming languages where if/else controls flow, `CASE` is an **expression** that returns a value. You use it inside SELECT, WHERE, ORDER BY, or anywhere you'd use a column.

## Simple CASE (Exact Match)

```sql
SELECT
    title,
    CASE shelf
        WHEN 'A-12' THEN 'Fiction Aisle'
        WHEN 'B-07' THEN 'Sci-Fi Aisle'
        WHEN 'C-09' THEN 'Fantasy Aisle'
        ELSE 'Other'
    END AS location
FROM books;
```

Each WHEN compares the expression (shelf) against a value.

## Searched CASE (Boolean Conditions)

More flexible — you can use comparisons:

```sql
SELECT
    title,
    year,
    CASE
        WHEN year < 1950 THEN 'Vintage'
        WHEN year < 1980 THEN 'Classic'
        WHEN year < 2010 THEN 'Modern'
        ELSE 'Contemporary'
    END AS era
FROM books;
```

## CASE in WHERE

```sql
SELECT * FROM products
ORDER BY
    CASE
        WHEN stock = 0 THEN 1  -- Out of stock first
        ELSE 0
    END,
    price;
```

## CASE in GROUP BY

```sql
SELECT
    CASE
        WHEN price < 50 THEN 'Budget'
        WHEN price < 500 THEN 'Mid-range'
        ELSE 'Premium'
    END AS price_tier,
    COUNT(*) AS product_count,
    AVG(price) AS avg_price
FROM products
GROUP BY price_tier;
```

## CASE for Pivoting (Row to Column)

```sql
SELECT
    DATE_TRUNC('month', order_date) AS month,
    COUNT(*) FILTER (WHERE status = 'shipped') AS shipped,
    COUNT(*) FILTER (WHERE status = 'pending') AS pending,
    COUNT(*) FILTER (WHERE status = 'cancelled') AS cancelled
FROM orders
GROUP BY month;
```

## CASE and COALESCE Deep Dive

`COALESCE` is essentially a special CASE:

```sql
-- These are equivalent:
COALESCE(a, b, c)
CASE WHEN a IS NOT NULL THEN a
     WHEN b IS NOT NULL THEN b
     ELSE c
END
```

## NULLIF Deep Dive

`NULLIF(a, b)` is also a CASE:

```sql
-- Equivalent:
NULLIF(a, b)
CASE WHEN a = b THEN NULL ELSE a END
```

## Nested CASE

```sql
SELECT
    name,
    CASE
        WHEN role = 'Admin' THEN
            CASE WHEN tenant_id = 1 THEN 'Super Admin'
                 ELSE 'Tenant Admin'
            END
        WHEN role = 'User' THEN 'Standard User'
        ELSE 'Guest'
    END AS access_level
FROM users;
```

## Practical Pattern: Bucketing

```sql
SELECT
    CASE
        WHEN total_spent < 100 THEN 'Bronze'
        WHEN total_spent < 500 THEN 'Silver'
        WHEN total_spent < 1000 THEN 'Gold'
        ELSE 'Platinum'
    END AS tier,
    COUNT(*) AS customers
FROM (
    SELECT customer_id, SUM(total) AS total_spent
    FROM orders GROUP BY customer_id
) AS customer_totals
GROUP BY tier;
```

## Summary

| Feature | Description | Example |
|---------|-------------|---------|
| Simple CASE | Match exact value | `CASE col WHEN 1 THEN 'a' END` |
| Searched CASE | Boolean conditions | `CASE WHEN col > 5 THEN 'big' END` |
| ELSE | Default if no match | `ELSE 'other'` |
| CASE in ORDER BY | Conditional sorting | `ORDER BY CASE WHEN ... THEN 0 ELSE 1 END` |
| CASE in GROUP BY | Conditional grouping | `GROUP BY CASE WHEN ... THEN ... END` |
