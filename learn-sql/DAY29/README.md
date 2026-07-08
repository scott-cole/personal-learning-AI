# DAY29 — EXPLAIN ANALYZE: Under the Hood

## Anecdote: EXPLAIN is a Mechanic Peeking Under the Hood

Your car is making a strange noise. You take it to a mechanic. They pop the hood, peer at the engine, and say, "There's your problem — the alternator belt is loose." They didn't guess. They looked.

`EXPLAIN ANALYZE` is that mechanic's peek under the hood. When a query is slow, you don't need to guess why. PostgreSQL will show you exactly what it did: which indexes it used, how many rows it scanned, how long each step took, and where the bottlenecks are.

Without EXPLAIN, you're driving blind. With it, you're a master diagnostician.

## EXPLAIN vs EXPLAIN ANALYZE

```sql
-- Estimated plan only (no execution):
EXPLAIN SELECT * FROM books WHERE year = 1965;

-- Estimated plan + actual execution:
EXPLAIN ANALYZE SELECT * FROM books WHERE year = 1965;
```

`EXPLAIN` shows the query plan with estimated costs. `EXPLAIN ANALYZE` **actually runs the query** and shows real timings.

## Reading an EXPLAIN Output

```
                                   QUERY PLAN
-------------------------------------------------------------------------------
 Seq Scan on books  (cost=0.00..12.00 rows=1 width=100)
   Filter: (year = 1965)
```

- `Seq Scan` — sequential scan (full table scan)
- `cost=0.00..12.00` — estimated cost (first number is startup, second is total)
- `rows=1` — estimated rows returned
- `width=100` — estimated bytes per row

## After Adding an Index

```
                                   QUERY PLAN
-------------------------------------------------------------------------------
 Index Scan using idx_books_year on books  (cost=0.00..8.27 rows=1 width=100)
   Index Cond: (year = 1965)
```

`Index Scan` is much faster than `Seq Scan` for selective queries.

## Common Node Types

| Node Type | Meaning |
|-----------|---------|
| `Seq Scan` | Full table scan (slow on large tables) |
| `Index Scan` | Lookup via index |
| `Index Only Scan` | All needed data in index (no table access) |
| `Bitmap Heap Scan` | Multiple index matches combined |
| `Nested Loop` | For each row in outer, scan inner (good for small sets) |
| `Hash Join` | Build hash table of inner, probe with outer |
| `Merge Join` | Sort both sides, merge (good for already-sorted data) |
| `Sort` | Explicit sort (often due to ORDER BY) |
| `Aggregate` | GROUP BY or aggregate function |

## Understanding Costs

```
cost=0.00..12.00
```

- Format: `startup_cost..total_cost`
- Measured in arbitrary units (not milliseconds)
- Lower is better
- Compare costs between different query plans

## EXPLAIN ANALYZE with Actuals

```
                               QUERY PLAN
-------------------------------------------------------------------------------
 Seq Scan on books  (cost=0.00..12.00 rows=1 width=100)
   Filter: (year = 1965)
   Planning Time: 0.042 ms
   Execution Time: 0.035 ms
```

For EXPLAIN ANALYZE, you also see:
- `Planning Time` — time to create the query plan
- `Execution Time` — actual runtime

## Identifying Slow Queries

Common performance killers:
1. **Seq Scan on large table** — missing index
2. **Sort with large row count** — missing index on ORDER BY column
3. **Nested Loop with large outer set** — needs different join strategy
4. **Rows estimate way off** — stale statistics (run `ANALYZE`)

## Improving Performance

```sql
-- Analyze tables (update statistics)
ANALYZE books;

-- Add index
CREATE INDEX idx_books_year ON books (year);

-- Rewrite query (e.g., use EXISTS instead of IN for large subqueries)
-- Use CTEs carefully (materialization fence)

-- Check for sequential scans on large tables
EXPLAIN ANALYZE SELECT * FROM books WHERE year = 1965;
```

## Visual Explain Tools

- `pgAdmin` has a visual EXPLAIN diagram
- `explain.depesz.com` — paste plan for analysis
- `PEV2` — PostgreSQL explain visualizer

## Summary

| Command | What It Does |
|---------|-------------|
| `EXPLAIN` | Shows estimated query plan (safe, doesn't run query) |
| `EXPLAIN ANALYZE` | Runs query + shows actual timings |
| `ANALYZE table` | Updates statistics for the query planner |
| Seq Scan | Full table scan — bad on big tables |
| Index Scan | Index lookup — good for selective queries |
| cost | Arbitrary units, lower is better |
