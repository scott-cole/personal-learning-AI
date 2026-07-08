# DAY17: Migrations & Schema Design — "Blueprint Changes"

---

## The anecdote: "A migration is a blueprint revision"

You built a house. Now you want to add a window. You don't tear down the house and rebuild. You draw a revision to the blueprint and execute it.

Migrations are blueprint revisions for your database:

```
Migration 001: Create the urls table
Migration 002: Add access_count column
Migration 003: Add an index on code
```

Each migration is a file. Applied in order. Never changed after creation.

---

## Why migrations matter

Without migrations:

```go
// main.go
db.Exec("CREATE TABLE urls (...)")
```

You change the struct, edit the CREATE TABLE, delete the DB, re-run. Lose all data.

With migrations:

```
001_create_urls.sql  → applied
002_add_index.sql    → applied
003_add_owner.sql    → pending
```

You apply `003`, your data is still there. Your teammate runs the same files, same result.

---

## A simple migration system

Each migration is a numbered file:

```
migrations/
├── 001_create_urls.sql
├── 002_add_index.sql
└── 003_add_access_count.sql
```

Plus a `schema_migrations` table that tracks what's been applied:

```sql
CREATE TABLE schema_migrations (
    version INTEGER PRIMARY KEY,
    applied_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## Designing the URL shortener schema

Current model:

```go
type URL struct {
    Code         string
    OriginalURL  string
    CreatedAt    time.Time
    AccessCount  int
}
```

SQL schema:

```sql
CREATE TABLE urls (
    code         TEXT PRIMARY KEY,  -- code IS the primary key
    original_url TEXT NOT NULL,
    access_count INTEGER NOT NULL DEFAULT 0,
    created_at   TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_urls_code ON urls(code);
```

---

## Senior mindset: "Design for queries, not storage"

Don't ask "what data do I store?" Ask "what queries will I run?"

```
Query: "Find URL by code"        → index on code
Query: "Count URLs by date"      → index on created_at
Query: "Find URLs with most hits" → index on access_count
```

Design your schema around your queries, not the other way around.

---

## Summary

| Concept | Meaning |
|---------|---------|
| Migration | A versioned SQL file that changes the schema |
| schema_migrations | Table that tracks what's been applied |
| Idempotent | Can be run multiple times safely (IF NOT EXISTS) |
| Index | Speeds up queries, slows down writes |

Open DAY17/DRILL.md.
