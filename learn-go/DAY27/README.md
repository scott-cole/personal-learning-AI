# DAY27: Project Structure — "The Standard Layout"

---

## The anecdote: "A well-organised toolbox"

A plumber doesn't throw all tools in a bucket. Each tool has a place: wrenches in one drawer, screwdrivers in another, pipe fittings in a box.

A well-organised Go project is the same. Anyone (including you in 6 months) can open it and know where to find things.

---

## The standard Go project layout

```
url-shortener/
├── cmd/
│   └── server/
│       └── main.go          → entry point (thin)
├── internal/
│   ├── handler/
│   │   └── handler.go       → HTTP handlers
│   ├── service/
│   │   └── service.go       → business logic
│   ├── storage/
│   │   ├── memory.go        → in-memory repository
│   │   └── repository.go    → Repository interface + URL model
│   └── middleware/
│       └── middleware.go    → logging, rate limit, etc.
├── go.mod
└── go.sum
```

---

## Why `cmd/` and `internal/`?

### `cmd/` — entry points

```
cmd/
├── server/      → main.go (HTTP server)
└── migrate/     → main.go (run migrations from CLI)
```

Each subdirectory is a separate `main` package that builds to a binary.

```bash
go build ./cmd/server    → produces "server" binary
go build ./cmd/migrate   → produces "migrate" binary
```

### `internal/` — not importable

The `internal` package is a Go-enforced boundary:

```
url-shortener/internal/handler → CAN be imported by url-shortener/cmd/server
url-shortener/internal/handler → CANNOT be imported by github.com/anyone/else
```

Go prevents any package outside `url-shortener` from importing `internal/...`. This enforces encapsulation — your internal packages are **private**.

---

## Package responsibilities

| Package | Responsibility | Imports |
|---------|---------------|---------|
| `cmd/server/main.go` | Wire everything together, start server | All internal packages |
| `internal/handler` | Parse request, call service, write response | `service` |
| `internal/service` | Business logic, validation | `storage` |
| `internal/storage` | Data access (memory or SQL) | Nothing from the project |
| `internal/middleware` | Cross-cutting concerns | `net/http` |

---

## Dependency direction

```
cmd/server
    ↓
internal/handler → internal/service → internal/storage (interface)
                        ↓
              internal/storage (SQLRepository implementation)
```

Dependencies flow **down**. Never up. Never sideways.

---

## Senior mindset: "Start simple, extract later"

You don't need this structure on DAY01. Start with a single `main.go`. When it grows past 500 lines, start splitting:

1. Extract storage → `storage/`
2. Extract service → `service/`
3. Extract handlers → `handler/`
4. Add `cmd/` when you have multiple binaries
5. Add `internal/` when you want to enforce boundaries

---

## Summary

| Directory | Purpose |
|-----------|---------|
| `cmd/` | Entry points (one subdir per binary) |
| `internal/` | Private packages (can't be imported externally) |
| Each package | Single responsibility |

Open DAY27/DRILL.md.
