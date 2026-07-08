# DAY21 — Project: Analytics Queries on a Sales Database

## Week 3 Project

Week 3 covered subqueries, EXISTS/ANY/ALL, set operations, string functions, date/time functions, and CASE expressions. Now you'll combine everything into a serious analytics project.

## Scenario

You're a data analyst at "Megastore." The CEO wants a monthly performance dashboard. Your job: write SQL queries that produce the exact answers.

## Schema

```sql
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(200) UNIQUE NOT NULL,
    signup_date DATE DEFAULT CURRENT_DATE,
    region VARCHAR(50)
);

CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(200) NOT NULL,
    category VARCHAR(100),
    price NUMERIC(10,2) NOT NULL,
    cost NUMERIC(10,2) NOT NULL
);

CREATE TABLE sales (
    id SERIAL PRIMARY KEY,
    product_id INTEGER REFERENCES products(id),
    customer_id INTEGER REFERENCES customers(id),
    sale_date TIMESTAMP DEFAULT NOW(),
    quantity INTEGER NOT NULL,
    discount NUMERIC(5,2) DEFAULT 0.00
);
```

Insert at least:
- 6 customers across 3 regions
- 8 products across 3 categories
- 25+ sales spanning 3 months

## Analytics Queries

### 1. Monthly revenue trend

Show year-month, total revenue (quantity × price), and number of sales. Order chronologically.

### 2. Top 5 products by revenue

Product name, total revenue, total quantity sold.

### 3. Customer lifetime value

Customer name, total spent, number of orders, average order value. Sort by total spent descending.

### 4. Category performance

Category, total revenue, % of overall revenue (use a subquery).

### 5. Monthly bestseller

For each month, which product had the highest revenue? (Use subquery with ROW_NUMBER or DISTINCT ON.)

### 6. Region breakdown

Region, total revenue, number of customers who bought something, average customer spend.

### 7. Discount analysis

Show how much revenue was lost to discounts (SUM of price × quantity × discount/100). Also show the average discount per sale.

### 8. Product affinity (cross-selling)

Find pairs of products that are frequently bought together by the same customer in the same month. (Self-join on sales.)

### 9. Customer segmentation with CASE

Categorize customers by total spend: < 500 = 'Bronze', 500-1500 = 'Silver', > 1500 = 'Gold'. Show count per tier.

### 10. Rolling 7-day average (advanced)

Use window functions (or a self-join) to compute a rolling 7-day average of daily revenue.

## Deliverable

Write all SQL into `analytics_project.sql`. Each query should be clearly labelled with a comment.

## Hints

- `TO_CHAR(sale_date, 'YYYY-MM')` for monthly grouping
- `s.quantity * p.price` for revenue
- `(s.quantity * p.price * s.discount / 100)` for discount amount
- Use `ROUND(..., 2)` for clean numbers
- For query 5, `DISTINCT ON (month) ... ORDER BY month, revenue DESC`
- For query 10, use `AVG(revenue) OVER (ORDER BY day ROWS BETWEEN 7 PRECEDING AND CURRENT ROW)`
