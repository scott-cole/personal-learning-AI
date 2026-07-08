# DAY14 — Checkpoint

## Questions

**1. In an e-commerce database, why is `order_items` a separate table rather than a column in `orders`?**

<details>
<summary>Answer</summary>
An order can contain multiple products. Storing them in a separate `order_items` table with a foreign key to `orders` keeps the schema normalized and avoids repeating order-level data.
</details>

**2. Why does `order_items` have its own `unit_price` column instead of relying on `products.price`?**

<details>
<summary>Answer</summary>
Product prices can change over time. The `order_items.unit_price` captures the price at the time of purchase, preserving historical accuracy.
</details>

**3. What does `ON DELETE CASCADE` on the `order_id` FK in `order_items` do?**

<details>
<summary>Answer</summary>
If an order is deleted, all its order_items are automatically deleted too.
</details>

**4. Write a query to calculate the total amount of a specific order (id = 1).**

<details>
<summary>Answer</summary>
```sql
SELECT SUM(quantity * unit_price) AS order_total
FROM order_items
WHERE order_id = 1;
```
</details>

**5. How would you find all orders placed by a customer named 'Alice'?**

<details>
<summary>Answer</summary>
```sql
SELECT o.*
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id
WHERE c.name = 'Alice';
```
</details>
