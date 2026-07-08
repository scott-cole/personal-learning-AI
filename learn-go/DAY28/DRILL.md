# DAY28: Drill — "Build the Final Project"

Build the URL shortener with analytics. No step-by-step drill today — you know enough to build it yourself.

## Recommended order

1. **Set up the project structure**
   - `go mod init learn-go/DAY28`
   - Create `cmd/server/main.go`, `internal/*` packages

2. **Config**
   - `internal/config/config.go` — port, db path, base URL

3. **Storage**
   - `internal/storage/repository.go` — URL, Visit structs + interfaces
   - `internal/storage/sqlite.go` — SQL implementation with migrations

4. **Service**
   - `internal/service/service.go` — Shorten, Resolve, Stats, ListAll

5. **Handlers**
   - `internal/handler/handler.go` — POST /shorten, GET /{code}, GET /{code}/stats, GET /admin/urls

6. **Middleware**
   - `internal/middleware/logging.go`
   - `internal/middleware/requestid.go`
   - `internal/middleware/ratelimit.go`

7. **Main**
   - `cmd/server/main.go` — wire everything, graceful shutdown

8. **Migrations**
   - `migrations/001_create_urls.sql`
   - `migrations/002_create_visits.sql`

## Run it

```bash
cd ~/Dev/learn-go/DAY28
go run ./cmd/server/
```

## Test it

```bash
# Shorten
curl -s -X POST http://localhost:8080/shorten \
  -H "Content-Type: application/json" \
  -d '{"url":"https://example.com"}' | jq .

# Resolve (replace CODE)
curl -v http://localhost:8080/CODE

# Stats (replace CODE)
curl -s http://localhost:8080/CODE/stats | jq .

# Admin list
curl -s http://localhost:8080/admin/urls | jq .
```

## When you're done

You've built a production-ready URL shortener. You know:

- Go fundamentals (types, structs, interfaces, pointers)
- Backend architecture (repository, service, handler, middleware)
- SQL and database/sql
- Testing with table-driven tests and mocks
- Concurrency with mutexes
- Production patterns (config, graceful shutdown, rate limiting)

You're ready to build real backends.
