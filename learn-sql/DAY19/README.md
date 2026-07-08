# DAY19 — Date and Time Functions

## Anecdote: Date Functions are a Calendar and a Clock

You're planning a road trip. You need to know:
- What's the date today? (NOW, CURRENT_DATE)
- What month is it? (EXTRACT)
- How many days until the trip? (Interval arithmetic)
- What's the first day of the month? (DATE_TRUNC)
- What day of the week does the trip start? (EXTRACT DOW)

PostgreSQL's date/time functions are your calendar and clock. They let you slice time into pieces, measure durations, and round dates to any precision.

PostgreSQL is famous for its excellent date/time support. Let's dive in.

## Getting Current Date/Time

```sql
SELECT NOW();               -- 2024-03-15 14:30:00.123456+00
SELECT CURRENT_DATE;        -- 2024-03-15
SELECT CURRENT_TIME;        -- 14:30:00.123456+00
SELECT CURRENT_TIMESTAMP;   -- 2024-03-15 14:30:00.123456+00
```

## Date/Time Arithmetic

```sql
-- Add/subtract intervals
SELECT CURRENT_DATE + INTERVAL '1 day';
SELECT CURRENT_DATE - INTERVAL '7 days';
SELECT NOW() + INTERVAL '3 hours';
SELECT NOW() - INTERVAL '30 minutes';

-- Difference between dates (returns integer)
SELECT DATE '2024-03-20' - DATE '2024-03-15';  -- 5

-- Difference between timestamps (returns interval)
SELECT TIMESTAMP '2024-03-20 10:00:00' - TIMESTAMP '2024-03-15 14:00:00';
-- Returns: '4 days 20:00:00'
```

## EXTRACT — Get Components

```sql
SELECT EXTRACT(YEAR FROM NOW());
SELECT EXTRACT(MONTH FROM NOW());
SELECT EXTRACT(DAY FROM NOW());
SELECT EXTRACT(HOUR FROM NOW());
SELECT EXTRACT(MINUTE FROM NOW());
SELECT EXTRACT(DOW FROM NOW());        -- Day of week (0=Sunday, 6=Saturday)
SELECT EXTRACT(DOY FROM NOW());        -- Day of year (1-366)
SELECT EXTRACT(QUARTER FROM NOW());    -- 1-4
SELECT EXTRACT(EPOCH FROM NOW());      -- Unix timestamp (seconds since 1970)
```

## DATE_TRUNC — Round Down

```sql
SELECT DATE_TRUNC('year', NOW());       -- 2024-01-01 00:00:00
SELECT DATE_TRUNC('month', NOW());      -- 2024-03-01 00:00:00
SELECT DATE_TRUNC('day', NOW());        -- 2024-03-15 00:00:00
SELECT DATE_TRUNC('hour', NOW());       -- 2024-03-15 14:00:00
SELECT DATE_TRUNC('week', NOW());       -- Monday of current week
```

`DATE_TRUNC` is invaluable for GROUP BY queries over time periods:

```sql
SELECT
    DATE_TRUNC('month', order_date) AS month,
    SUM(total) AS revenue
FROM orders
GROUP BY DATE_TRUNC('month', order_date)
ORDER BY month;
```

## AGE — Human-Readable Duration

```sql
SELECT AGE(TIMESTAMP '2024-03-15', TIMESTAMP '1990-01-15');
-- Returns: '34 years 2 mons 0 days'

SELECT AGE(CURRENT_DATE, '1990-01-15');  -- Age from birthdate
```

## TO_CHAR — Formatting

```sql
SELECT TO_CHAR(NOW(), 'YYYY-MM-DD');         -- '2024-03-15'
SELECT TO_CHAR(NOW(), 'Mon DD, YYYY');       -- 'Mar 15, 2024'
SELECT TO_CHAR(NOW(), 'Day, Month DD');      -- 'Friday, March 15'
SELECT TO_CHAR(NOW(), 'HH12:MI AM');         -- '02:30 PM'
```

## Creating Dates

```sql
SELECT MAKE_DATE(2024, 3, 15);       -- 2024-03-15
SELECT MAKE_TIMESTAMP(2024, 3, 15, 14, 30, 0);  -- 2024-03-15 14:30:00
SELECT MAKE_INTERVAL(days => 5);     -- '5 days'
```

## Time Zones

```sql
SELECT NOW() AT TIME ZONE 'UTC';
SELECT NOW() AT TIME ZONE 'America/New_York';
SELECT CURRENT_SETTING('TIMEZONE');  -- Show current timezone
```

## Summary

| Function | Purpose | Example |
|----------|---------|---------|
| `NOW()` | Current timestamp | `2024-03-15 14:30:00` |
| `CURRENT_DATE` | Current date | `2024-03-15` |
| `EXTRACT(field FROM ts)` | Get component | `EXTRACT(YEAR FROM date)` |
| `DATE_TRUNC(field, ts)` | Round down | `DATE_TRUNC('month', date)` |
| `AGE(a, b)` | Duration between | `AGE('2024-01-01', '2023-01-01')` |
| `TO_CHAR(ts, fmt)` | Format | `TO_CHAR(date, 'YYYY')` |
| `ts + INTERVAL 'n days'` | Add interval | `date + INTERVAL '7 days'` |
