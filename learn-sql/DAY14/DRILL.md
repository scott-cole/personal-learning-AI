# DAY14 — Drill: E-Commerce Project

## Task

Build a complete e-commerce database in `ecommerce_project.sql`.

## Schema

Create these tables with proper types, constraints, and foreign keys:

- **customers** — id, name, email (UNIQUE), joined_at
- **products** — id, name, price (CHECK > 0), category, stock (DEFAULT 0)
- **orders** — id, customer_id (FK), order_date, status (DEFAULT 'pending')
- **order_items** — id, order_id (FK, CASCADE), product_id (FK), quantity (CHECK > 0), unit_price

## Sample Data

Insert:
- 4 customers: Alice, Bob, Charlie, Diana
- 6 products: Laptop ($1200), Mouse ($25), Keyboard ($75), Monitor ($350), Desk ($450), Chair ($300)
- 5+ orders with 8+ order items

## Queries

Write and test these queries. Here's the expected output for key queries:

### Query 3: Top 3 most-ordered products

```
  product  | order_count
-----------+-------------
 Laptop    |           3
 Mouse     |           2
 Monitor   |           2
```

### Query 4: Customers who spent > $500

```
 customer | total_spent
----------+-------------
 Alice    |     1975.00
 Charlie  |      800.00
 Diana    |     1200.00
```

### Query 6: Products never ordered

```
  product
-----------
 Desk
 Chair
```

## Hints

- For query 10 (same category), join products to itself or use subqueries
- `SUM(oi.quantity * oi.unit_price)` for order totals
- `DATE_TRUNC('month', o.order_date)` for monthly grouping
- `COALESCE(SUM(...), 0)` to avoid NULLs
