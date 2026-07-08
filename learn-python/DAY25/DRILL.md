# DAY25 Drill — The Filing Cabinet

## Task

Write a Python script (`drill.py`) that builds a simple inventory database with SQLite.

### Requirements

1. **Create the database** — `inventory.db` with a table `products`
2. **Table schema**:
   ```sql
   CREATE TABLE IF NOT EXISTS products (
       id INTEGER PRIMARY KEY AUTOINCREMENT,
       name TEXT NOT NULL,
       category TEXT NOT NULL,
       price REAL NOT NULL,
       quantity INTEGER NOT NULL DEFAULT 0
   )
   ```
3. **Insert sample data** — Add at least 6 products (electronics, office supplies, etc.)
4. **Query functions**:
   - `get_all_products()` — returns all products as list of dicts
   - `get_by_category(category)` — returns products matching category
   - `get_low_stock(threshold)` — returns products with quantity < threshold
   - `total_value()` — returns the total value of all stock (price × quantity)
5. **Update** — Increase the quantity of a product by a given amount
6. **Delete** — Remove a product by ID
7. **Demonstrate** all functions in `main()`

### Starting data

```python
sample_products = [
    ("Laptop", "Electronics", 1200.00, 10),
    ("Mouse", "Electronics", 25.00, 50),
    ("Desk Chair", "Furniture", 350.00, 5),
    ("Notebook", "Office Supplies", 3.50, 200),
    ("Monitor", "Electronics", 350.00, 8),
    ("Desk Lamp", "Furniture", 45.00, 15),
    ("Stapler", "Office Supplies", 12.00, 30),
]
```

### Expected output

```
=== INVENTORY DATABASE ===
All products:
1. Laptop (Electronics) — $1200.00 (qty: 10)
2. Mouse (Electronics) — $25.00 (qty: 50)
...
=== BY CATEGORY: Electronics ===
Laptop — $1200.00 (qty: 10)
Mouse — $25.00 (qty: 50)
Monitor — $350.00 (qty: 8)
=== LOW STOCK (qty < 10) ===
Laptop — qty: 10 — OK
Desk Chair — qty: 5 — LOW STOCK!
Monitor — qty: 8 — LOW STOCK!
=== TOTAL VALUE ===
Total inventory value: $XXXX.XX
=== AFTER UPDATE ===
Increased Laptop qty by 5: now 15
=== AFTER DELETE ===
Deleted Stapler (ID 7)
Updated products: 6 remaining
```

### Hints

- `conn.row_factory = sqlite3.Row` then `dict(row)` to get dicts
- `cursor.lastrowid` for the ID of the last inserted row
- `sum(p["price"] * p["quantity"] for p in products)` for total value

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] Connect to SQLite and create table
- [ ] Insert sample data
- [ ] Implement `get_all_products()`
- [ ] Implement `get_by_category()`
- [ ] Implement `get_low_stock()`
- [ ] Implement `total_value()`
- [ ] Implement update and delete
- [ ] Demonstrate everything in `main()`
