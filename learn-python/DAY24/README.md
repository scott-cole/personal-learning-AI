# DAY24 — Flask REST API

## Anecdote: REST is a menu with sections

Walk into a restaurant. The host hands you a menu divided into sections: Appetizers, Mains, Desserts, Drinks. Each section lists items with names and prices. You order a "Caesar Salad" from Appetizers — the kitchen knows exactly what to do because the menu has a standard structure.

A REST API is that menu. The sections are **endpoints** (`/appetizers`, `/mains`). The items are **resources**. The verbs are **HTTP methods**:

| HTTP Method | Restaurant action | CRUD equivalent |
|-------------|------------------|-----------------|
| `GET` | Read the menu | **Read** |
| `POST` | Place a new order | **Create** |
| `PUT` | Change your order | **Update** (full) |
| `PATCH` | Modify part of your order | **Update** (partial) |
| `DELETE` | Cancel your order | **Delete** |

---

## Returning JSON

Flask makes JSON responses easy:

```python
from flask import Flask, jsonify, request

app = Flask(__name__)

@app.route("/api/greet/<name>")
def greet(name):
    return jsonify({"message": f"Hello, {name}!", "status": "ok"})
```

`jsonify()` converts a Python dict to a JSON response with the correct `Content-Type` header.

---

## A simple REST API — books collection

```python
books = [
    {"id": 1, "title": "The Great Gatsby", "author": "F. Scott Fitzgerald", "year": 1925},
    {"id": 2, "title": "1984", "author": "George Orwell", "year": 1949},
    {"id": 3, "title": "To Kill a Mockingbird", "author": "Harper Lee", "year": 1960},
]
next_id = 4

# GET all books
@app.route("/api/books", methods=["GET"])
def get_books():
    return jsonify(books)

# GET a single book
@app.route("/api/books/<int:book_id>", methods=["GET"])
def get_book(book_id):
    book = next((b for b in books if b["id"] == book_id), None)
    if book is None:
        return jsonify({"error": "Book not found"}), 404
    return jsonify(book)

# POST a new book
@app.route("/api/books", methods=["POST"])
def create_book():
    global next_id
    data = request.get_json()
    if not data or "title" not in data or "author" not in data:
        return jsonify({"error": "Missing required fields"}), 400
    book = {
        "id": next_id,
        "title": data["title"],
        "author": data["author"],
        "year": data.get("year"),
    }
    books.append(book)
    next_id += 1
    return jsonify(book), 201

# DELETE a book
@app.route("/api/books/<int:book_id>", methods=["DELETE"])
def delete_book(book_id):
    global books
    book = next((b for b in books if b["id"] == book_id), None)
    if book is None:
        return jsonify({"error": "Book not found"}), 404
    books = [b for b in books if b["id"] != book_id]
    return jsonify({"message": "Book deleted"}), 200
```

---

## Testing with `curl`

```bash
# GET all
curl http://127.0.0.1:5000/api/books

# GET one
curl http://127.0.0.1:5000/api/books/1

# POST
curl -X POST http://127.0.0.1:5000/api/books \
  -H "Content-Type: application/json" \
  -d '{"title":"Dune","author":"Frank Herbert","year":1965}'

# DELETE
curl -X DELETE http://127.0.0.1:5000/api/books/3
```

---

## HTTP status codes

| Code | Meaning | When to use |
|------|---------|-------------|
| `200` | OK | Successful `GET`, `PUT`, `PATCH`, `DELETE` |
| `201` | Created | Successful `POST` |
| `204` | No Content | Successful delete, no body needed |
| `400` | Bad Request | Invalid input, missing fields |
| `404` | Not Found | Resource doesn't exist |
| `409` | Conflict | Duplicate resource |
| `422` | Unprocessable Entity | Validation error |
| `500` | Internal Server Error | Unexpected server failure |

---

## Request body parsing

```python
# JSON body
data = request.get_json()

# Form data
name = request.form.get("name")

# Query parameters
page = request.args.get("page", 1, type=int)
```

---

## CORS (Cross-Origin Resource Sharing)

For frontend apps to call your API from a different domain:

```bash
pip install flask-cors
```

```python
from flask_cors import CORS

app = Flask(__name__)
CORS(app)  # Allow all origins
```

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| `jsonify()` | Return a JSON response from a dict |
| `request.get_json()` | Parse incoming JSON body |
| HTTP methods | `GET` read, `POST` create, `PUT`/`PATCH` update, `DELETE` delete |
| Status codes | `200`, `201`, `400`, `404`, `500` |
| Route params | `<int:id>` captures URL segments |
| Error responses | Return `(dict, status_code)` tuple |
| `flask-cors` | Enable cross-origin requests |
