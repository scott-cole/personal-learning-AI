# DAY29 — Final Project: URL Shortener

## Anecdote: Your fourth workshop build

You've built a stool (Day 7), a tool rack (Day 14), and a radio (Day 21). Now it's time to build something you'd actually use every day — a **URL shortener**.

Think of it as a *tiny address book*. You give it a long, messy URL like `https://example.com/very/long/path?with=many&params=true`, and it gives you a short code like `abc123`. When someone visits `http://localhost:5000/abc123`, it looks up the original URL and redirects them there.

You'll build this with Flask (the web layer) and SQLite (the storage). It's the perfect capstone project for the production week — it ties together routing, templates, databases, and HTTP.

---

## How a URL shortener works

1. User submits a long URL via a form
2. Server generates a unique short code (e.g., 6 random characters)
3. Server stores the mapping in SQLite: `short_code → original_url`
4. Server returns the short URL to the user
5. When someone visits `/{short_code}`, the server looks it up and redirects (HTTP 302)

### Optional tracking
- Count how many times each short URL was visited
- Show a list of all created short URLs

---

## Project structure

```
url_shortener/
├── app.py
├── templates/
│   ├── index.html      — main page with form
│   └── stats.html      — list of all short URLs
└── shortener.db        — created automatically
```

---

## Database schema

```sql
CREATE TABLE IF NOT EXISTS urls (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    short_code TEXT UNIQUE NOT NULL,
    original_url TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    visits INTEGER DEFAULT 0
)
```

---

## Core functions

```python
import sqlite3
import string
import random

DB_NAME = "shortener.db"

def get_db():
    conn = sqlite3.connect(DB_NAME)
    conn.row_factory = sqlite3.Row
    return conn

def init_db():
    with get_db() as conn:
        conn.execute("""
            CREATE TABLE IF NOT EXISTS urls (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                short_code TEXT UNIQUE NOT NULL,
                original_url TEXT NOT NULL,
                created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
                visits INTEGER DEFAULT 0
            )
        """)

def generate_short_code(length=6):
    chars = string.ascii_letters + string.digits
    return ''.join(random.choices(chars, k=length))

def create_short_url(original_url):
    short_code = generate_short_code()
    with get_db() as conn:
        conn.execute(
            "INSERT INTO urls (short_code, original_url) VALUES (?, ?)",
            (short_code, original_url)
        )
    return short_code

def get_original_url(short_code):
    with get_db() as conn:
        row = conn.execute(
            "SELECT original_url FROM urls WHERE short_code = ?",
            (short_code,)
        ).fetchone()
        if row:
            conn.execute(
                "UPDATE urls SET visits = visits + 1 WHERE short_code = ?",
                (short_code,)
            )
            return row["original_url"]
    return None

def get_all_urls():
    with get_db() as conn:
        rows = conn.execute(
            "SELECT * FROM urls ORDER BY created_at DESC"
        ).fetchall()
        return [dict(r) for r in rows]
```

---

## Flask routes

```python
from flask import Flask, render_template, request, redirect, url_for, flash

app = Flask(__name__)
app.secret_key = "your-secret-key-here"

@app.route("/", methods=["GET", "POST"])
def index():
    if request.method == "POST":
        original_url = request.form.get("url")
        if original_url:
            short_code = create_short_url(original_url)
            short_url = url_for("redirect_url", short_code=short_code, _external=True)
            return render_template("index.html", short_url=short_url)
    return render_template("index.html", short_url=None)

@app.route("/<short_code>")
def redirect_url(short_code):
    original = get_original_url(short_code)
    if original:
        return redirect(original)
    return render_template("index.html", error="Short URL not found"), 404

@app.route("/stats")
def stats():
    urls = get_all_urls()
    return render_template("stats.html", urls=urls)
```

---

## Templates

### `templates/index.html`

```html
<!DOCTYPE html>
<html>
<head>
    <title>URL Shortener</title>
</head>
<body>
    <h1>URL Shortener</h1>
    <form method="POST">
        <input type="url" name="url" placeholder="Enter a long URL" required size="50">
        <button type="submit">Shorten</button>
    </form>

    {% if short_url %}
        <p>Short URL: <a href="{{ short_url }}">{{ short_url }}</a></p>
    {% endif %}

    {% if error %}
        <p style="color: red">{{ error }}</p>
    {% endif %}

    <p><a href="{{ url_for('stats') }}">View stats</a></p>
</body>
</html>
```

### `templates/stats.html`

```html
<!DOCTYPE html>
<html>
<head>
    <title>URL Stats</title>
</head>
<body>
    <h1>All Short URLs</h1>
    <table border="1">
        <tr>
            <th>Short Code</th>
            <th>Original URL</th>
            <th>Visits</th>
            <th>Created</th>
        </tr>
        {% for url in urls %}
        <tr>
            <td><a href="{{ url_for('redirect_url', short_code=url.short_code) }}">{{ url.short_code }}</a></td>
            <td>{{ url.original_url }}</td>
            <td>{{ url.visits }}</td>
            <td>{{ url.created_at }}</td>
        </tr>
        {% endfor %}
    </table>
    <p><a href="{{ url_for('index') }}">Back</a></p>
</body>
</html>
```

---

## Running the project

```bash
python3 app.py
# Visit http://127.0.0.1:5000
```

---

## Summary

| Component | Purpose |
|-----------|---------|
| `create_short_url()` | Generate code + insert into DB |
| `get_original_url()` | Lookup + increment visit count |
| `get_all_urls()` | Stats page data |
| `GET /` | Show form |
| `POST /` | Accept URL, create short URL |
| `GET /<code>` | Redirect (302) to original |
| `/stats` | List all URLs with visit counts |
