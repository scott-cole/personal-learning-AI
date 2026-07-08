# DAY26 — Drill: Normalize a Denormalized Schema

## Setup

```sql
CREATE TABLE sales_denormalized (
    id SERIAL PRIMARY KEY,
    customer_name VARCHAR(100),
    customer_email VARCHAR(200),
    customer_city VARCHAR(100),
    product_name VARCHAR(200),
    product_category VARCHAR(100),
    product_price NUMERIC(10,2),
    quantity INTEGER,
    sale_date DATE,
    employee_name VARCHAR(100),
    employee_role VARCHAR(50)
);

INSERT INTO sales_denormalized VALUES
(1, 'Alice', 'alice@email.com', 'New York', 'Laptop', 'Electronics', 1200.00, 1, '2024-01-15', 'Bob', 'Sales Rep'),
(2, 'Alice', 'alice@email.com', 'New York', 'Mouse', 'Electronics', 25.00, 2, '2024-01-15', 'Bob', 'Sales Rep'),
(3, 'Bob', 'bob@email.com', 'Los Angeles', 'Monitor', 'Electronics', 350.00, 1, '2024-01-16', 'Carol', 'Manager'),
(4, 'Charlie', 'charlie@email.com', 'Chicago', 'Desk', 'Furniture', 450.00, 1, '2024-01-17', 'Bob', 'Sales Rep'),
(5, 'Alice', 'alice@email.com', 'New York', 'Chair', 'Furniture', 300.00, 2, '2024-01-18', 'Carol', 'Manager');
```

## Tasks

### 1. Identify redundancies

List all columns that are repeated across rows (candidate for normalization).

### 2. Design 3NF schema

Design tables that would bring this to 3NF. Write the CREATE TABLE statements.

### 3. Insert normalized data

Write INSERT statements to migrate the denormalized data into your normalized tables.

### 4. Query the normalized schema

Write a query that reproduces the original denormalized view (all columns) using JOINs.

### 5. Verify

Compare `SELECT * FROM sales_denormalized` with your reconstructed view. They should match.

## Expected Normalized Tables

You should have at least:
- **customers** — id, name, email, city
- **products** — id, name, category, price
- **employees** — id, name, role
- **sales** — id, customer_id, product_id, employee_id, quantity, sale_date

## Hints

- Each unique customer in the denormalized table should become one row in `customers`
- Each unique product should become one row in `products`
- Each unique employee should become one row in `employees`
- The `sales` table references all three via foreign keys
- Use `DISTINCT` to extract unique values for parent tables
