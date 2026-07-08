# DAY05 — Data Types: The Right Container for the Right Job

## Anecdote: Data Types are Labelled Containers

Walk into any organised kitchen and you'll see a system of labelled containers. Flour is in a big bin with a wide mouth. Olive oil is in a dark glass bottle. Spices are in tiny jars. Eggs are in a carton that says "FRAGILE."

You would never store olive oil in a flour bin — it would leak everywhere. You wouldn't put eggs loose in the spice drawer — they'd crack.

**Data types** are those labelled containers. PostgreSQL gives you a rich set of containers, each designed for a specific kind of content. Put the right data in the right container and everything works. Put text in a number column and PostgreSQL will refuse to store it — just like your kitchen refuses to store milk in a bread box.

## PostgreSQL Data Types Overview

### Numeric Types

```sql
-- Integer (whole numbers, no decimal)
CREATE TABLE example_int (
    count INTEGER,          -- 4 bytes, ±2.1 billion
    small SMALLINT,         -- 2 bytes, ±32,767
    big BIGINT              -- 8 bytes, ±9 quintillion
);

-- Auto-incrementing integers
CREATE TABLE example_serial (
    id SERIAL,              -- Equivalent to INTEGER with auto-increment
    id2 BIGSERIAL           -- BIGINT with auto-increment
);

-- Exact decimal (for money, precise calculations)
CREATE TABLE example_numeric (
    price NUMERIC(10, 2),   -- 10 digits total, 2 after decimal point
    rating NUMERIC(3, 1)    -- e.g. 9.5
);

-- Floating point (approximate, for scientific)
CREATE TABLE example_float (
    temp FLOAT,             -- 8 bytes
    ratio REAL              -- 4 bytes
);
```

### Character Types

```sql
CREATE TABLE example_text (
    title VARCHAR(200),      -- Variable length, max 200
    name VARCHAR,            -- Variable length, no limit (same as TEXT)
    bio TEXT,                -- Unlimited length
    code CHAR(10)            -- Fixed length, padded with spaces
);
```

**Rule of thumb:** Use `VARCHAR(n)` when you need a length limit (e.g., usernames). Use `TEXT` when there's no practical limit. Never use `CHAR` unless you're storing fixed-length codes.

### Boolean

```sql
CREATE TABLE example_bool (
    is_active BOOLEAN,       -- TRUE, FALSE, or NULL
    is_admin BOOLEAN DEFAULT FALSE
);

INSERT INTO example_bool VALUES (TRUE, FALSE);
-- PostgreSQL accepts: TRUE, 'true', 't', '1', YES, etc.
```

### Date and Time

```sql
CREATE TABLE example_dates (
    born DATE,                     -- Just the date (no time)
    login TIMESTAMP,               -- Date + time (no timezone)
    login_tz TIMESTAMPTZ,          -- Date + time with timezone
    duration INTERVAL,             -- A span of time (e.g. '2 days')
    start_time TIME                -- Just the time
);

INSERT INTO example_dates VALUES (
    '1990-01-15',
    '2024-03-15 14:30:00',
    '2024-03-15 14:30:00+05:30',
    '2 days 3 hours',
    '09:00:00'
);
```

### Other Useful Types

```sql
CREATE TABLE example_other (
    data JSONB,              -- JSON data (binary, queryable)
    id UUID DEFAULT gen_random_uuid(),  -- Universally unique ID
    url TEXT,
    ip INET                  -- IPv4 or IPv6 address
);
```

## Type Casting

Sometimes you need to convert between types:

```sql
-- Cast text to integer
SELECT '123'::INTEGER;

-- Cast integer to text
SELECT 42::TEXT;

-- Standard SQL syntax
SELECT CAST(123.45 AS INTEGER);
```

## Choosing the Right Type

| Data | Wrong Type | Right Type |
|------|-----------|------------|
| User's age | TEXT | SMALLINT |
| Book price | FLOAT | NUMERIC(10,2) |
| Description | VARCHAR(50) | TEXT |
| Is admin? | INTEGER | BOOLEAN |
| Birth date | VARCHAR | DATE |

## Summary

| Category | Types | Use Case |
|----------|-------|----------|
| Integers | `SMALLINT`, `INTEGER`, `BIGINT` | Counts, IDs, ages |
| Auto-ID | `SERIAL`, `BIGSERIAL` | Primary keys |
| Decimals | `NUMERIC(p,s)` | Money, precise values |
| Text | `VARCHAR(n)`, `TEXT` | Names, descriptions |
| Boolean | `BOOLEAN` | True/false flags |
| Date/Time | `DATE`, `TIMESTAMP`, `TIMESTAMPTZ`, `INTERVAL` | Events, durations |
| JSON | `JSONB` | Flexible nested data |
