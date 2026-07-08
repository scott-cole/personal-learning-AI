# DAY03: Structs and Methods

---

## The anecdote: "A struct is a paper form"

Imagine you're at the DMV. You need to fill out a form:

```
NAME: _____________
AGE:  _____________
ADDRESS: __________
```

That form is a **struct** — a way to group related fields together.

In Go:

```go
type Person struct {
    Name    string
    Age     int
    Address string
}
```

Now you can create a Person:

```go
p := Person{
    Name:    "Scott",
    Age:     30,
    Address: "123 Main St",
}

fmt.Println(p.Name)   // Scott
fmt.Println(p.Age)    // 30
```

**Why does this matter?** Real applications are built of structs. A `User`, an `Order`, a `Transaction`, a `Payment` — all structs.

From the Monzo exercise you've seen:

```go
type Account struct {
    ID       string
    Balance  int64
    Currency Currency
}
```

---

## Methods: "a function with a context"

A method is a function that belongs to a struct.

```go
// This is a function
func greet(name string) string { ... }

// This is a METHOD on Person
func (p Person) greet() string {
    return "Hello, I'm " + p.Name
}
```

The `(p Person)` part is the **receiver**. It says "this function works with a Person".

```go
p := Person{Name: "Scott"}
message := p.greet()   // "Hello, I'm Scott"
```

**Compare with JavaScript:**

```javascript
// JS: the method uses `this`
class Person {
    greet() { return "Hello, I'm " + this.name }
}

// Go: the receiver IS the `this`
func (p Person) greet() string {
    return "Hello, I'm " + p.Name
}
```

---

## Value receiver vs Pointer receiver

```go
// Value receiver — works on a COPY
func (p Person) getName() string {
    return p.Name  // safe, not modifying anything
}

// Pointer receiver — works on the ORIGINAL
func (p *Person) setAge(age int) {
    p.Age = age  // modifies the original
}
```

**When to use which:**

| Use pointer receiver `*` | Use value receiver |
|--------------------------|-------------------|
| You need to modify the struct | You're only reading |
| The struct is large (saves copying) | The struct is small |
| Consistency (all methods use pointer) | — |

---

## Senior mindset: "Zero values are your friend"

In Go, every variable starts at zero. You don't need a constructor:

```go
var p Person
fmt.Println(p.Name)    // "" (empty string)
fmt.Println(p.Age)     // 0
fmt.Println(p.Address) // "" (empty string)
```

This means **no null checks** for basic fields. Compare with Java where every object field could be null.

```
Go:     var p Person   → p.Name is "" 
Java:   Person p       → p.Name causes NullPointerException
```

You'll learn to love this.

---

## Summary

| Concept | Code |
|---------|------|
| Define a struct | `type Person struct { Name string }` |
| Create an instance | `p := Person{Name: "Scott"}` |
| Access a field | `p.Name` |
| Method (value receiver) | `func (p Person) greet() string` |
| Method (pointer receiver) | `func (p *Person) setAge(a int)` |

Open DAY03/DRILL.md.
