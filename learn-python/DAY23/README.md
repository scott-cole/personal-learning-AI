# DAY23 — Flask Basics

## Anecdote: Flask is a food truck kitchen

A restaurant kitchen is huge — walk-in fridges, multiple stoves, a pastry station, a deep fryer, a grill, and a team of chefs. That's Django.

A food truck kitchen is the opposite — a single burner, a small counter, a few ingredients. You can fit it in a van and start serving customers in an afternoon. That's Flask.

Flask is a **micro-framework**. It gives you just enough to serve web pages: routing, templating, and request handling. Everything else (database, forms, authentication) you add as needed. It's perfect for small applications, APIs, and prototypes.

---

## Installing Flask

```bash
pip install flask
```

---

## Your first Flask app

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello, World!"

@app.route("/about")
def about():
    return "<h1>About Page</h1><p>This is a Flask app.</p>"

if __name__ == "__main__":
    app.run(debug=True)
```

```bash
python3 app.py
# * Running on http://127.0.0.1:5000
```

Visit `http://127.0.0.1:5000/` in your browser.

---

## Routes with variables

```python
@app.route("/user/<name>")
def user_profile(name):
    return f"<h1>Welcome, {name}!</h1>"

@app.route("/post/<int:post_id>")
def show_post(post_id):
    return f"<p>Viewing post #{post_id}</p>"

@app.route("/path/<path:subpath>")
def show_subpath(subpath):
    return f"<p>Subpath: {subpath}</p>"
```

| Converter | Description |
|-----------|-------------|
| `string` | Default — any text without slashes |
| `int` | Positive integer |
| `float` | Positive float |
| `path` | Like string but accepts `/` |

---

## Templates with Jinja2

Flask uses Jinja2 as its template engine. Templates go in a `templates/` folder.

```python
from flask import render_template

@app.route("/hello/<name>")
def hello(name):
    return render_template("hello.html", name=name, title="Greeting")
```

**`templates/hello.html`**:

```html
<!DOCTYPE html>
<html>
<head>
    <title>{{ title }}</title>
</head>
<body>
    <h1>Hello, {{ name }}!</h1>
    {% if name %}
        <p>Welcome back, {{ name }}.</p>
    {% else %}
        <p>Welcome, stranger.</p>
    {% endif %}
</body>
</html>
```

### Jinja2 template syntax

| Syntax | Purpose |
|--------|---------|
| `{{ var }}` | Output a variable |
| `{% if %}` / `{% endif %}` | Conditionals |
| `{% for %}` / `{% endfor %}` | Loops |
| `{# comment #}` | Comments (not in output) |
| `{{ \| filter }}` | Filters (e.g., `\| upper`, `\| length`) |

---

## Request and response

```python
from flask import request, jsonify

@app.route("/search")
def search():
    query = request.args.get("q", "")
    page = request.args.get("page", 1, type=int)
    return f"Searching for '{query}' on page {page}"
```

---

## Static files

Place CSS, JS, images in a `static/` folder. Reference them with `url_for`:

```python
from flask import url_for

# In template:
# <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
```

---

## Redirects and error pages

```python
from flask import redirect, abort

@app.route("/old-page")
def old_redirect():
    return redirect("/new-page")

@app.route("/user/<int:uid>")
def user(uid):
    if uid < 1:
        abort(404)
    return f"User {uid}"
```

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| `Flask(__name__)` | Create the app |
| `@app.route()` | Map a URL to a function |
| `render_template()` | Render an HTML file with Jinja2 |
| `request.args` | Access query parameters |
| `redirect()` / `abort()` | Control flow |
| Templates | `{{ }}` for variables, `{% %}` for logic |
| Static folder | CSS, JS, images |
| `debug=True` | Auto-reload on code changes |
