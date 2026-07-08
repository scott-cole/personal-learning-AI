# DAY12 — Drill: Aggregate a Sales Database

## Setup

```sql
CREATE TABLE sales (
    id SERIAL PRIMARY KEY,
    product VARCHAR(100) NOT NULL,
    category VARCHAR(50),
    price NUMERIC(10,2) NOT NULL,
    quantity INTEGER NOT NULL,
    sale_date DATE DEFAULT CURRENT_DATE
);

INSERT INTO sales (product, category, price, quantity, sale_date) VALUES
('Laptop', 'Electronics', 1200.00, 2, '2024-01-15'),
('Mouse', 'Electronics', 25.00, 5, '2024-01-15'),
('Desk', 'Furniture', 450.00, 1, '2024-01-16'),
('Chair', 'Furniture', 300.00, 3, '2024-01-16'),
('Monitor', 'Electronics', 350.00, 2, '2024-01-17'),
('Notebook', 'Stationery', 5.00, 20, '2024-01-17'),
('Pen', 'Stationery', 2.00, 100, '2024-01-18'),
(NULL, 'Misc', 10.00, 1, '2024-01-18');
```

## Tasks

### 1. Total number of sales records

Count all rows.

### 2. Total revenue

Calculate total revenue (SUM of price × quantity).

### 3. Average price of products sold

### 4. Most expensive and cheapest product price

Show MIN and MAX price.

### 5. Count non-NULL products

Compare `COUNT(*)` vs `COUNT(product)`.

### 6. Total revenue per category

(Skip this if GROUP BY hasn't clicked yet — we'll do it properly on DAY13.)

### 7. Use FILTER (PostgreSQL-specific)

Count how many sales are in Electronics vs Furniture:

```sql
SELECT
    COUNT(*) FILTER (WHERE category = 'Electronics') AS electronics,
    COUNT(*) FILTER (WHERE category = 'Furniture') AS furniture
FROM sales;
```

## Expected Output for Task 2

```
 total_revenue
---------------
       5225.00
```

## Hints

- `SUM(price * quantity)` for task 2
- `AVG(price)` for task 3
- `COUNT(*)` counts all rows; `COUNT(product)` counts non-NULL products only
