# DAY14 — Project: E-Commerce Database

## Week 2 Project

You've completed two weeks of SQL. You now know SELECT, WHERE, ORDER BY, CRUD, data types, NULLs, primary/foreign keys, INNER/LEFT/RIGHT/FULL JOINs, CROSS JOINs, self-joins, aggregation, and GROUP BY/HAVING.

Now you'll build a complete e-commerce database from scratch.

## Scenario

A small online store needs a database to track:
- **Customers** — who buys from the store
- **Products** — what's for sale
- **Orders** — who bought what and when
- **Order Items** — the individual line items within an order

## Schema Design

### customers

| Column | Type | Constraints |
|--------|------|-------------|
| id | SERIAL | PK |
| name | VARCHAR(100) | NOT NULL |
| email | VARCHAR(200) | UNIQUE, NOT NULL |
| joined_at | DATE | DEFAULT CURRENT_DATE |

### products

| Column | Type | Constraints |
|--------|------|-------------|
| id | SERIAL | PK |
| name | VARCHAR(200) | NOT NULL |
| price | NUMERIC(10,2) | NOT NULL, CHECK > 0 |
| category | VARCHAR(100) | |
| stock | INTEGER | DEFAULT 0 |

### orders

| Column | Type | Constraints |
|--------|------|-------------|
| id | SERIAL | PK |
| customer_id | INTEGER | FK → customers(id) |
| order_date | TIMESTAMP | DEFAULT NOW() |
| status | VARCHAR(20) | DEFAULT 'pending' |

### order_items

| Column | Type | Constraints |
|--------|------|-------------|
| id | SERIAL | PK |
| order_id | INTEGER | FK → orders(id) ON DELETE CASCADE |
| product_id | INTEGER | FK → products(id) |
| quantity | INTEGER | NOT NULL, CHECK > 0 |
| unit_price | NUMERIC(10,2) | NOT NULL |

## Sample Data

Insert:
- 4 customers
- 6 products (mix of categories)
- 5 orders spread across customers
- 8-10 order items linking orders to products

## Queries to Write

1. List all orders with customer names and total amounts
2. Show each product and how many times it has been ordered
3. Find the top 3 most-ordered products
4. List customers who have spent more than $500 total
5. Show all orders that include a 'Laptop' along with the customer name
6. Find products that have never been ordered (using LEFT JOIN)
7. Calculate the average order value (total revenue / number of orders)
8. Monthly revenue report (group by month)
9. Show each customer's most recent order date and total spent
10. Challenge: Find customers who ordered a product in the same category more than once

## Deliverable

Write everything into `ecommerce_project.sql`. Make it runnable end-to-end with `\i ecommerce_project.sql`.

## Hints

- Use `COALESCE` for products with zero orders
- `DATE_TRUNC('month', order_date)` for monthly grouping
- JOIN order_items → orders → customers for customer + total queries
- LEFT JOIN from products to order_items for un-ordered products
