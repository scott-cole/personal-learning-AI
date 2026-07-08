# DAY04: Slices — "A magic expanding backpack"

---

## The anecdote: "A magic expanding backpack"

An array is a fixed-size backpack. If it has 3 pockets, you can only store 3 items.

A **slice** is a magic expanding backpack. Start with 0 items. Add more. It grows.

```go
// Array: fixed size
var scores [3]int  // exactly 3 slots, forever

// Slice: dynamic size
var scores []int   // 0 slots, can grow
```

**In practice, you almost never use arrays in Go. You use slices.**

---

## Creating slices

```go
// Empty slice
var items []string

// Slice with initial values
colors := []string{"red", "green", "blue"}

// Using make (when you know the size but want 0 items)
items := make([]string, 0)  // length 0, capacity 0
items := make([]string, 0, 10)  // length 0, capacity 10 (pre-allocate)
```

**`make` vs `var`:** `var items []string` creates a nil slice. `make` creates an empty slice. Both work with `append`.

---

## Append: the magic

```go
var users []string
users = append(users, "Alice")
users = append(users, "Bob")
users = append(users, "Charlie")

fmt.Println(users) // [Alice Bob Charlie]
```

`append` returns a **new** slice. You must assign it back:

```go
users = append(users, "Dave")  // must reassign!
```

---

## Common operations

```go
fruits := []string{"apple", "banana", "cherry"}

len(fruits)    // 3 — how many items
fruits[0]      // "apple" — first item
fruits[1]      // "banana" — second item
fruits[2]      // "cherry" — last item

// Slice (create a sub-slice)
fruits[1:3]    // ["banana", "cherry"]  — from index 1 to (but not including) 3
fruits[:2]     // ["apple", "banana"]   — first 2 items
fruits[1:]     // ["banana", "cherry"]  — from index 1 to end

// Loop over a slice
for i, fruit := range fruits {
    fmt.Println(i, fruit)
}
// 0 apple
// 1 banana
// 2 cherry

// If you only need the value, not the index:
for _, fruit := range fruits {
    fmt.Println(fruit)
}
```

---

## Nil vs empty slice

```go
var s []string       // nil slice
s == nil             // true
len(s)               // 0

t := []string{}      // empty slice (not nil)
t == nil             // false
len(t)               // 0
```

Both work with `append`. Both have `len` 0. The difference matters when you send data over JSON (nil becomes `null`, empty becomes `[]`).

---

## Senior mindset: "Pre-allocate when you can"

```go
// Slow: append grows one by one
var items []string
for i := 0; i < 1000; i++ {
    items = append(items, "item")
}

// Fast: pre-allocate capacity
items := make([]string, 0, 1000)
for i := 0; i < 1000; i++ {
    items = append(items, "item")
}
```

The second version does **one** memory allocation instead of potentially **many**. In production, this matters at scale.

---

## Summary

| Operation | Code |
|-----------|------|
| Create empty slice | `var items []string` |
| Create with values | `items := []string{"a", "b"}` |
| Add item | `items = append(items, "c")` |
| Length | `len(items)` |
| Loop | `for i, v := range items` |
| Sub-slice | `items[1:3]` |

Open DAY04/DRILL.md.
