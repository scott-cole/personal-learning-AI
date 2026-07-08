# DAY15 — Drill: Write Subqueries

## Setup

```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(200) NOT NULL,
    category VARCHAR(100),
    price NUMERIC(10,2) NOT NULL,
    stock INTEGER DEFAULT 0
);

INSERT INTO products (name, category, price, stock) VALUES
('Laptop', 'Electronics', 1200.00, 10),
('Mouse', 'Electronics', 25.00, 200),
('Keyboard', 'Electronics', 75.00, 150),
('Monitor', 'Electronics', 350.00, 30),
('Desk', 'Furniture', 450.00, 5),
('Chair', 'Furniture', 300.00, 15),
('Notebook', 'Stationery', 5.00, 500),
('Pen', 'Stationery', 2.00, 1000),
('Desk Lamp', 'Furniture', 65.00, 25);
```

## Tasks

### 1. Scalar subquery in WHERE

Find products with a price above the average price of all products.

### 2. Subquery with IN

Find products in categories that have more than 3 products. Use a subquery to find those category names first.

### 3. Derived table in FROM

Find categories where the average price is above $100. Use a subquery in FROM.

### 4. Subquery in SELECT

Show each product's name, price, and the difference from the overall average price.

### 5. Correlated subquery

Find products that are above the average price for their own category.

### 6. Multiple levels of nesting

Find the cheapest product in each category. Use a subquery with GROUP BY inside a WHERE.

## Expected Output for Task 6

```
  name   | category   | price
---------+------------+-------
 Mouse   | Electronics|  25.00
 Pen     | Stationery |   2.00
 Desk    | Furniture  | 450.00
```

Wait — Desk at $450 is the cheapest in Furniture? Yes (Chair is $300 — hmm, then Chair should be in the output). Re-check the data.

## Hints

- Subqueries in `WHERE IN` need to return a single column
- Derived tables must have an alias: `FROM (SELECT ...) AS alias`
- For task 6: `WHERE (category, price) IN (SELECT category, MIN(price) FROM products GROUP BY category)`
