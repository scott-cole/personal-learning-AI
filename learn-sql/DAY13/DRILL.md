# DAY13 — Drill: Group and Filter

## Setup

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer VARCHAR(100) NOT NULL,
    product VARCHAR(100) NOT NULL,
    price NUMERIC(10,2) NOT NULL,
    quantity INTEGER NOT NULL,
    order_date DATE DEFAULT CURRENT_DATE,
    region VARCHAR(50)
);

INSERT INTO orders (customer, product, price, quantity, order_date, region) VALUES
('Alice', 'Laptop', 1200.00, 1, '2024-01-15', 'North'),
('Bob', 'Mouse', 25.00, 3, '2024-01-15', 'South'),
('Alice', 'Monitor', 350.00, 2, '2024-01-16', 'North'),
('Charlie', 'Desk', 450.00, 1, '2024-01-16', 'East'),
('Bob', 'Chair', 300.00, 1, '2024-01-17', 'South'),
('Alice', 'Mouse', 25.00, 2, '2024-01-18', 'North'),
('Diana', 'Laptop', 1200.00, 1, '2024-01-19', 'West'),
('Charlie', 'Monitor', 350.00, 1, '2024-01-20', 'East');
```

## Tasks

### 1. Total spent per customer

Show each customer and total amount spent (price × quantity).

### 2. Number of orders per customer

Show customer name and order count, sorted by count descending.

### 3. Average order value per region

Show region and average order value.

### 4. Customers who spent more than $500

Use HAVING to filter groups after aggregation.

### 5. Products with total quantity sold > 2

Group by product, sum quantity, filter with HAVING.

### 6. Combine WHERE and HAVING

Show regions where total revenue exceeds $500, but only include orders from January 2024. (WHERE for date filter, GROUP BY region, HAVING for revenue.)

### 7. Multiple GROUP BY columns

Show total revenue by region AND customer combination.

## Expected Output for Task 1

```
 customer | total_spent
----------+-------------
 Alice    |     1975.00
 Bob      |      375.00
 Charlie  |      800.00
 Diana    |     1200.00
```

## Hints

- `SUM(price * quantity)` for total spent
- `GROUP BY customer` for task 1, `GROUP BY region` for task 3
- `HAVING SUM(price * quantity) > 500` for task 4
- `WHERE order_date >= '2024-01-01' AND order_date < '2024-02-01'` for task 6
