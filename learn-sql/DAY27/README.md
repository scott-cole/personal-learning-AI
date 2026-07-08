# DAY27 — Window Functions: Looking Left and Right in a Queue

## Anecdote: Window Functions are Looking Left and Right in a Queue

You're standing in a queue at the post office. You can see:
- Your own position (number 7)
- The person in front of you (number 6) — `LAG`
- The person behind you (number 8) — `LEAD`
- Your rank from the front (7th) — `ROW_NUMBER`
- Your rank if people with same number of parcels are tied (tied for 3rd) — `RANK`
- Your rank if ties are collapsed (3rd, 4th, 5th are all 3rd) — `DENSE_RANK`
- What quarter of the queue you're in — `NTILE(4)`

These are **window functions**. Unlike `GROUP BY` which collapses rows into groups, window functions perform calculations **across a set of rows related to the current row** without collapsing them. Each row keeps its identity — you just get extra computed columns.

## ROW_NUMBER

Assigns a unique number to each row within a partition:

```sql
SELECT
    name,
    department,
    salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) AS rank
FROM employees;
```

```sql
-- Reset numbering per department:
SELECT
    name,
    department,
    salary,
    ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS dept_rank
FROM employees;
```

## RANK and DENSE_RANK

```sql
SELECT
    name,
    department,
    salary,
    RANK() OVER (ORDER BY salary DESC) AS rank,
    DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rank
FROM employees;
```

- `RANK`: ties get same number, next gets gap (1, 1, 3, 4)
- `DENSE_RANK`: ties get same number, next gets consecutive (1, 1, 2, 3)

## NTILE

Divides rows into N buckets:

```sql
SELECT
    name,
    salary,
    NTILE(4) OVER (ORDER BY salary DESC) AS quartile
FROM employees;
```

## LAG and LEAD

Access rows before/after the current row:

```sql
SELECT
    sale_date,
    total,
    LAG(total, 1) OVER (ORDER BY sale_date) AS prev_day_total,
    total - LAG(total, 1) OVER (ORDER BY sale_date) AS day_over_day_change
FROM daily_sales;
```

```sql
-- Compare each employee's salary to their department average
SELECT
    name,
    department,
    salary,
    AVG(salary) OVER (PARTITION BY department) AS dept_avg,
    salary - AVG(salary) OVER (PARTITION BY department) AS diff_from_avg
FROM employees;
```

## Window Frame Clause

Control exactly which rows the window includes:

```sql
SELECT
    sale_date,
    total,
    AVG(total) OVER (
        ORDER BY sale_date
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS moving_avg_3_days
FROM daily_sales;
```

Frame options:
- `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`
- `ROWS BETWEEN 7 PRECEDING AND CURRENT ROW`
- `ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING`
- `RANGE BETWEEN INTERVAL '7 days' PRECEDING AND CURRENT ROW`

## FIRST_VALUE and LAST_VALUE

```sql
SELECT
    department,
    name,
    salary,
    FIRST_VALUE(name) OVER (PARTITION BY department ORDER BY salary DESC) AS highest_paid
FROM employees;
```

## Common Patterns

**Top N per group:**

```sql
SELECT * FROM (
    SELECT *,
        ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS rn
    FROM employees
) ranked
WHERE rn <= 3;
```

**Running total:**

```sql
SELECT
    sale_date,
    total,
    SUM(total) OVER (ORDER BY sale_date) AS running_total
FROM daily_sales;
```

## Summary

| Function | Purpose |
|----------|---------|
| `ROW_NUMBER()` | Unique row number per partition |
| `RANK()` | Rank with gaps on ties |
| `DENSE_RANK()` | Rank without gaps on ties |
| `NTILE(n)` | Divide into n buckets |
| `LAG(col, n)` | Value n rows before |
| `LEAD(col, n)` | Value n rows after |
| `FIRST_VALUE(col)` | First value in window |
| `LAST_VALUE(col)` | Last value in window |
| `SUM/AVG/... OVER()` | Aggregates without collapsing |
