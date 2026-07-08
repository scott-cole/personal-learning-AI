# DAY20 — Drill: CASE Conditional Logic

## Setup

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer VARCHAR(100) NOT NULL,
    total NUMERIC(10,2) NOT NULL,
    status VARCHAR(20) NOT NULL,
    order_date DATE DEFAULT CURRENT_DATE
);

INSERT INTO orders (customer, total, status, order_date) VALUES
('Alice', 1500.00, 'shipped', '2024-01-15'),
('Bob', 250.00, 'pending', '2024-01-16'),
('Charlie', 75.00, 'cancelled', '2024-01-17'),
('Alice', 3000.00, 'shipped', '2024-01-20'),
('Bob', 50.00, 'pending', '2024-01-22'),
('Diana', 800.00, 'shipped', '2024-02-01'),
('Charlie', 1200.00, 'pending', '2024-02-05'),
('Eve', 30.00, 'cancelled', '2024-02-10');
```

## Tasks

### 1. Categorize orders by value

Use CASE to label orders: < 100 as 'Small', 100-1000 as 'Medium', > 1000 as 'Large'.

### 2. Status display using CASE

Show each order with a human-readable status: 'shipped' → 'Delivered', 'pending' → 'Processing', 'cancelled' → 'Cancelled'.

### 3. CASE in ORDER BY

Show orders sorted by status priority: cancelled first, then pending, then shipped.

### 4. Customer tier using CASE

Show each customer and their tier based on total spend (use a subquery to total per customer first).

### 5. Conditional aggregation with CASE

Pivot the statuses: show month, shipped_count, pending_count, cancelled_count.

### 6. NULLIF practice

Given a column that stores 'N/A' for missing values, use NULLIF to convert it to proper NULL.

```sql
CREATE TABLE reviews (product VARCHAR(100), rating VARCHAR(10));
INSERT INTO reviews VALUES ('Laptop', '5'), ('Mouse', 'N/A'), ('Keyboard', '4');
```

Convert 'N/A' ratings to NULL, then show the average rating (cast to numeric).

## Expected Output for Task 1

```
 customer | total  |  size
----------+--------+--------
 Alice    | 1500.00| Large
 Bob      |  250.00| Medium
 Charlie  |   75.00| Small
 Alice    | 3000.00| Large
 Bob      |   50.00| Small
 Diana    |  800.00| Medium
 Charlie  | 1200.00| Large
 Eve      |   30.00| Small
```

## Hints

- `CASE WHEN total < 100 THEN 'Small' WHEN total <= 1000 THEN 'Medium' ELSE 'Large' END`
- For task 4, use a subquery to aggregate first
- For task 5, use `COUNT(*) FILTER (WHERE status = 'shipped')` or `COUNT(CASE WHEN status = 'shipped' THEN 1 END)`
