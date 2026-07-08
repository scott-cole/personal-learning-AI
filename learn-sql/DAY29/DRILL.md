# DAY29 — Drill: Performance Analysis

## Setup

```sql
CREATE TABLE large_table (
    id SERIAL PRIMARY KEY,
    category VARCHAR(50),
    value INTEGER,
    description TEXT,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Insert 10000 rows
INSERT INTO large_table (category, value, description)
SELECT
    CASE WHEN g % 4 = 0 THEN 'A'
         WHEN g % 4 = 1 THEN 'B'
         WHEN g % 4 = 2 THEN 'C'
         ELSE 'D' END,
    (random() * 10000)::INTEGER,
    'Description for row ' || g
FROM generate_series(1, 10000) g;
```

## Tasks

### 1. Seq Scan observation

Run and observe the plan:

```sql
EXPLAIN ANALYZE SELECT * FROM large_table WHERE value = 5000;
```

Note the cost and execution time.

### 2. Create index and compare

```sql
CREATE INDEX idx_large_value ON large_table (value);

EXPLAIN ANALYZE SELECT * FROM large_table WHERE value = 5000;
```

How much faster?

### 3. Composite index test

```sql
EXPLAIN ANALYZE SELECT * FROM large_table WHERE category = 'A' ORDER BY value;
```

Then add:

```sql
CREATE INDEX idx_large_cat_val ON large_table (category, value);
```

Re-run and compare.

### 4. Partial index test

```sql
CREATE INDEX idx_large_high ON large_table (value) WHERE value > 9000;

EXPLAIN ANALYZE SELECT * FROM large_table WHERE value > 9500;
EXPLAIN ANALYZE SELECT * FROM large_table WHERE value < 100;
```

Does each query use the partial index?

### 5. Seq Scan vs Index Scan threshold

Find the approximate threshold where PostgreSQL switches from Index Scan to Seq Scan. (Hint: it depends on what percentage of rows the query returns.)

### 6. ANALYZE command

Check what happens when you run:

```sql
ANALYZE large_table;
EXPLAIN ANALYZE SELECT * FROM large_table WHERE value = 5000;
```

## Hints

- Execution time is in milliseconds
- `Seq Scan` on 10k rows is usually fast — try larger data for more dramatic results
- The planner switches from Index Scan to Seq Scan when it estimates the query will return > ~10-20% of rows
- `ANALYZE` (without EXPLAIN) updates statistics; run it after large inserts
- Index creation can take time on large tables
