# DAY14: Project — "Full URL Shortener v1"

Open DAY14/README.md for the full spec.

Build a URL shortener with:
- Repository with `IncrementAccess` and `List`
- Service with `Stats` method
- JSON handlers for all endpoints
- Logging and request ID middleware
- Click tracking via `AccessCount`
- `GET /{code}/stats` endpoint

## Steps

1. Copy DAY13 structure
2. Update `storage/memory.go` with `AccessCount` field, `IncrementAccess`, `List`
3. Update `service/service.go` with `Stats` method
4. Update `main.go` with `GET /{code}/stats` handler
5. Run and test with curl

## Run it

```bash
cd ~/Dev/learn-go/DAY14
go run main.go
```
