# DAY02 — WHERE Clauses: Comparison and Logical Operators

## Anecdote: WHERE is a Filter Funnel

Picture a hardware store that sells nuts and bolts. Behind the counter is a massive bin room filled with thousands of mixed fasteners. A customer walks in and says, *"I need 10mm hex bolts, stainless steel."*

The clerk doesn't dump the entire bin room on the counter. They use a **filter funnel**: first they strain out everything that isn't a bolt, then everything that isn't 10mm, then everything that isn't stainless steel. By the third pour, only the exact items remain.

`WHERE` is that filter funnel. It sits between you and the table and lets you sift through rows, keeping only the ones that meet your conditions. Without `WHERE`, you'd always get everything — the entire bin room on the counter, every single time.

## The WHERE Clause

`WHERE` filters rows **before** they're returned:

```sql
SELECT * FROM books WHERE year > 1980;
```

Only books published after 1980 will appear.

## Comparison Operators

| Operator | Meaning |
|----------|---------|
| `=` | Equal to |
| `<>` or `!=` | Not equal to |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal |
| `<=` | Less than or equal |

```sql
-- Books published in exactly 1984
SELECT * FROM books WHERE year = 1984;

-- Books NOT by Frank Herbert
SELECT * FROM books WHERE author <> 'Frank Herbert';

-- Books published after 1970
SELECT * FROM books WHERE year > 1970;
```

## Text with WHERE

String comparisons are case-sensitive in PostgreSQL by default:

```sql
-- Exact match
SELECT * FROM books WHERE title = 'Dune';

-- Case-insensitive (using ILIKE, which we'll cover later)
SELECT * FROM books WHERE title ILIKE 'dune';
```

## AND / OR / NOT

Combine multiple conditions:

```sql
-- Both conditions must be true
SELECT * FROM books WHERE year > 1970 AND shelf = 'A-12';

-- Either condition can be true
SELECT * FROM books WHERE year = 1965 OR year = 1984;

-- Negate a condition
SELECT * FROM books WHERE NOT year < 1980;
-- same as: year >= 1980
```

## Operator Precedence

`NOT` has the highest precedence, then `AND`, then `OR`. Use parentheses to be explicit:

```sql
-- Without parentheses: this means year > 1980 AND (shelf = 'A-12' OR shelf = 'B-07')
SELECT * FROM books WHERE year > 1980 AND shelf = 'A-12' OR shelf = 'B-07';

-- With parentheses to group OR first:
SELECT * FROM books WHERE year > 1980 AND (shelf = 'A-12' OR shelf = 'B-07');
```

These return different results! Always use parentheses when mixing `AND` and `OR`.

## IN, BETWEEN, LIKE

These are shorthand for common WHERE patterns:

```sql
-- IN: match any value in a list
SELECT * FROM books WHERE shelf IN ('A-12', 'B-07', 'C-09');

-- BETWEEN: inclusive range
SELECT * FROM books WHERE year BETWEEN 1960 AND 1980;

-- LIKE: pattern matching (% is wildcard, _ is single char)
SELECT * FROM books WHERE title LIKE 'The%';
```

## WHERE with Numbers vs. Text

```sql
-- Numbers: no quotes
SELECT * FROM books WHERE year > 2000;

-- Text: single quotes required
SELECT * FROM books WHERE author = 'Frank Herbert';

-- Aliases (AS) can't be used in WHERE — WHERE runs before SELECT
```

## Summary

| Concept | Example |
|---------|---------|
| Equal | `WHERE title = 'Dune'` |
| Not equal | `WHERE year <> 2000` |
| AND | `WHERE a AND b` |
| OR | `WHERE a OR b` |
| NOT | `WHERE NOT a` |
| IN list | `WHERE x IN (1, 2, 3)` |
| Range | `WHERE x BETWEEN a AND b` |
| Pattern | `WHERE name LIKE 'D%'` |
