# DAY13 — GROUP BY and HAVING

## Anecdote: GROUP BY is Sorting Laundry; HAVING is Checking Each Pile's Size

Laundry day. You dump everything on the bed: socks, shirts, towels, sheets. Before washing, you sort them into piles — all whites together, all darks together, all delicates together.

That's `GROUP BY`. It takes all the rows and groups them by a column value. Instead of seeing every sock individually, you see one pile per colour.

Then you look at each pile and ask: "Is this pile big enough to run a full load?" If a pile has fewer than 3 items, you don't wash it separately. That's `HAVING` — it filters groups (not individual rows) based on aggregate values.

## GROUP BY

Groups rows that share the same value in the specified column:

```sql
SELECT category, COUNT(*)
FROM products
GROUP BY category;
```

```
  category   | count
-------------+-------
 Electronics |     3
 Furniture   |     2
 Stationery  |     2
```

## GROUP BY with Multiple Aggregates

```sql
SELECT
    category,
    COUNT(*) AS products,
    AVG(price) AS avg_price,
    MAX(price) AS max_price
FROM products
GROUP BY category;
```

## GROUP BY Multiple Columns

```sql
SELECT category, shelf, COUNT(*)
FROM books
GROUP BY category, shelf;
```

Groups by the combination of category AND shelf.

## SELECT Rules with GROUP BY

Any column in SELECT must either be:
1. Listed in the GROUP BY clause, OR
2. Used in an aggregate function

```sql
-- This WORKS: category is in GROUP BY
SELECT category, COUNT(*) FROM products GROUP BY category;

-- This FAILS: product is not in GROUP BY or in an aggregate
SELECT product, category, COUNT(*) FROM products GROUP BY category;
```

## HAVING — Filter Groups

`WHERE` filters **rows** before grouping. `HAVING` filters **groups** after grouping:

```sql
SELECT category, COUNT(*) AS cnt
FROM products
GROUP BY category
HAVING COUNT(*) >= 2;
```

Only categories with 2 or more products appear.

## WHERE vs HAVING

```sql
-- WHERE filters rows first, then groups the remaining
SELECT category, AVG(price) AS avg_price
FROM products
WHERE price > 10
GROUP BY category;

-- HAVING filters groups after aggregation
SELECT category, AVG(price) AS avg_price
FROM products
GROUP BY category
HAVING AVG(price) > 100;
```

You can use both together:

```sql
SELECT category, COUNT(*) AS cnt
FROM products
WHERE price > 10
GROUP BY category
HAVING COUNT(*) >= 2;
```

## Order of Execution

```
FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT
```

This is why:
- `WHERE` can't use aggregates (they don't exist yet)
- `HAVING` can use aggregates (groups are formed)
- You can `ORDER BY` aggregates: `ORDER BY COUNT(*) DESC`

## Summary

| Clause | Purpose | When It Runs | Can Use Aggregates? |
|--------|---------|-------------|---------------------|
| `WHERE` | Filter rows | Before GROUP BY | No |
| `GROUP BY` | Form groups | After WHERE | No (but defines groups) |
| `HAVING` | Filter groups | After GROUP BY | Yes |
| `ORDER BY` | Sort results | After SELECT | Yes |
