# DAY16 — Drill: EXISTS, ANY, ALL

## Setup

```sql
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(200) UNIQUE
);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INTEGER REFERENCES customers(id),
    total NUMERIC(10,2),
    status VARCHAR(20) DEFAULT 'pending'
);

INSERT INTO customers (name, email) VALUES
('Alice', 'alice@email.com'),
('Bob', 'bob@email.com'),
('Charlie', 'charlie@email.com'),
('Diana', 'diana@email.com'),
('Eve', 'eve@email.com');

INSERT INTO orders (customer_id, total, status) VALUES
(1, 150.00, 'shipped'),
(1, 200.00, 'pending'),
(2, 75.00, 'shipped'),
(3, 500.00, 'cancelled'),
(3, 100.00, 'pending'),
(NULL, 50.00, 'pending');
-- Note: Diana has no orders. Eve has no orders.
-- There's an order with no customer.
```

## Tasks

### 1. Customers who have placed at least one order (EXISTS)

### 2. Customers who have never placed an order (NOT EXISTS)

### 3. Orders with total greater than ANY order by customer 3

### 4. Products with price greater than ALL products in 'Stationery' category (use DAY15's products table)

### 5. Show customers and their total spent, but only if they've placed at least one shipped order (EXISTS subquery filtering)

### 6. Find customers who have at least one order with total above the average order total

## Expected Output for Task 2

```
  name   |        email
---------+---------------------
 Diana   | diana@email.com
 Eve     | eve@email.com
```

## Hints

- `EXISTS (SELECT 1 FROM ... WHERE ...)` is the standard pattern
- `NOT EXISTS` is often clearer than `LEFT JOIN ... IS NULL`
- For task 4, combine with the products table from DAY15
- Task 6 needs a nested subquery for the average
