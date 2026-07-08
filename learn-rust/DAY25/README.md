# DAY25: REST API with JSON Responses

---

## The anecdote: "REST is a menu with standard sections"

A restaurant menu has standard sections: Appetizers, Entrees, Desserts. Everyone knows what to expect in each section.

REST APIs have standard operations: GET (read), POST (create), PUT (update), DELETE (remove). Everyone knows what to expect from each operation.

Today you'll build a REST API for managing books — a full CRUD interface.

---

## CRUD Operations

| Operation | HTTP Method | Route | What it does |
|-----------|-------------|-------|-------------|
| Create | POST | `/books` | Add a new book |
| Read (all) | GET | `/books` | List all books |
| Read (one) | GET | `/books/:id` | Get one book |
| Update | PUT | `/books/:id` | Update a book |
| Delete | DELETE | `/books/:id` | Remove a book |

---

## Using shared state in axum

```rust
use std::sync::{Arc, Mutex};
use axum::{Router, routing::get, extract::State};

type Db = Arc<Mutex<Vec<Book>>>;

#[derive(Clone)]
struct AppState {
    db: Db,
}

async fn list_books(State(state): State<AppState>) -> Json<Vec<Book>> {
    let books = state.db.lock().unwrap();
    Json(books.clone())
}

// In main:
let state = AppState {
    db: Arc::new(Mutex::new(vec![])),
};

let app = Router::new()
    .route("/books", get(list_books))
    .with_state(state);
```

---

## Request body with POST

```rust
use axum::extract::State;

async fn create_book(
    State(state): State<AppState>,
    Json(book): Json<CreateBookRequest>,
) -> Json<Book> {
    let mut books = state.db.lock().unwrap();
    let new_book = Book {
        id: books.len() as u64 + 1,
        title: book.title,
        author: book.author,
    };
    books.push(new_book.clone());
    Json(new_book)
}
```

---

## Handling errors

```rust
use axum::http::StatusCode;

async fn get_book(
    State(state): State<AppState>,
    Path(id): Path<u64>,
) -> Result<Json<Book>, StatusCode> {
    let books = state.db.lock().unwrap();
    books.iter()
        .find(|b| b.id == id)
        .cloned()
        .map(Json)
        .ok_or(StatusCode::NOT_FOUND)
}
```

---

## Summary

| Concept | Code |
|---------|------|
| Shared state | `State(state): State<AppState>` |
| Request body | `Json(body): Json<T>` |
| Status codes | `Result<Json<T>, StatusCode>` |
| 404 Not Found | `StatusCode::NOT_FOUND` |
| 201 Created | `(StatusCode::CREATED, Json(data))` |
| Router with state | `.with_state(state)` |

Your turn. Open DAY25/DRILL.md.
