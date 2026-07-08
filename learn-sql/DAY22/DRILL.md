# DAY22 — Drill: Index Performance

## Setup

```sql
CREATE TABLE large_products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(200) NOT NULL,
    category VARCHAR(100),
    price NUMERIC(10,2),
    created_at TIMESTAMP DEFAULT NOW()
);

-- Insert 1000 rows (adjust if needed)
INSERT INTO large_products (name, category, price)
SELECT
    'Product ' || g,
    CASE WHEN g % 3 = 0 THEN 'Electronics'
         WHEN g % 3 = 1 THEN 'Furniture'
         ELSE 'Stationery' END,
    ROUND((random() * 1000 + 10)::NUMERIC, 2)
FROM generate_series(1, 1000) g;
```

## Tasks

### 1. Query without index

Run this and note the cost:

```sql
EXPLAIN ANALYZE SELECT * FROM large_products WHERE price = 500.00;
```

### 2. Create an index

```sql
CREATE INDEX idx_large_products_price ON large_products (price);
```

### 3. Query with index

Re-run the same query with EXPLAIN ANALYZE and compare the cost.

### 4. Composite index test

```sql
CREATE INDEX idx_large_products_cat_price ON large_products (category, price);
```

Then test:

```sql
EXPLAIN ANALYZE SELECT * FROM large_products WHERE category = 'Electronics' AND price > 100;
```

Does it use the index?

### 5. Partial index

Create an index only for expensive products (price > 500) and test queries.

### 6. Index on expression

```sql
CREATE INDEX idx_lower_name ON large_products (LOWER(name));

EXPLAIN ANALYZE SELECT * FROM large_products WHERE LOWER(name) = 'product 42';
```

## Hints

- Use `EXPLAIN ANALYZE` (not just `EXPLAIN`) to see actual execution time
- `random()` in the insert creates different data each time — your costs will vary
- Compare "Seq Scan" vs "Index Scan" in the output
- A partial index is only used when the WHERE matches the index condition
