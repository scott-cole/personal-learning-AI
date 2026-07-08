# DAY28: Final Project — "URL Shortener with Analytics"

---

## The brief

Build a production-ready URL shortener with click analytics.

This is your portfolio piece. Everything you've learned across 28 days goes into it.

---

## The spec

### Endpoints

```
POST /shorten     → Create a short URL (201 + JSON)
GET /{code}       → Redirect to original URL (302)
GET /{code}/stats → Get analytics for a code (200 + JSON)
GET /admin/urls   → List all URLs with stats (200 + JSON)
```

### Data model

```sql
CREATE TABLE urls (
    code         TEXT PRIMARY KEY,
    original_url TEXT NOT NULL,
    access_count INTEGER NOT NULL DEFAULT 0,
    created_at   TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE visits (
    id         INTEGER PRIMARY KEY AUTOINCREMENT,
    code       TEXT NOT NULL REFERENCES urls(code),
    ip_address TEXT,
    user_agent TEXT,
    visited_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

### Features

1. **Shorten** — validate URL, generate random code, save to DB
2. **Redirect** — look up code, record visit (IP + user agent), redirect
3. **Stats** — return URL info + total visits + recent visits list
4. **Admin list** — list all URLs sorted by most visited

### Quality checklist

- [ ] Config via env vars (port, db path, base URL)
- [ ] SQLite with migrations
- [ ] Repository interface with SQL implementation
- [ ] Service layer with validation
- [ ] JSON request/response with proper status codes
- [ ] Logging middleware
- [ ] Request ID middleware
- [ ] Rate limiting middleware (10 req/s per IP)
- [ ] Graceful shutdown
- [ ] At least 80% test coverage on service + storage
- [ ] Standard project layout (cmd/, internal/)

---

## Stretch goals

- [ ] Add `DELETE /{code}` endpoint
- [ ] Add `PUT /{code}` to update the original URL
- [ ] Dockerfile
- [ ] Add a simple HTML home page at `GET /`

---

## Senior mindset: "Done is better than perfect"

You won't implement every stretch goal. Prioritise:

1. Working endpoints (shorten + redirect + stats)
2. Data persistence (SQLite)
3. Middleware stack (logging + rate limit)
4. Graceful shutdown
5. Tests

If you have time, polish the rest.

---

## Structure

```
DAY28/
├── go.mod
├── cmd/
│   └── server/
│       └── main.go
├── internal/
│   ├── config/
│   │   └── config.go
│   ├── handler/
│   │   └── handler.go
│   ├── service/
│   │   └── service.go
│   ├── storage/
│   │   ├── repository.go
│   │   └── sqlite.go
│   └── middleware/
│       ├── logging.go
│       ├── ratelimit.go
│       └── requestid.go
└── migrations/
    ├── 001_create_urls.sql
    └── 002_create_visits.sql
```

---

## Day 28 is your capstone

This is the project you show in interviews. Take your time. Make it clean.

Open DAY28/DRILL.md.
