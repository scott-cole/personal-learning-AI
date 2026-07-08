# DAY06 — Drill: Handle the Nulls

## Setup

```sql
CREATE TABLE shipments (
    id SERIAL PRIMARY KEY,
    item VARCHAR(100) NOT NULL,
    quantity INTEGER,
    delivered_at TIMESTAMP,
    destination VARCHAR(100)
);

INSERT INTO shipments (item, quantity, delivered_at, destination) VALUES
('Widget', 10, '2024-01-15 10:00:00', 'Warehouse A'),
('Gadget', NULL, '2024-01-16 10:00:00', 'Warehouse B'),
('Doohickey', 5, NULL, NULL),
('Thingamajig', 0, '2024-01-17 10:00:00', 'Warehouse A'),
(NULL, 3, '2024-01-18 10:00:00', 'Warehouse C');
```

## Tasks

### 1. Find all shipments where `quantity` is NULL

### 2. Find all shipments where `destination` is NOT NULL

### 3. Use COALESCE to show 'Unknown destination' for NULL destinations

```sql
SELECT item, COALESCE(destination, 'Unknown destination') AS dest FROM shipments;
```

### 4. Use NULLIF to treat quantity 0 as NULL, then divide 100 by it safely

```sql
SELECT 100 / NULLIF(quantity, 0) AS ratio FROM shipments;
```

### 5. Find shipments where `delivered_at` is NULL OR item is NULL

### 6. Show all shipments sorted by quantity with NULLs first

## Expected Output for Task 3

```
    item     |      dest
-------------+---------------------
 Widget      | Warehouse A
 Gadget      | Warehouse B
 Doohickey   | Unknown destination
 Thingamajig | Warehouse A
 NULL        | Warehouse C
```

## Hints

- `IS NULL`, not `= NULL`
- `COALESCE` takes multiple arguments
- `NULLIF(x, 0)` is handy for division
