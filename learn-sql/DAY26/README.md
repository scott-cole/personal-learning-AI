# DAY26 — Normalization: Organising a Kitchen

## Anecdote: Normalization is Organising a Kitchen

Imagine your kitchen has one massive drawer. Everything goes in there: knives, forks, spoons, spatulas, measuring cups, spice jars, and an old takeout menu from 2019. When you need a teaspoon, you dig through the chaos. You find three teaspoons eventually, plus a whisk you forgot you owned, plus a dried-up marker.

Now organise: separate drawer for cutlery, cabinet for spices, hook for measuring cups. You still have one of each item, but now each thing is in its proper place. You never have duplicates (you don't need six teaspoons if you only use two).

That's **normalization**: organising data to reduce redundancy and improve integrity. You decompose large, messy tables into smaller, well-structured ones connected by foreign keys.

## First Normal Form (1NF)

Three rules:
1. Each column contains atomic (indivisible) values
2. Each column contains values of the same type
3. Each row is unique (has a primary key)

**Violation (bad):**
```
 id | customer  | products
----+-----------+-------------------------
 1  | Alice     | Laptop, Mouse, Keyboard
 2  | Bob       | Monitor, Desk
```

**Fixed (1NF):**
```
 id | customer  | product
----+-----------+----------
 1  | Alice     | Laptop
 2  | Alice     | Mouse
 3  | Alice     | Keyboard
 4  | Bob       | Monitor
 5  | Bob       | Desk
```

## Second Normal Form (2NF)

Must be in 1NF, AND every non-key column must depend on the **entire** primary key (relevant for composite keys).

**Violation (bad):**
```sql
CREATE TABLE orders (
    order_id INTEGER,
    product_id INTEGER,
    customer_name VARCHAR(100),  -- Depends only on order_id, not product_id
    product_name VARCHAR(200),   -- Depends only on product_id, not order_id
    quantity INTEGER,
    PRIMARY KEY (order_id, product_id)
);
```

**Fixed (2NF):** Split into `orders`, `order_items`, and `products` tables.

## Third Normal Form (3NF)

Must be in 2NF, AND no transitive dependencies (non-key column depends on another non-key column).

**Violation (bad):**
```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INTEGER,
    customer_phone VARCHAR(20),  -- Depends on customer_id, not order
    total NUMERIC(10,2)
);
```

**Fixed (3NF):** Move `customer_phone` to the `customers` table.

## Denormalization (When You Break the Rules)

Sometimes you intentionally denormalize for performance:
- A dashboard table that pre-joins data to avoid 5 JOINs every query
- A `total_spent` column on `customers` that's updated periodically
- Summary tables that are refreshed nightly

Normalization is the default. Denormalize only when you have a measured performance problem.

## Normal Forms Summary

| Form | Rule | Typical Violation |
|------|------|------------------|
| 1NF | Atomic columns, unique rows | Comma-separated list in a column |
| 2NF | Full PK dependency | Column depends on part of composite key |
| 3NF | No transitive dependencies | Column depends on another non-key column |
| BCNF | Every determinant is a candidate key | Overlapping composite keys |
| 4NF | No multi-valued dependencies | Independent 1:N relationships in same table |

## Practical Process

```sql
-- Start with one big table (0NF):
CREATE TABLE sales_denormalized (
    customer VARCHAR(100),
    customer_email VARCHAR(200),
    product VARCHAR(200),
    product_category VARCHAR(100),
    price NUMERIC(10,2),
    quantity INTEGER,
    sale_date DATE
);

-- After normalization (3NF):
-- customers(id, name, email)
-- products(id, name, category, price)
-- sales(id, customer_id, product_id, quantity, sale_date)
```

## Summary

| Form | Core Idea |
|------|-----------|
| 1NF | No repeating groups within a column |
| 2NF | No partial dependencies on a composite key |
| 3NF | No transitive dependencies |
| Denormalization | Intentional redundancy for performance |
