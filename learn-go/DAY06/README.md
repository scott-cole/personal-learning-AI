# DAY06: Pointers — "A House Address"

---

## The anecdote: "A pointer is a house address"

You want your friend to visit your house. You have two options:

1. **Give them a copy of your house** (a photo, a 3D model) — expensive, takes time, and they can't actually enter it
2. **Give them your address** — a tiny piece of paper, and they can walk right in

A **value** is option 1 (the whole house). A **pointer** is option 2 (just the address).

```go
// Value — a copy of the whole thing
func birthday(person Person) {
    person.Age++  // modifies the COPY, original unchanged
}

// Pointer — just the address, modify the original
func birthday(person *Person) {
    person.Age++  // modifies the ORIGINAL
}
```

---

## The syntax

```go
// *Type means "pointer to Type"
var p *Person  // p is a pointer to a Person (p holds an address, not a Person)

// &variable means "address of this variable"
scott := Person{Name: "Scott"}
p := &scott     // p now holds the address of scott

// *pointer means "the thing at this address" (dereference)
fmt.Println((*p).Name)  // Scott — go to the address, get the Name

// But Go is smart — you can skip the *:
fmt.Println(p.Name)     // Scott — Go automatically dereferences for field access
```

---

## When to use pointers

| Use a pointer | Don't use a pointer |
|--------------|-------------------|
| You need to MODIFY the original | You only need to READ data |
| The struct is LARGE (saves copying) | The struct is tiny (an int, a bool) |
| You want to signal "this can be nil" | A zero value makes sense |
| You need shared access | Each caller needs independent data |

---

## nil pointer safety

A pointer can be `nil` — it points to nothing:

```go
var p *Person  // nil — no address
fmt.Println(p.Name)  // PANIC! Can't read Name of nothing
```

**Always check for nil:**

```go
if p == nil {
    return errors.New("person is nil")
}
```

---

## The "new" function

`new(T)` creates a zero-valued T and returns a pointer to it:

```go
p := new(Person)
// same as:
p := &Person{}
```

`new` is rarely used in practice. `&Type{}` is more common and readable.

---

## Senior mindset: "Understand when you're copying"

```go
func processUsers(users []User) {
    // Slices are passed by reference (well, a slice header is copied,
    // but the underlying array is shared)
}

func processUser(user User) {
    // Structs are COPIED entirely. If User is 100 fields, that's expensive.
}

func processUser(user *User) {
    // Only the address (8 bytes) is copied, regardless of User size.
}
```

In production, if a struct has many fields or is passed through many layers, use a pointer to avoid copying gigabytes of data unnecessarily.

---

## Summary

| Concept | Code |
|---------|------|
| Pointer type | `var p *Person` |
| Get address | `p := &person` |
| Dereference | `(*p).Name` |
| Auto-dereference | `p.Name` |
| Nil check | `if p == nil { ... }` |
| Create pointer | `p := &Person{Name: "Scott"}` |

Open DAY06/DRILL.md.
