# DAY10 — LEFT JOIN, RIGHT JOIN, FULL OUTER JOIN

## Anecdote: OUTER JOINs are Plus-Ones at a Wedding

You're hosting a wedding. You have two lists: the **guest list** (who was invited) and the **RSVP list** (who actually responded).

- **INNER JOIN** — only people who are on BOTH lists (invited AND responded). The reliable ones.
- **LEFT JOIN** — everyone on the guest list, plus their RSVP status if they responded. If they didn't, you see NULL for RSVP info. The guest list is the "left" table — it gets to bring all its guests even if they didn't RSVP.
- **RIGHT JOIN** — everyone who RSVP'd, plus their invitation status. Anyone who RSVP'd without being invited? Still included (maybe they're a plus-one who snuck in).
- **FULL OUTER JOIN** — everyone from both lists. Invited but no RSVP? Included. RSVP'd but not invited? Also included. Every person shows up at least once.

## LEFT JOIN

Returns ALL rows from the left table, with matching rows from the right. If no match, right-side columns are NULL:

```sql
SELECT b.title, l.member_name
FROM books b
LEFT JOIN loans l ON b.id = l.book_id;
```

Every book appears. Books with no loans have NULL in `member_name`.

## When to Use LEFT JOIN

- "Show me all products, and whether they've been ordered"
- "List all employees, and their department name (if assigned)"
- The left table is your "master" list

## RIGHT JOIN

Returns ALL rows from the right table, with matching rows from the left:

```sql
SELECT b.title, l.member_name
FROM books b
RIGHT JOIN loans l ON b.id = l.book_id;
```

Every loan appears. Loans with no matching book will have NULL title. RIGHT JOIN is less common — you can usually flip the tables and use LEFT JOIN.

## FULL OUTER JOIN

Returns ALL rows from BOTH tables. Rows without matches get NULLs:

```sql
SELECT b.title, l.member_name
FROM books b
FULL OUTER JOIN loans l ON b.id = l.book_id;
```

## Visual Comparison

Given:
- Books: {A, B, C} (3 books)
- Loans: {A, D} (2 loans, one matching book A, one with no book)

| Join Type | Result |
|-----------|--------|
| INNER JOIN | A — only A, the match |
| LEFT JOIN (books left) | A (with loan), B (NULL loan), C (NULL loan) |
| RIGHT JOIN (books left) | A (with loan), D (no book title) |
| FULL OUTER JOIN | A, B (NULL), C (NULL), D (no book title) |

## Using LEFT JOIN to Find Missing Rows

A classic pattern: find rows in the left table that have no match:

```sql
-- Books that have never been borrowed
SELECT b.*
FROM books b
LEFT JOIN loans l ON b.id = l.book_id
WHERE l.id IS NULL;
```

## Summary

| Join Type | Left Table | Right Table |
|-----------|-----------|-------------|
| INNER JOIN | Only matches | Only matches |
| LEFT JOIN | All rows | Only matches |
| RIGHT JOIN | Only matches | All rows |
| FULL OUTER JOIN | All rows | All rows |
