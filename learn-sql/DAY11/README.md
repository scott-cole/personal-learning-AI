# DAY11 — CROSS JOIN and Self-Joins

## Anecdote: Self-Join is a Mirror

Stand in front of a mirror. You see yourself — same person, same features, but reversed. A **self-join** is like that mirror: you join a table to itself, treating it as two separate copies so you can compare rows within the same table.

A **CROSS JOIN** is a whole different beast. Imagine you own 5 shirts and 3 pairs of pants. A CROSS JOIN answers the question "what are all possible outfits?" — every shirt paired with every pair of pants. 5 × 3 = 15 outfits. It's a Cartesian product.

## CROSS JOIN

Every row from table A paired with every row from table B:

```sql
SELECT *
FROM colors
CROSS JOIN sizes;

-- Same thing:
SELECT * FROM colors, sizes;
```

Use case: generating all combinations (inventory SKUs, calendar dates × time slots, etc.).

```sql
-- Generate all possible shirt configurations
CREATE TABLE colors (name VARCHAR(20));
CREATE TABLE sizes (name VARCHAR(20));

INSERT INTO colors VALUES ('Red'), ('Blue'), ('Green');
INSERT INTO sizes VALUES ('S'), ('M'), ('L'), ('XL');

SELECT c.name || ' ' || s.name AS variant
FROM colors c
CROSS JOIN sizes s;

-- Returns 12 rows: Red S, Red M, ..., Green XL
```

## Self-Join

Join a table to itself using table aliases:

```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    manager_id INTEGER REFERENCES employees(id)
);

INSERT INTO employees (name, manager_id) VALUES
('Alice', NULL),       -- CEO, no manager
('Bob', 1),            -- Reports to Alice
('Charlie', 1),        -- Reports to Alice
('Diana', 2);          -- Reports to Bob
```

Find each employee and their manager's name:

```sql
SELECT
    e.name AS employee,
    m.name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id;
```

You use `LEFT JOIN` (not INNER) because Alice has no manager.

## Self-Join for Relational Pairs

```sql
CREATE TABLE friendships (
    user_id INTEGER REFERENCES users(id),
    friend_id INTEGER REFERENCES users(id),
    PRIMARY KEY (user_id, friend_id)
);

-- Find each user's friends:
SELECT u.name AS user, f.name AS friend
FROM friendships fs
INNER JOIN users u ON fs.user_id = u.id
INNER JOIN users f ON fs.friend_id = f.id;
```

## Self-Join for Sequential Relationships

```sql
CREATE TABLE events (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    previous_event_id INTEGER REFERENCES events(id)
);
```

## CROSS JOIN vs. INNER JOIN vs. SELF-JOIN

| Join Type | Tables | What It Returns |
|-----------|--------|----------------|
| CROSS JOIN | Two different | Every combination (Cartesian product) |
| Self-Join | One table twice | Compare rows within same table |
| INNER JOIN | Two different | Only matching combinations |

## Summary

| Concept | Syntax | Example Use |
|---------|--------|-------------|
| CROSS JOIN | `FROM a CROSS JOIN b` | Generate all combinations |
| Self-Join | `FROM t a JOIN t b ON ...` | Hierarchies (employee/manager) |
| Self-Join + LEFT | `FROM t a LEFT JOIN t b ON ...` | Include roots (CEO with no manager) |
