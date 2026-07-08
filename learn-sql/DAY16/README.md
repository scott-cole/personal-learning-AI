# DAY16 — EXISTS, NOT EXISTS, ANY, ALL

## Anecdote: EXISTS is a Bouncer with a Guest List

Picture a nightclub. The bouncer at the door has a guest list. You walk up and he asks, "Is your name on the list?" He doesn't care who you are, how many people are with you, or what you're wearing. He only cares: **does your name exist on the list?** Yes or no. You're in or you're out.

`EXISTS` is that bouncer. It checks whether a subquery returns **any rows at all**. It doesn't care what the rows contain, just whether they exist. If the subquery returns one or more rows, `EXISTS` is true.

`NOT EXISTS` is the bouncer checking the "banned" list.

`ANY` and `ALL` are comparison modifiers that let you compare a value against a list.

## EXISTS

```sql
SELECT name FROM authors a
WHERE EXISTS (
    SELECT 1 FROM books b WHERE b.author_id = a.id
);
```

Returns authors who have at least one book. The `SELECT 1` is a convention — it doesn't matter what you select, only whether rows exist.

## NOT EXISTS

```sql
SELECT name FROM authors a
WHERE NOT EXISTS (
    SELECT 1 FROM books b WHERE b.author_id = a.id
);
```

Returns authors with zero books.

## ANY

`ANY` compares a value to **any** value from a subquery or list:

```sql
-- Find products with price greater than ANY product in Electronics
-- (cheaper than the most expensive electronic)
SELECT * FROM products
WHERE price > ANY (
    SELECT price FROM products WHERE category = 'Electronics'
);

-- With a literal list:
SELECT * FROM products WHERE price > ANY (100, 200, 300);
```

`= ANY` is equivalent to `IN`:

```sql
SELECT * FROM products WHERE category = ANY (ARRAY['Electronics', 'Furniture']);
-- Same as: WHERE category IN ('Electronics', 'Furniture')
```

## ALL

`ALL` compares a value to **all** values from a subquery:

```sql
-- Find products more expensive than EVERY product in Stationery
SELECT * FROM products
WHERE price > ALL (
    SELECT price FROM products WHERE category = 'Stationery'
);
```

If the subquery returns more expensive than the most expensive stationery item.

## Comparison with IN

```sql
-- These are equivalent:
WHERE category IN ('A', 'B', 'C')
WHERE category = ANY (ARRAY['A', 'B', 'C'])

-- These are NOT the same:
WHERE price > ANY (subquery)  -- greater than at least one
WHERE price > ALL (subquery)  -- greater than every single one
```

## NULL Behavior

- `EXISTS` stops as soon as it finds one row — it's fast
- If the subquery returns an empty set, `ALL` is true (vacuously)
- `ANY` with an empty set is always false

```sql
-- If all products have NULL prices, this returns nothing:
SELECT * FROM products WHERE price > ALL (SELECT price FROM products);
```

## Performance

`EXISTS` is often faster than `IN` when the subquery is large because it short-circuits — it stops scanning as soon as it finds a match.

## Summary

| Operator | Returns TRUE when... |
|----------|---------------------|
| `EXISTS (subquery)` | Subquery returns at least one row |
| `NOT EXISTS (subquery)` | Subquery returns zero rows |
| `x = ANY (subquery)` | x equals at least one value from subquery |
| `x > ANY (subquery)` | x is greater than at least one value |
| `x > ALL (subquery)` | x is greater than every value |
