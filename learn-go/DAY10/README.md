# DAY10: Service Layer — "The Manager"

---

## The anecdote: "A service is a manager"

The repository is the filing clerk — it stores and retrieves data.
The service is the **manager** — it makes decisions, applies rules, and coordinates.

```
HTTP Request → [ Handler ] → [ Service ] → [ Repository ]
                                  ↑
                         Business logic lives here
                         "Generate short code"
                         "Validate URL format"
                         "Prevent duplicate codes"
```

The handler doesn't know how codes are generated. The repository doesn't know what a valid URL looks like. The service sits in the middle and **orchestrates**.

---

## Today's goal: service with business logic

```go
type Service struct {
    repo Repository
}

func NewService(repo Repository) *Service {
    return &Service{repo: repo}
}

func (s *Service) Shorten(originalURL string) (storage.URL, error) {
    // 1. Validate the URL
    // 2. Generate a short code
    // 3. Save to repository
    // 4. Return the URL
}
```

**Notice:** The service takes a `Repository` interface, not a `MemoryRepository`. That means it works with ANY implementation — memory, SQLite, PostgreSQL, whatever.

---

## Dependency injection

The service doesn't create its own repository. It receives it:

```go
// In main.go — wiring everything together
repo := storage.NewMemoryRepository()
svc := service.NewService(repo)  // "inject" the dependency
```

This is called **dependency injection**. It's the #1 pattern for building testable code. You can inject a mock repository in tests and a real one in production.

```
Production:  repo → sqlRepo    → service → handler
Tests:       repo → mockRepo   → service → handler
```

Same service. Zero code changes. Different behaviour.

---

## Senior mindset: "Thin handlers, fat services"

Handlers should be 3-5 lines:

1. Parse the request
2. Call the service
3. Write the response

The **service** has all the logic. This means:

- You can test business logic without HTTP
- You can reuse the same service with different transports (HTTP, CLI, gRPC)
- If the API changes, you change the handler — the service stays untouched

---

## Summary

| Layer      | Job                           | Tests                     |
| ---------- | ----------------------------- | ------------------------- |
| Repository | Data access                   | Test with real or mock DB |
| Service    | Business logic                | Test without HTTP         |
| Handler    | Parse request, write response | Test with `httptest`      |

Open DAY10/DRILL.md.
