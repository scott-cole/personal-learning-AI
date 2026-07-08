# DAY15: SQL Fundamentals — "A Spreadsheet You Can Ask Questions Of"

---

## The anecdote: "SQL is a spreadsheet with a phone line"

You know Excel/Google Sheets. A table is a sheet. A row is one record. A column is one field.

```
Sheet: urls
| code   | original_url                  | created_at          |
|--------|-------------------------------|---------------------|
| abc123 | https://example.com           | 2026-06-29 12:00:00 |
| def456 | https://google.com            | 2026-06-29 12:05:00 |
```

SQL lets you ask questions:

```
SELECT original_url FROM urls WHERE code = 'abc123';
→ https://example.com
```

No clicking. No scrolling. Just ask.

---

## The four commands you'll use 90% of the time

```sql
SELECT  -- read data
INSERT  -- create data
UPDATE  -- update data
DELETE  -- remove data
```

---

## SELECT

```sql
-- Get everything
SELECT * FROM urls;

-- Get specific columns
SELECT code, original_url FROM urls;

-- Filter with WHERE
SELECT * FROM urls WHERE code = 'abc123';

-- Count
SELECT COUNT(*) FROM urls;

-- Like (partial match)
SELECT * FROM urls WHERE original_url LIKE '%example%';
```

## INSERT

```sql
-- Insert a row
INSERT INTO urls (code, original_url, created_at)
VALUES ('abc123', 'https://example.com', '2026-06-29 12:00:00');
```

## UPDATE

```sql
-- Update a row
UPDATE urls SET access_count = 1 WHERE code = 'abc123';
```

## DELETE

```sql
-- Delete a row
DELETE FROM urls WHERE code = 'abc123';
```

---

## Data types

```sql
CREATE TABLE urls (
    id          INTEGER PRIMARY KEY AUTOINCREMENT,
    code        TEXT NOT NULL UNIQUE,
    original_url TEXT NOT NULL,
    access_count INTEGER DEFAULT 0,
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

| SQL type | Go type | Notes |
|----------|---------|-------|
| INTEGER | int, int64 | Also used for booleans (0/1) |
| TEXT | string | Variable length |
| REAL | float64 | Decimal numbers |
| TIMESTAMP | time.Time | Date and time |
| BLOB | []byte | Binary data |

---

## Why NULL is dangerous

In SQL, `NULL` is not `0` or `""`. It's "unknown."

```sql
SELECT * FROM urls WHERE access_count = 0;
-- Does NOT match rows where access_count IS NULL

SELECT * FROM urls WHERE access_count IS NULL;
-- This one does
```

Always use `NOT NULL` + `DEFAULT` in your schema to avoid NULLs:

```sql
access_count INTEGER NOT NULL DEFAULT 0
```

---

## Senior mindset: "SELECT * is fine in drills, name columns in production"

```sql
-- ❌ Don't do this in production code
SELECT * FROM urls;

-- ✅ Name what you need
SELECT code, original_url FROM urls;
```

Why? Your Go code depends on column order and existence. Renaming a column in SQL breaks `SELECT *` silently. Naming columns makes the contract explicit.

---

## Summary

| Command | Purpose |
|---------|---------|
| SELECT | Read data |
| INSERT | Create data |
| UPDATE | Update data |
| DELETE | Remove data |
| WHERE | Filter rows |
| JOIN | Combine tables (later) |

Open DAY15/DRILL.md.

To run SQLite: `sqlite3 test.db` (install with `brew install sqlite` if needed).
