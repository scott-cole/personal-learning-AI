# DAY24 — Constraints: Guardrails on a Bridge

## Anecdote: Constraints are Guardrails on a Bridge

You're driving across a bridge. The guardrails prevent you from:
- Swerving off the edge (NOT NULL — prevents missing data)
- Crashing into oncoming traffic (UNIQUE — prevents duplicates)
- Driving a semi-truck into a tunnel with a 7-foot clearance (CHECK — validates data)
- Going the wrong direction on a one-way street (FOREIGN KEY — ensures valid references)

**Constraints** are the database's guardrails. They enforce rules at the database level so bad data never gets in — no matter what application or person is inserting it.

Constraints are better than application-level validation because they're always enforced, even if someone connects via a different tool or bypasses your app.

## NOT NULL

The most basic constraint: this column must always have a value:

```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(200) NOT NULL,    -- Must have a name
    price NUMERIC(10,2) NOT NULL   -- Must have a price
);

-- Adding to an existing table:
ALTER TABLE products ALTER COLUMN name SET NOT NULL;
```

## UNIQUE

No two rows can have the same value:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(200) UNIQUE,     -- One account per email
    username VARCHAR(50) UNIQUE NOT NULL
);

-- Multi-column unique:
CREATE TABLE follows (
    follower_id INTEGER,
    followee_id INTEGER,
    UNIQUE (follower_id, followee_id)  -- Can't follow same person twice
);
```

## CHECK

Arbitrary boolean condition:

```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    price NUMERIC(10,2) CHECK (price > 0),  -- No free or negative items
    stock INTEGER CHECK (stock >= 0),       -- No negative stock
    rating NUMERIC(2,1) CHECK (rating >= 0 AND rating <= 10),
    status VARCHAR(20) CHECK (status IN ('active', 'inactive', 'discontinued'))
);
```

```sql
-- Add CHECK to existing table:
ALTER TABLE orders ADD CONSTRAINT positive_total CHECK (total >= 0);
```

## DEFAULT

Not strictly a constraint, but related: provides a fallback value:

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    status VARCHAR(20) DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT NOW(),
    total NUMERIC(10,2) DEFAULT 0.00
);
```

## Exclusion Constraints (PostgreSQL-Specific)

Ensures no two rows conflict on a range or comparison:

```sql
CREATE TABLE meeting_rooms (
    room_id INTEGER,
    reserved_at TIMESTAMPTZ,
    duration INTERVAL,
    EXCLUDE USING gist (room_id WITH =, reserved_at WITH &&)
);
```

This prevents overlapping reservations for the same room.

## Naming Constraints

Always name your constraints for better error messages:

```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    email VARCHAR(200),
    CONSTRAINT employees_email_unique UNIQUE (email),
    CONSTRAINT employees_email_not_empty CHECK (email <> '')
);
```

When a constraint is violated, the name appears in the error:

```
ERROR: duplicate key value violates unique constraint "employees_email_unique"
```

## Dropping Constraints

```sql
ALTER TABLE products DROP CONSTRAINT products_price_check;
ALTER TABLE users ALTER COLUMN email DROP NOT NULL;
```

## Summary

| Constraint | Purpose | Example |
|-----------|---------|---------|
| `NOT NULL` | Column must have a value | `name VARCHAR NOT NULL` |
| `UNIQUE` | No duplicate values | `email VARCHAR UNIQUE` |
| `PRIMARY KEY` | NOT NULL + UNIQUE | `id SERIAL PRIMARY KEY` |
| `FOREIGN KEY` | References another table | `ref INTEGER REFERENCES t(id)` |
| `CHECK` | Custom validation | `CHECK (price > 0)` |
| `DEFAULT` | Default value | `DEFAULT NOW()` |
| `EXCLUDE` | Conflict prevention | `EXCLUDE USING gist (...)` |
