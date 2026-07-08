# DAY21 — Drill: Analytics Project

## Task

Build a sales analytics database in `analytics_project.sql`. Insert data and write 10 queries.

## Setup Data

```sql
INSERT INTO customers (name, email, region) VALUES
('Alice', 'alice@email.com', 'North'),
('Bob', 'bob@email.com', 'South'),
('Charlie', 'charlie@email.com', 'East'),
('Diana', 'diana@email.com', 'West'),
('Eve', 'eve@email.com', 'North'),
('Frank', 'frank@email.com', 'South');

INSERT INTO products (name, category, price, cost) VALUES
('Laptop', 'Electronics', 1200.00, 800.00),
('Mouse', 'Electronics', 25.00, 10.00),
('Keyboard', 'Electronics', 75.00, 40.00),
('Monitor', 'Electronics', 350.00, 200.00),
('Desk', 'Furniture', 450.00, 250.00),
('Chair', 'Furniture', 300.00, 180.00),
('Notebook', 'Stationery', 5.00, 2.00),
('Pen Set', 'Stationery', 12.00, 5.00);
```

Insert 25+ sales across January-March 2024, varying quantities (1-3), occasional discounts (0-15%).

## Expected Output Examples

### Query 1: Monthly revenue

```
  month   | revenue | sales_count
----------+---------+-------------
 2024-01  | 5200.00 |          10
 2024-02  | 3800.00 |           8
 2024-03  | 6100.00 |          11
```

### Query 4: Category performance with %

```
  category   | revenue | pct
-------------+---------+------
 Electronics | 8500.00 | 56.3
 Furniture   | 4500.00 | 29.8
 Stationery  | 2100.00 | 13.9
```

## Hints

- Use `ROUND(revenue / (SELECT SUM(revenue) FROM ...) * 100, 1)` for percentages
- `DISTINCT ON` is PostgreSQL-specific and perfect for finding top per group
- For product affinity, join sales table to itself on `customer_id` and `DATE_TRUNC('month', sale_date)` with different product_ids
- Use `COALESCE` for any NULL results
