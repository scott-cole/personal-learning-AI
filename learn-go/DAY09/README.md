# DAY09: Repository Pattern — "The Filing Cabinet"

---

## The anecdote: "A repository is a filing cabinet"

A filing cabinet has drawers. Each drawer has folders. Each folder has documents.

You don't care **how** the cabinet organises things internally — you just say "give me the document with ID 42" and it appears. The cabinet could be a wooden box, a metal drawer, or a cloud server. The interface is the same: "store this, find that."

```
Request → [ Service ] → [ Repository ] → [ Database ]
                               ↑
                      You don't care if it's
                      in-memory, SQLite, or Postgres
```

---

## What is the repository pattern?

A **repository** wraps data access behind an interface. The rest of your code talks to the interface, never directly to the database.

```
type Repository interface {
    Save(url URL) error
    FindByCode(code string) (URL, error)
    Delete(code string) error
    List() ([]URL, error)
}
```

**The service layer** (tomorrow) calls `repo.Save()` and `repo.FindByCode()`. It doesn't know or care if the data is in a map, a file, or PostgreSQL. That's the whole point.

---

## Today's goal: organise what you built yesterday

Yesterday you wrote a `memoryStore` in a single file. Today you'll split it into **proper package structure**:

```
DAY09/
├── main.go           → uses the repository
└── storage/
    └── memory.go     → the MemoryRepository implementation
```

This is how real Go projects are structured:

```
project/
├── cmd/
│   └── server/
│       └── main.go         → entry point
├── internal/
│   └── repository/
│       ├── interface.go     → the Repository interface
│       └── memory.go        → in-memory implementation
│   └── service/
│       └── service.go       → business logic (DAY10)
│   └── handler/
│       └── handler.go       → HTTP handlers (DAY11)
```

---

## Go package naming rules

- A package is a folder
- All files in the same folder share the same `package` name
- Uppercase = exported (public), lowercase = unexported (private)
- Import by module path: `import "learn-go/DAY09/storage"`

---

## Senior mindset: "Program to an interface, not an implementation"

```go
// ❌ Bad: depends on a specific implementation
func process(store *memoryStore) {
    store.Save(url)
}

// ✅ Good: depends on an interface
func process(store Repository) {
    store.Save(url)
}
```

The first version can never use a database. The second version can use anything that satisfies `Repository`. In production, this means you can:
- Start with in-memory for rapid prototyping
- Swap to SQL when you need persistence
- Use a mock repository in tests
- All without changing the code that uses it

---

## Summary

| Concept | Why it matters |
|---------|---------------|
| Repository interface | Defines WHAT data operations exist |
| Memory implementation | Quick prototyping, testing |
| SQL implementation (later) | Real persistence |
| Everything uses the interface | Swap implementations without touching business logic |

Open DAY09/DRILL.md.
