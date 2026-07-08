# DAY25 — Drill: Create and Use Views

## Setup

Use the e-commerce database from DAY14 or create:

```sql
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(200) UNIQUE NOT NULL
);

CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(200) NOT NULL,
    price NUMERIC(10,2) NOT NULL,
    category VARCHAR(100)
);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INTEGER REFERENCES customers(id),
    order_date TIMESTAMP DEFAULT NOW()
);

CREATE TABLE order_items (
    id SERIAL PRIMARY KEY,
    order_id INTEGER REFERENCES orders(id) ON DELETE CASCADE,
    product_id INTEGER REFERENCES products(id),
    quantity INTEGER NOT NULL,
    unit_price NUMERIC(10,2) NOT NULL
);

-- Insert sample data (at least 3 customers, 5 products, 5 orders, 10 items)
```

## Tasks

### 1. Create a view: `customer_orders`

Show customer name, order id, order date, and total amount per order.

```sql
CREATE VIEW customer_orders AS
SELECT ... ;
```

### 2. Query the view

```sql
SELECT * FROM customer_orders WHERE total > 100;
```

### 3. Create a view: `product_sales`

Show product name, total quantity sold, total revenue.

### 4. Create a materialized view: `daily_sales_summary`

Summarize sales by day with total revenue and order count.

### 5. Test updatable views

Create a simple view on customers and insert through it.

### 6. WITH CHECK OPTION

```sql
CREATE VIEW premium_products AS
SELECT * FROM products WHERE price >= 100
WITH CHECK OPTION;

-- Test: insert a product with price 50 through this view
-- What happens?
```

## Hints

- `CREATE VIEW name AS query` — that's it!
- Materialized views need `REFRESH MATERIALIZED VIEW name` to update
- `WITH CHECK OPTION` prevents inserts/updates that would make the row invisible in the view
- `DROP VIEW IF EXISTS name` to clean up before recreating
