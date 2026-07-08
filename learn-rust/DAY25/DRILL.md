# DAY25: Drill — "Books REST API"

Continue from DAY24's project, or create a new one:

```bash
cargo new --bin books_api
cd books_api
```

Add to `Cargo.toml`:

```toml
[dependencies]
axum = "0.8"
tokio = { version = "1", features = ["full"] }
serde = { version = "1", features = ["derive"] }
```

## Requirements

Build a complete CRUD REST API for managing books.

```rust
use axum::{
    Router,
    routing::{get, post, put, delete},
    extract::{State, Path},
    response::Json,
    http::StatusCode,
};
use serde::{Serialize, Deserialize};
use std::sync::{Arc, Mutex};

#[derive(Serialize, Deserialize, Clone, Debug)]
struct Book {
    id: u64,
    title: String,
    author: String,
    year: i32,
}

#[derive(Deserialize)]
struct CreateBookRequest {
    title: String,
    author: String,
    year: i32,
}

#[derive(Deserialize)]
struct UpdateBookRequest {
    title: Option<String>,
    author: Option<String>,
    year: Option<i32>,
}

type Db = Arc<Mutex<Vec<Book>>>;

#[derive(Clone)]
struct AppState {
    db: Db,
}

// TODO: GET /books — list all books
async fn list_books(State(state): State<AppState>) -> Json<Vec<Book>> {
    // TODO
}

// TODO: GET /books/:id — get one book
async fn get_book(
    State(state): State<AppState>,
    Path(id): Path<u64>,
) -> Result<Json<Book>, StatusCode> {
    // TODO
}

// TODO: POST /books — create a book
async fn create_book(
    State(state): State<AppState>,
    Json(req): Json<CreateBookRequest>,
) -> (StatusCode, Json<Book>) {
    // TODO
}

// TODO: PUT /books/:id — update a book
async fn update_book(
    State(state): State<AppState>,
    Path(id): Path<u64>,
    Json(req): Json<UpdateBookRequest>,
) -> Result<Json<Book>, StatusCode> {
    // TODO
}

// TODO: DELETE /books/:id — delete a book
async fn delete_book(
    State(state): State<AppState>,
    Path(id): Path<u64>,
) -> Result<StatusCode, StatusCode> {
    // TODO
}

#[tokio::main]
async fn main() {
    let state = AppState {
        db: Arc::new(Mutex::new(vec![])),
    };

    let app = Router::new()
        .route("/books", get(list_books).post(create_book))
        .route("/books/:id", get(get_book).put(update_book).delete(delete_book))
        .with_state(state);

    let listener = tokio::net::TcpListener::bind("127.0.0.1:3000").await.unwrap();
    println!("Books API running on http://127.0.0.1:3000");
    axum::serve(listener, app).await.unwrap();
}
```

## Testing with curl

```bash
# Create a book
curl -X POST http://127.0.0.1:3000/books \
  -H "Content-Type: application/json" \
  -d '{"title":"The Rust Book","author":"Steve Klabnik","year":2019}'

# List all books
curl http://127.0.0.1:3000/books

# Get book by ID
curl http://127.0.0.1:3000/books/1

# Update a book
curl -X PUT http://127.0.0.1:3000/books/1 \
  -H "Content-Type: application/json" \
  -d '{"year":2025}'

# Delete a book
curl -X DELETE http://127.0.0.1:3000/books/1
```

## Hints

<details>
<summary>Click for hints</summary>

- `list_books`: lock the mutex, clone the vec, return `Json(books)`
- `get_book`: lock, find by id, `ok_or(StatusCode::NOT_FOUND)`, map to Json
- `create_book`: lock, create Book with id = len + 1, push, return `(StatusCode::CREATED, Json(book))`
- `update_book`: lock, find_mut by id, update Some fields only, return Ok or Err(NotFound)
- `delete_book`: lock, find position, remove, return Ok(NoContent) or Err(NotFound)
</details>

## How to run

```bash
cd ~/Dev/learn-rust/DAY25/books_api
cargo run
```

Then test with curl in another terminal.

## Before you ask for review

- Does each CRUD operation work?
- What happens when you GET a non-existent book? (Should return 404)
- What happens when you DELETE a non-existent book? (Should return 404)
- Can you create two books and list them both?

## When you're done

Show me your `src/main.rs` and curl output.
