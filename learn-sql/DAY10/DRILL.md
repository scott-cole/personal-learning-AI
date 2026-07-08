# DAY10 — Drill: Master Outer Joins

## Setup

```sql
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INTEGER REFERENCES customers(id),
    total NUMERIC(10,2),
    order_date DATE DEFAULT CURRENT_DATE
);

INSERT INTO customers (name) VALUES
('Alice'), ('Bob'), ('Charlie'), ('Diana');

INSERT INTO orders (customer_id, total) VALUES
(1, 100.00), (1, 50.00), (2, 75.00), (NULL, 25.00);
-- Note: customer 3 (Charlie) has no orders
-- Note: order 4 has no customer
```

## Tasks

### 1. LEFT JOIN — all customers and their orders

Show every customer name and their order totals. Charlie should appear even without orders.

### 2. Find customers with no orders

Use LEFT JOIN with WHERE to find customers who have never placed an order.

### 3. RIGHT JOIN — all orders with customer names

Show every order with customer name. The order with no customer should still appear.

### 4. FULL OUTER JOIN — everything

Write a FULL OUTER JOIN between customers and orders.

### 5. Challenge: COALESCE with LEFT JOIN

Show customer names and their total spent. Use COALESCE so customers with no orders show 0.00 instead of NULL.

### 6. Challenge: Count orders per customer using LEFT JOIN

Show each customer name and the count of their orders. Customers with 0 orders should show 0.

## Expected Output for Task 5

```
 name   | total_spent
--------+-------------
 Alice  |      150.00
 Bob    |       75.00
 Charlie|        0.00
 Diana  |        0.00
```

## Hints

- `COALESCE(SUM(o.total), 0)` for task 5
- `LEFT JOIN` then `WHERE right_table.id IS NULL` to find missing rows
- Use `GROUP BY c.name` for task 6
