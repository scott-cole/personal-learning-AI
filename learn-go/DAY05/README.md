# DAY05: Maps — "A Coat Check Ticket"

---

## The anecdote: "A map is a coat check"

You walk into a club. You hand your coat to the attendant. They give you a ticket with a number. Later, you hand back the ticket, and they give you your coat.

```
Ticket #42 → [Shelf] → Your coat
    key          map       value
```

A map is exactly this:

```go
var coatCheck map[int]string  // map[ticket_number]coat_label
```

But unlike a coat check, you can use any type for the key:

```go
phoneBook := map[string]string{
    "Alice": "555-0100",
    "Bob":   "555-0101",
}
```

---

## Creating maps

```go
// Method 1: literal
ages := map[string]int{
    "Alice": 30,
    "Bob":   25,
}

// Method 2: make
scores := make(map[string]int)
scores["Alice"] = 95
scores["Bob"] = 87

// Method 3: var (nil map — DANGER!)
var looks map[string]int
looks["Alice"] = 10  // PANIC! nil map can't store values
```

**Always use `make` or a literal. Never use `var` if you plan to add items.**

---

## CRUD operations

```go
users := make(map[string]int)

// CREATE / UPDATE
users["Alice"] = 30
users["Bob"] = 25

// READ
aliceAge := users["Alice"]  // 30

// DELETE
delete(users, "Bob")

// CHECK IF EXISTS
age, ok := users["Bob"]
if !ok {
    fmt.Println("Bob not found")
}
```

**The comma-ok idiom** is the most important pattern with maps:

```go
value, ok := myMap[key]
if !ok {
    // key doesn't exist
}
```

If you just read `myMap[key]` and the key doesn't exist, you get the **zero value** (0 for int, "" for string). You can't tell if the value is 0 because it doesn't exist or because it's actually 0.

---

## Maps are references

When you assign a map to another variable, they share the same data:

```go
original := map[string]int{"count": 1}
copy := original
copy["count"] = 99
fmt.Println(original["count"]) // 99 — both changed!
```

This is different from slices and structs. Be careful.

---

## Looping over maps

```go
ages := map[string]int{"Alice": 30, "Bob": 25}

for name, age := range ages {
    fmt.Printf("%s is %d years old\n", name, age)
}
```

**Warning:** Map iteration order is **random** in Go. Don't rely on it.

---

## Senior mindset: "Use maps for lookups"

A slice asks "what's at position X?" A map asks "what's at key X?"

- **Slice** = ordered, indexed by position
- **Map** = unordered, indexed by key

In production, maps are everywhere:
- Look up a user by ID
- Cache API responses by URL
- Count occurrences of things
- Store configuration by name

---

## Summary

| Operation | Code |
|-----------|------|
| Create | `m := make(map[string]int)` |
| Set | `m["key"] = 42` |
| Get | `val := m["key"]` |
| Safe get | `val, ok := m["key"]` |
| Delete | `delete(m, "key")` |
| Length | `len(m)` |
| Loop | `for k, v := range m` |

Open DAY05/DRILL.md.
