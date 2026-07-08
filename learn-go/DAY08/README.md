# DAY08: Interfaces — "A USB Port"

---

## The anecdote: "An interface is a USB port"

A USB port doesn't care what's plugged into it. A keyboard, a mouse, a phone charger — they all work. Why? Because they all implement the USB **interface**.

The interface says: "If you have these 4 pins and communicate at this voltage, you're USB-compatible."

Go interfaces work the same way:

```go
type USB interface {
    Connect() error
    Transfer(data []byte) (int, error)
    Disconnect() error
}
```

Anything that has `Connect()`, `Transfer()`, and `Disconnect()` methods **is a USB**, automatically. No "implements" keyword. No declaration. If it fits the shape, it works.

---

## The syntax

```go
// Define an interface
type Storage interface {
    Save(url URL) error
    FindByCode(code string) (URL, error)
}

// Implement it — just write the methods on any type
type memoryStore struct {
    data map[string]URL
}

func (m *memoryStore) Save(url URL) error {
    m.data[url.Code] = url
    return nil
}

func (m *memoryStore) FindByCode(code string) (URL, error) {
    url, ok := m.data[code]
    if !ok {
        return URL{}, errors.New("not found")
    }
    return url, nil
}
```

**Notice:** nowhere does `memoryStore` say `implements Storage`. Go figures it out by checking: does `*memoryStore` have `Save(URL) error` and `FindByCode(string) (URL, error)`? Yes? Then it satisfies `Storage`. Done.

---

## Why this is powerful

You can swap implementations without changing the code that uses it:

```go
// This function works with ANY Storage
func handleRequest(s Storage, code string) {
    url, err := s.FindByCode(code)
    // ...
}

// Use the in-memory version
mem := &memoryStore{data: make(map[string]URL)}
handleRequest(mem, "abc123")

// Later, swap to a database version — handleRequest unchanged!
db := &sqlStore{db: connectToDB()}
handleRequest(db, "abc123")
```

**This is the L in SOLID** — Liskov Substitution Principle. You can replace a type with any other type that satisfies the same interface, and everything still works.

---

## The standard library is full of interfaces

```
fmt.Stringer    →  String() string
error           →  Error() string  
io.Reader       →  Read(p []byte) (n int, err error)
io.Writer       →  Write(p []byte) (n int, err error)
http.Handler    →  ServeHTTP(w http.ResponseWriter, r *http.Request)
```

You've already been using interfaces without knowing it. `fmt.Println` accepts anything that implements `Stringer`. `http.Handler` is an interface with one method.

---

## Senior mindset: "Accept interfaces, return structs"

This is the #1 Go idiom:

```go
// Constructor returns a struct (concrete type)
func NewMemoryStore() *memoryStore {
    return &memoryStore{data: make(map[string]URL)}
}

// Functions accept interfaces (abstract type)
func ProcessRequests(s Storage) {
    // s can be ANY Storage implementation
}
```

**Why?** Returning a struct lets the caller use all the type's methods, not just the interface's. Accepting an interface lets the caller pass in any implementation. Flexibility in, concrete out.

---

## Summary

| Concept | Code |
|---------|------|
| Define an interface | `type X interface { Method() error }` |
| Implement implicitly | Just write the methods — no declaration needed |
| Accept interfaces | `func Do(s Storage)` |
| Return structs | `func New() *memoryStore` |

Open DAY08/DRILL.md.
