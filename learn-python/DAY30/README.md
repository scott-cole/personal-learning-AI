# DAY30 — Final Project: Polish & QA

## Anecdote: The final inspection

You've built a beautiful piece of furniture — a walnut bookshelf with dovetail joints. Before it leaves the workshop, you run your hand over every surface. Are there splinters? Are the shelves level? Does the finish have bubbles? You don't ship something with rough edges.

Today is that final inspection. You take the URL shortener from Day 29 and add the three things that separate a prototype from a production-quality tool:

1. **Tests** — prove it works
2. **Error handling** — make it robust
3. **Logging** — make it observable

And you package it with a `requirements.txt` so anyone can set it up.

---

## 1. Adding tests

Create `test_shortener.py`:

```python
import pytest
import app  # your Flask app
import tempfile
import os

@pytest.fixture
def client():
    """Create a test client for the Flask app."""
    app.app.config["TESTING"] = True
    # Use a temp database
    app.DB_NAME = "test_shortener.db"
    with app.app.test_client() as client:
        with app.app.app_context():
            app.init_db()
        yield client
    # Cleanup
    os.remove("test_shortener.db")

def test_home_page(client):
    rv = client.get("/")
    assert rv.status_code == 200
    assert b"URL Shortener" in rv.data

def test_shorten_url(client):
    rv = client.post("/", data={"url": "https://example.com"}, follow_redirects=False)
    assert rv.status_code == 200
    assert b"Short URL" in rv.data

def test_redirect(client):
    # First create a short URL
    rv = client.post("/", data={"url": "https://example.com/test"}, follow_redirects=True)
    # Extract the short code from the response and follow it
    # Check that we can access the stats page
    rv = client.get("/stats")
    assert rv.status_code == 200
    assert b"https://example.com/test" in rv.data

def test_invalid_url(client):
    rv = client.post("/", data={"url": "not-a-url"}, follow_redirects=True)
    assert rv.status_code == 200
    assert b"Invalid" in rv.data or b"valid" in rv.data
```

---

## 2. Adding error handling

```python
import re

def validate_url(url):
    """Validate URL format."""
    if not url:
        raise ValueError("URL is required")
    if not re.match(r"^https?://", url):
        raise ValueError("URL must start with http:// or https://")
    return url

# In the Flask route:
@app.route("/", methods=["GET", "POST"])
def index():
    if request.method == "POST":
        original_url = request.form.get("url", "").strip()
        try:
            original_url = validate_url(original_url)
            short_code = create_short_url(original_url)
            short_url = url_for("redirect_url", short_code=short_code, _external=True)
            return render_template("index.html", short_url=short_url)
        except ValueError as e:
            return render_template("index.html", error=str(e))
    return render_template("index.html", short_url=None)
```

### Graceful 404

```python
@app.errorhandler(404)
def not_found(e):
    return render_template("index.html", error="Page not found"), 404
```

---

## 3. Adding logging

```python
import logging

logger = logging.getLogger(__name__)
logger.setLevel(logging.DEBUG)

handler = logging.StreamHandler()
handler.setFormatter(logging.Formatter(
    "%(asctime)s [%(levelname)s] %(message)s"
))
logger.addHandler(handler)

# Usage in routes:
@app.route("/", methods=["GET", "POST"])
def index():
    logger.info("Home page accessed")
    if request.method == "POST":
        original_url = request.form.get("url", "").strip()
        logger.debug(f"Received URL: {original_url}")
        # ...
        logger.info(f"Created short URL for: {original_url[:50]}...")

@app.route("/<short_code>")
def redirect_url(short_code):
    logger.info(f"Redirect request for code: {short_code}")
    original = get_original_url(short_code)
    if original:
        logger.info(f"Redirecting {short_code} -> {original}")
        return redirect(original)
    logger.warning(f"Short code not found: {short_code}")
    return render_template("index.html", error="Short URL not found"), 404
```

---

## 4. Creating `requirements.txt`

```
flask>=3.0
pytest>=8.0
pytest-cov>=5.0
```

Generate with:

```bash
pip freeze > requirements.txt
```

---

## 5. Final directory structure

```
url_shortener/
├── app.py               # Main application
├── test_shortener.py    # Tests
├── requirements.txt     # Dependencies
├── shortener.db         # SQLite database (auto-created)
└── templates/
    ├── index.html
    └── stats.html
```

---

## Running everything

```bash
# Install
pip install -r requirements.txt

# Run
python3 app.py

# Test
pytest test_shortener.py -v

# Test with coverage
pytest --cov=app test_shortener.py -v
```

---

## Summary

| Aspect | What you added |
|--------|----------------|
| Tests | Flask test client, fixtures, assertions for all routes |
| Error handling | URL validation, 404 handler, try/except in routes |
| Logging | `INFO` for normal ops, `WARNING` for issues, `DEBUG` for details |
| Requirements | `requirements.txt` for one-command setup |
| Production mindset | Code that's testable, observable, and robust |
