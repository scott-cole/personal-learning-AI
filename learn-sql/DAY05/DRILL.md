# DAY05 — Drill: Design Tables with Proper Types

## Setup

Create a fresh database called `inventory`:

```sql
CREATE DATABASE inventory;
\c inventory
```

## Tasks

### 1. Create an `employees` table

Include:
- `id` — auto-incrementing primary key
- `first_name` — variable text, max 100
- `last_name` — variable text, max 100
- `email` — variable text, max 255
- `salary` — numeric with 2 decimal places
- `is_full_time` — boolean
- `hire_date` — date only
- `last_login` — timestamp with timezone

Write the CREATE TABLE statement.

### 2. Insert three employees

Use appropriate values for each data type.

### 3. Create a `products` table

Include:
- `id` — auto-incrementing primary key
- `name` — variable text, max 200
- `price` — numeric with 2 decimal places
- `in_stock` — integer (count, not boolean)
- `description` — unlimited text
- `created_at` — timestamp with timezone, default to NOW()

### 4. Insert three products

### 5. Experiment with type errors

Try inserting these and observe the errors:

```sql
-- What happens?
INSERT INTO employees (salary) VALUES ('not-a-number');

-- What about this?
INSERT INTO products (price) VALUES (123.456);
```

### 6. Use type casting

```sql
SELECT '42'::INTEGER + 8;

SELECT '2024-01-01'::DATE - '2023-01-01'::DATE AS days_between;
```

## Hints

- Use `DEFAULT NOW()` for `created_at`
- `TIMESTAMPTZ` is shorthand for `TIMESTAMP WITH TIME ZONE`
- `NUMERIC(10, 2)` means 10 total digits, 2 after the decimal
