# DAY24 Drill — Build a REST API

## Task

Build a Flask REST API (`app.py`) for managing a collection of movies.

### Data

```python
movies = [
    {"id": 1, "title": "The Matrix", "director": "Lana Wachowski", "year": 1999, "rating": 8.7},
    {"id": 2, "title": "Inception", "director": "Christopher Nolan", "year": 2010, "rating": 8.8},
    {"id": 3, "title": "Parasite", "director": "Bong Joon-ho", "year": 2019, "rating": 8.5},
]
```

### Endpoints

| Method | Route | Description |
|--------|-------|-------------|
| `GET` | `/api/movies` | List all movies |
| `GET` | `/api/movies/<id>` | Get a single movie |
| `POST` | `/api/movies` | Create a new movie |
| `PUT` | `/api/movies/<id>` | Update a movie (replace all fields) |
| `DELETE` | `/api/movies/<id>` | Delete a movie |
| `GET` | `/api/movies/search?q=matrix` | Search movies by title (case-insensitive) |

### Expected behaviour

**GET /api/movies**
```json
[
    {"id": 1, "title": "The Matrix", "director": "Lana Wachowski", "year": 1999, "rating": 8.7},
    ...
]
```

**GET /api/movies/2**
```json
{"id": 2, "title": "Inception", "director": "Christopher Nolan", "year": 2010, "rating": 8.8}
```

**GET /api/movies/999**
```json
{"error": "Movie not found"}
```

**POST /api/movies** with `{"title": "Arrival", "director": "Denis Villeneuve", "year": 2016, "rating": 7.9}`
```json
{"id": 4, ...}  (status 201)
```

**PUT /api/movies/1** with `{"title": "The Matrix Reloaded", "director": "Lana Wachowski", "year": 2003, "rating": 7.2}`
→ Updated movie (status 200)

**DELETE /api/movies/3** → `{"message": "Movie deleted"}` (status 200)

**GET /api/movies/search?q=matrix**
```json
[{"id": 1, "title": "The Matrix", ...}]
```

### Hints

- `request.get_json()` for POST/PUT body
- Search is case-insensitive: `query.lower() in movie["title"].lower()`
- Use `global next_id` for auto-incrementing
- Validate required fields in POST and return `400` if missing

### How to run

```bash
python3 app.py
# Test with curl or httpie
```

## TODOs

- [ ] Implement `GET /api/movies`
- [ ] Implement `GET /api/movies/<id>` with 404 handling
- [ ] Implement `POST /api/movies` with validation
- [ ] Implement `PUT /api/movies/<id>` with full update
- [ ] Implement `DELETE /api/movies/<id>`
- [ ] Implement search endpoint
- [ ] Test all endpoints with curl
