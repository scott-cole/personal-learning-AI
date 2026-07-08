# DAY08 — Primary Keys, Foreign Keys, REFERENCES

## Anecdote: PKs are Passport Numbers; FKs are Hotel Room Assignments

Imagine you're travelling internationally. Your **passport number** is unique to you — no one else in the world has it. When you check into a hotel, the front desk doesn't write your full name, address, and birth date on the room key card. They just write your **passport number**. Later, if someone needs to find you, they look up passport number 123456 and cross-reference it with the immigration database.

That passport number is a **primary key** (PK). It uniquely identifies one row.

The hotel's room assignment ledger doesn't store your entire life story. It stores your passport number — a **foreign key** (FK) that references the passport database. If the hotel needs your name, they follow the FK back to the PK in the other table.

This is the heart of relational databases: tables don't duplicate data. They store tiny references to each other.

## Primary Keys

A **primary key** uniquely identifies each row in a table:

```sql
CREATE TABLE authors (
    id SERIAL PRIMARY KEY,  -- Most common pattern
    name VARCHAR(100) NOT NULL
);

CREATE TABLE books (
    isbn VARCHAR(13) PRIMARY KEY,  -- Natural key (exists in real world)
    title VARCHAR(200) NOT NULL
);
```

Properties of a PK:
- Must be unique across all rows
- Cannot be NULL
- A table can have only ONE primary key
- Can be a single column or composite (multiple columns)

## Composite Primary Keys

Sometimes one column isn't enough:

```sql
CREATE TABLE book_authors (
    book_id INTEGER,
    author_id INTEGER,
    role VARCHAR(50),
    PRIMARY KEY (book_id, author_id)  -- Composite: each pair is unique
);
```

## Foreign Keys

A **foreign key** references a primary key in another table:

```sql
CREATE TABLE books (
    id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    author_id INTEGER REFERENCES authors(id)  -- FK
);
```

### ON DELETE Actions

What happens when you delete an author who has books? You have options:

```sql
CREATE TABLE books (
    id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    author_id INTEGER REFERENCES authors(id)
        ON DELETE RESTRICT,    -- Prevent delete if books exist
        ON DELETE CASCADE,     -- Delete all books too
        ON DELETE SET NULL,    -- Set author_id to NULL
        ON DELETE SET DEFAULT, -- Set to default value
        ON DELETE NO ACTION    -- Same as RESTRICT (default)
);
```

## Full Example

```sql
CREATE TABLE authors (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    birth_year INTEGER
);

CREATE TABLE books (
    id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    author_id INTEGER NOT NULL REFERENCES authors(id) ON DELETE RESTRICT,
    year INTEGER,
    CONSTRAINT fk_author FOREIGN KEY (author_id) REFERENCES authors(id)
);

-- This will fail if author 1 has books (ON DELETE RESTRICT):
DELETE FROM authors WHERE id = 1;

-- Use CASCADE to delete author AND their books:
```

## Foreign Key Conventions

- Column name typically matches the referenced table: `author_id` for `authors.id`
- Always create FKs to maintain data integrity
- FK columns can be NULL (unlike PKs)
- A table can have multiple FKs referencing different tables

## Summary

| Concept | Purpose | Example |
|---------|---------|---------|
| PRIMARY KEY | Uniquely identifies each row | `id SERIAL PRIMARY KEY` |
| FOREIGN KEY | References a PK in another table | `author_id INTEGER REFERENCES authors(id)` |
| ON DELETE CASCADE | Auto-delete related rows | `REFERENCES ... ON DELETE CASCADE` |
| ON DELETE RESTRICT | Prevent deletion if related rows exist | `REFERENCES ... ON DELETE RESTRICT` |
| Composite PK | Two columns together as unique ID | `PRIMARY KEY (a, b)` |
