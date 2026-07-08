# DAY18 — String Functions: Scissors and Glue

## Anecdote: String Functions are Scissors and Glue

You're crafting a scrapbook. You have magazine clippings, photos, labels, and captions. You need:
- **Scissors** to cut text into pieces (SUBSTRING, TRIM, LEFT, RIGHT)
- **Glue** to stick pieces together (CONCAT, ||
- **Ruler** to measure how long things are (LENGTH)
- **Paint** to change how text looks (UPPER, LOWER, INITCAP)
- **White-out** to fix mistakes (REPLACE, TRIM)

PostgreSQL's string functions give you the same toolkit. Raw text is messy — names have inconsistent casing, strings have extra spaces, data needs reformatting. String functions clean it all up.

## Concatenation

```sql
-- Standard SQL: ||
SELECT 'Hello' || ' ' || 'World';
-- Result: 'Hello World'

-- PostgreSQL CONCAT (handles NULLs gracefully)
SELECT CONCAT('Hello', ' ', 'World');
-- Result: 'Hello World'

-- CONCAT with separator
SELECT CONCAT_WS(', ', 'Apple', 'Banana', NULL, 'Cherry');
-- Result: 'Apple, Banana, Cherry' (skips NULL)
```

## Case Conversion

```sql
SELECT UPPER('hello');    -- 'HELLO'
SELECT LOWER('HELLO');    -- 'hello'
SELECT INITCAP('hello world');  -- 'Hello World'
```

## Length

```sql
SELECT LENGTH('Hello');     -- 5
SELECT CHAR_LENGTH('Hello'); -- 5 (same)
SELECT OCTET_LENGTH('Hello'); -- 5 (bytes, same for ASCII)
```

## Substring

```sql
SELECT SUBSTRING('PostgreSQL' FROM 1 FOR 4);  -- 'Post'
SELECT SUBSTRING('PostgreSQL' FROM 5);          -- 'greSQL'
SELECT SUBSTRING('PostgreSQL' FROM 9 FOR 2);    -- 'QL'

-- Using position with regex
SELECT SUBSTRING('abc123def' FROM '[0-9]+');    -- '123'
```

## REPLACE

```sql
SELECT REPLACE('Hello World', 'World', 'SQL');
-- 'Hello SQL'

SELECT REPLACE('banana', 'a', 'o');
-- 'bonono'
```

## TRIM, LTRIM, RTRIM

```sql
SELECT TRIM('  hello  ');          -- 'hello'
SELECT LTRIM('  hello  ');         -- 'hello  '
SELECT RTRIM('  hello  ');         -- '  hello'
SELECT TRIM(LEADING 'x' FROM 'xxhelloxx');  -- 'helloxx'
SELECT TRIM(TRAILING 'x' FROM 'xxhelloxx'); -- 'xxhello'
SELECT TRIM(BOTH 'x' FROM 'xxhelloxx');     -- 'hello'
```

## POSITION

```sql
SELECT POSITION('World' IN 'Hello World');   -- 7
SELECT STRPOS('Hello World', 'World');        -- 7 (same)
```

## LEFT and RIGHT

```sql
SELECT LEFT('PostgreSQL', 4);   -- 'Post'
SELECT RIGHT('PostgreSQL', 3);  -- 'QL'
```

## REVERSE

```sql
SELECT REVERSE('hello');  -- 'olleh'
```

## REPEAT

```sql
SELECT REPEAT('Ha', 3);  -- 'HaHaHa'
```

## Formatting

```sql
SELECT FORMAT('Hello %s, your total is $%.2f', 'Alice', 123.456);
-- 'Hello Alice, your total is $123.46'
```

## Summary

| Function | Purpose | Example |
|----------|---------|---------|
| `CONCAT(a, b)` | Join strings, NULL-safe | `CONCAT('a', 'b')` |
| `UPPER(s)` | Uppercase | `'HELLO'` |
| `LOWER(s)` | Lowercase | `'hello'` |
| `LENGTH(s)` | Character count | `5` |
| `SUBSTRING(s FROM n FOR m)` | Extract part | `'Post'` |
| `REPLACE(s, from, to)` | Find and replace | `'Hello SQL'` |
| `TRIM(s)` | Remove spaces | `'hello'` |
| `POSITION(x IN s)` | Find position | `7` |
