# DAY30: Drill — "Ship the URL shortener"

Your final project: build the complete URL shortener server.

## Part 1 — Create the project structure

Create these files in DAY30:

```
DAY30/
├── main.go          # Entry point + handlers
├── store.go         # In-memory storage
├── rate_limiter.go  # Rate limiter (from DAY22)
├── server.go        # Graceful shutdown setup
└── helpers.go       # JSON helpers + middleware
```

## Part 2 — Implement each file

### store.go
- URL struct with ID, ShortCode, LongURL, Clicks, CreatedAt
- Store struct with Create, GetByCode, GetAll, Delete, IncrementClicks
- generateShortCode() — random 7-char alphanumeric
- generateID() — "url_" + hex

### helpers.go
- `writeJSON(w, status, data)` — marshal JSON and write
- `writeError(w, status, code, message)` — consistent error format
- `corsMiddleware(next)` — add CORS headers
- `securityHeaders(next)` — add security headers

### rate_limiter.go
- Copy from DAY22 (or improve it)
- Configurable rate and window

### server.go
- HTTP server with graceful shutdown
- Handle SIGINT and SIGTERM
- Print "URL shortener on :8080"

### main.go
- Create store, rate limiter, handlers
- Wire up middleware
- Start server

## Part 3 — Test each endpoint

```bash
# Start server
go run . &

# Create a short URL
RESPONSE=$(curl -s -X POST http://localhost:8080/api/shorten \
  -H "Content-Type: application/json" \
  -d '{"url":"https://example.com/very/long/article?page=2"}')
echo "$RESPONSE"
# Extract the short code
CODE=$(echo "$RESPONSE" | python3 -c "import sys,json; print(json.load(sys.stdin)['short_code'])")
echo "Short code: $CODE"

# Redirect
curl -v http://localhost:8080/api/$CODE 2>&1 | grep -E "Location:|HTTP/"

# List URLs
curl http://localhost:8080/api/urls | python3 -m json.tool

# Get stats
# (you need to parse the URL ID from the list response)

# Delete
# (use the ID from the list response)

# Rate limiting
for i in {1..105}; do
  STATUS=$(curl -s -o /dev/null -w "%{http_code}" \
    -X POST http://localhost:8080/api/shorten \
    -H "Content-Type: application/json" \
    -d '{"url":"https://example.com/rate-test"}')
  if [ "$STATUS" = "429" ]; then
    echo "Rate limited after $i requests"
    break
  fi
done
```

## Part 4 — Graceful shutdown

```bash
go run . &
PID=$!
sleep 1

# Send a long-running request
time curl -s http://localhost:8080/api/shorten \
  -H "Content-Type: application/json" \
  -d '{"url":"https://example.com/slow"}' &

# Shut down gracefully
kill -TERM $PID
echo "Server stopped"
wait $PID
```

## Part 5 — Polish

Add these improvements:

1. **CORS** — allow all origins (for frontend development)
2. **Validation** — reject URLs longer than 2048 characters
3. **Duplicate detection** — if the same URL is shortened twice, return the existing short code
4. **Stats endpoint** — GET /api/urls/{id} returns click count + timestamps

## Deliverables

1. All 5 source files (main.go, store.go, rate_limiter.go, server.go, helpers.go)
2. Successful creation + redirect + listing + deletion
3. Rate limiting working (100 requests then 429)
4. Graceful shutdown working
5. At least 2 improvements from Part 5

## Hints

<details>
<summary>Click for hints</summary>

- Use `curl -v` to see the redirect headers (Location, status 302)
- For the stats endpoint, pass the URL ID (not short code) — e.g., `/api/urls/url_abc123`
- Rate limit by default: 100 requests per hour per IP
- Graceful shutdown: `signal.Notify` + `server.Shutdown`
- Test the redirect by visiting `http://localhost:8080/api/{code}` in your browser
- To stop the server: `kill %1` or `fg` then `Ctrl+C`
- The `short_code` field is returned in the create response — use it for the redirect
</details>
