# DAY25 — SQLite with Python

## Anecdote: SQLite is a filing cabinet in your kitchen

A big company has a server room with a database administrator, clustered databases, and replication — that's PostgreSQL or MySQL.

But you're at home. You need to file your recipes somewhere. You buy a simple metal filing cabinet, put it in the corner of your kitchen, and drop manila folders into it. No server, no admin, no network. Just you, a drawer, and some folders. That's SQLite — a full SQL database engine in a single file on your disk.

Python ships with `sqlite3` in the standard library. No installation needed.

---

## Connecting to a database

```python
import sqlite3

# Connect (creates the file if it doesn't exist)
conn = sqlite3.connect("store.db")

# Most operations need a cursor
cursor = conn.cursor()

# Close when done
conn.close()
```

---

## Creating tables

```python
conn = sqlite3.connect("store.db")
cursor = conn.cursor()

cursor.execute("""
    CREATE TABLE IF NOT EXISTS products (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT NOT NULL,
        price REAL NOT NULL,
        stock INTEGER DEFAULT 0
    )
""")

conn.commit()
conn.close()
```

---

## Parameterized queries — ALWAYS use these

Never use f-strings to build SQL queries. Use `?` placeholders.

```python
# ❌ DANGEROUS — SQL injection vulnerability
# cursor.execute(f"INSERT INTO products VALUES ({name}, {price})")

# ✅ Safe — parameterized query
cursor.execute(
    "INSERT INTO products (name, price, stock) VALUES (?, ?, ?)",
    ("Widget", 12.50, 100)
)
conn.commit()
```

---

## CRUD operations

### CREATE

```python
def add_product(name, price, stock=0):
    cursor.execute(
        "INSERT INTO products (name, price, stock) VALUES (?, ?, ?)",
        (name, price, stock)
    )
    conn.commit()
    return cursor.lastrowid  # returns the new product ID
```

### READ

```python
# Get all
cursor.execute("SELECT * FROM products")
rows = cursor.fetchall()  # list of tuples

# Get one
cursor.execute("SELECT * FROM products WHERE id = ?", (product_id,))
product = cursor.fetchone()  # single tuple or None

# With column names
cursor.row_factory = sqlite3.Row
cursor.execute("SELECT * FROM products")
for row in cursor.fetchall():
    print(row["name"], row["price"])
```

### UPDATE

```python
cursor.execute(
    "UPDATE products SET price = ? WHERE id = ?",
    (15.00, 1)
)
conn.commit()
```

### DELETE

```python
cursor.execute("DELETE FROM products WHERE id = ?", (product_id,))
conn.commit()
```

---

## Using a context manager

The connection can be used as a context manager. If the block succeeds, it commits. If it raises, it rolls back.

```python
conn = sqlite3.connect("store.db")

with conn:
    conn.execute("INSERT INTO products (name, price) VALUES (?, ?)", ("Gadget", 8.99))
    conn.execute("INSERT INTO products (name, price) VALUES (?, ?)", ("Doohickey", 15.50))
# Auto-committed on success
```

---

## A complete example

```python
import sqlite3

def init_db():
    with sqlite3.connect("store.db") as conn:
        conn.execute("""
            CREATE TABLE IF NOT EXISTS products (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                name TEXT NOT NULL,
                price REAL NOT NULL,
                stock INTEGER DEFAULT 0
            )
        """)

def add_product(name, price, stock=0):
    with sqlite3.connect("store.db") as conn:
        cursor = conn.execute(
            "INSERT INTO products (name, price, stock) VALUES (?, ?, ?)",
            (name, price, stock)
        )
        return cursor.lastrowid

def list_products():
    with sqlite3.connect("store.db") as conn:
        conn.row_factory = sqlite3.Row
        rows = conn.execute("SELECT * FROM products").fetchall()
        return [dict(row) for row in rows]
```

---

## SQLite data types

| SQLite type | Python type |
|-------------|-------------|
| `NULL` | `None` |
| `INTEGER` | `int` |
| `REAL` | `float` |
| `TEXT` | `str` |
| `BLOB` | `bytes` |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| `sqlite3.connect()` | Open (or create) a database file |
| `cursor.execute()` | Run a SQL statement |
| `?` placeholders | Parameterized queries — prevent SQL injection |
| `fetchall()` / `fetchone()` | Get results |
| `conn.commit()` | Save changes |
| `conn` as context manager | Auto-commit/rollback |
| `sqlite3.Row` | Access columns by name |
| No server needed | File-based, zero configuration |
