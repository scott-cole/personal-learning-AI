# DAY17 — UNION, INTERSECT, EXCEPT (Set Operations)

## Anecdote: Set Operations are Blending Smoothie Ingredients

Imagine you have two bowls of fruit. Bowl A has apples, bananas, and cherries. Bowl B has bananas, cherries, and dates.

- **UNION** — dump both bowls together, remove any duplicates. You get apples, bananas, cherries, dates. A combined fruit salad.
- **UNION ALL** — dump both bowls together, keep everything. You get apples, bananas, cherries, bananas, cherries, dates. (Bananas and cherries appear twice.)
- **INTERSECT** — what's in both bowls? Only bananas and cherries. The overlap.
- **EXCEPT** — what's in Bowl A that's NOT in Bowl B? Only apples. The unique-to-A items.

SQL set operations combine rows from two or more queries. Each query must return the **same number of columns** with **compatible data types**.

## UNION

Combines results, removing duplicates:

```sql
SELECT name FROM employees
UNION
SELECT name FROM former_employees;
```

Returns every name that appears in either table, with no duplicates.

## UNION ALL

Combines results, keeping duplicates:

```sql
SELECT city FROM customers
UNION ALL
SELECT city FROM suppliers;
```

If someone is in both tables, they appear twice.

## INTERSECT

Returns rows that appear in **both** queries:

```sql
SELECT product_id FROM january_sales
INTERSECT
SELECT product_id FROM february_sales;
```

Products sold in both months.

## EXCEPT

Returns rows from the first query that are **not** in the second:

```sql
SELECT product_id FROM full_catalogue
EXCEPT
SELECT product_id FROM discontinued_products;
```

Products that are in the catalogue but not discontinued.

## Rules

```sql
-- Column count must match:
SELECT id, name FROM employees
UNION
SELECT id, name FROM former_employees;  -- OK (2 columns)

SELECT id, name FROM employees
UNION
SELECT name FROM former_employees;      -- Error! Column count mismatch

-- Column types must be compatible:
SELECT name FROM employees
UNION
SELECT hire_date FROM employees;        -- Error! TEXT vs DATE
```

## Ordering Set Results

Apply `ORDER BY` at the very end of the combined query:

```sql
SELECT name, 'Current' AS status FROM employees
UNION ALL
SELECT name, 'Former' AS status FROM former_employees
ORDER BY name;
```

## Practical Example

```sql
CREATE TABLE current_students (name VARCHAR(100));
CREATE TABLE alumni (name VARCHAR(100));

INSERT INTO current_students VALUES ('Alice'), ('Bob'), ('Charlie');
INSERT INTO alumni VALUES ('Charlie'), ('Diana'), ('Eve');

-- All unique people associated with the school:
SELECT name FROM current_students
UNION
SELECT name FROM alumni;

-- People who are both students AND alumni (maybe grad students):
SELECT name FROM current_students
INTERSECT
SELECT name FROM alumni;

-- Current students who were never alumni (new students):
SELECT name FROM current_students
EXCEPT
SELECT name FROM alumni;
```

## Summary

| Operation | What It Returns | Duplicates? |
|-----------|----------------|-------------|
| `UNION` | Rows from A or B | Removed |
| `UNION ALL` | Rows from A or B | Kept |
| `INTERSECT` | Rows in both A and B | Removed |
| `EXCEPT` | Rows in A but not B | Removed |
