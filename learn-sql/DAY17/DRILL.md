# DAY17 — Drill: Set Operations

## Setup

```sql
CREATE TABLE fruit_basket_a (fruit VARCHAR(50));
CREATE TABLE fruit_basket_b (fruit VARCHAR(50));

INSERT INTO fruit_basket_a VALUES
('Apple'), ('Banana'), ('Cherry'), ('Banana');  -- Note duplicate!

INSERT INTO fruit_basket_b VALUES
('Banana'), ('Cherry'), ('Date'), ('Elderberry');
```

## Tasks

### 1. UNION — unique fruits across both baskets

### 2. UNION ALL — all fruits including duplicates

### 3. INTERSECT — fruits in both baskets

### 4. EXCEPT — fruits in A but not in B

### 5. EXCEPT (reversed) — fruits in B but not in A

### 6. Multi-set with labels

```sql
SELECT fruit, 'Basket A' AS source FROM fruit_basket_a
UNION ALL
SELECT fruit, 'Basket B' AS source FROM fruit_basket_b
ORDER BY fruit;
```

### 7. Real-world scenario

You have two tables:

```sql
CREATE TABLE active_users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(200) NOT NULL
);

CREATE TABLE banned_users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(200) NOT NULL
);

INSERT INTO active_users (email) VALUES
('alice@email.com'), ('bob@email.com'), ('charlie@email.com');

INSERT INTO banned_users (email) VALUES
('bob@email.com'), ('mallory@email.com');
```

Find:
- All unique emails across both tables (UNION)
- Emails that are active but not banned (EXCEPT)
- Emails that are both active and banned (INTERSECT)

## Expected Output for Task 4 (EXCEPT A - B)

```
 fruit
--------
 Apple
```

## Hints

- All set operations require same column count and compatible types
- `ORDER BY` goes at the very end
- `UNION` removes duplicates; `UNION ALL` keeps them
