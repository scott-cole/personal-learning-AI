# DAY21 — Checkpoint

## Questions

**1. How would you calculate the percentage contribution of each product category to total revenue?**

<details>
<summary>Answer</summary>
```sql
SELECT category, SUM(quantity * price) AS revenue,
    ROUND(SUM(quantity * price) / (SELECT SUM(quantity * price) FROM sales JOIN products ON ...) * 100, 1) AS pct
FROM ...
GROUP BY category;
```
</details>

**2. What does `DISTINCT ON (month)` do when combined with `ORDER BY month, revenue DESC`?**

<details>
<summary>Answer</summary>
It keeps only the first row for each unique month — the one with the highest revenue (since we ordered by revenue DESC).
</details>

**3. How do you calculate the number of days between a customer's signup and their first purchase?**

<details>
<summary>Answer</summary>
```sql
SELECT c.name, MIN(s.sale_date) - c.signup_date AS days_to_first_purchase
FROM customers c JOIN sales s ON c.id = s.customer_id
GROUP BY c.id, c.name, c.signup_date;
```
</details>

**4. Write a query to find customers who have NOT made a purchase in the last 30 days (from a reference date).**

<details>
<summary>Answer</summary>
```sql
SELECT * FROM customers c
WHERE NOT EXISTS (
    SELECT 1 FROM sales s
    WHERE s.customer_id = c.id
    AND s.sale_date > '2024-03-01' - INTERVAL '30 days'
);
```
</details>

**5. What is a rolling average and how would you implement it in PostgreSQL?**

<details>
<summary>Answer</summary>
A rolling average computes the average over a moving window of rows. Window functions: `AVG(amount) OVER (ORDER BY date ROWS BETWEEN 7 PRECEDING AND CURRENT ROW)`.
</details>
