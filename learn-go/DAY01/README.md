# DAY01: Variables, Types, and Your First Program

---

## The anecdote: "Variables are labelled boxes on a shelf"

Imagine you have a shelf in your garage. You put boxes on it.

Each box has:
- A **label** so you know what's inside (`name`, `age`, `price`)
- A **size** so you know what fits (`string`, `int`, `float64`)
- **Contents** that can change later (`"Scott"`, `30`, `19.99`)

In Go:

```go
var name string = "Scott"
var age int = 30
var price float64 = 19.99
```

That's it. That's a variable. A labelled box of a specific size with something inside.

**But Go programmers are lazy** (in a good way). Writing `var name string = "Scott"` is repetitive — we already know `"Scott"` is a string. So Go has a shortcut:

```go
name := "Scott"     // Go figures out it's a string
age := 30           // Go figures out it's an int
price := 19.99      // Go figures out it's a float64
```

The `:=` operator says **"declare and figure out the type for me"**. This is the most common way you'll write variables in Go.

---

## The types you need today

| Type | What it holds | Example |
|------|--------------|---------|
| `string` | Text (words, sentences) | `"Hello"`, `"Scott"` |
| `int` | Whole numbers | `30`, `-5`, `0`, `1000000` |
| `float64` | Decimal numbers | `3.14`, `-0.5`, `99.99` |
| `bool` | True or false | `true`, `false` |

**Production rule #1:** Use `int64` for money (pence/cents), never `float64`. Floats are approximations. `0.1 + 0.2` might be `0.30000000000000004`. In banking, that's a disaster. But for today, plain `int` is fine.

---

## Your first Go program

Every Go program has the same skeleton:

```go
package main   // 1. Every executable starts with package main

import "fmt"   // 2. Import the toolbox you need

func main() {  // 3. The front door — execution starts here
    // your code goes here
}
```

This is like the foundation of a house. You always start here.

### What is `fmt`?

`fmt` is the "format" package. It's your way to print things to the screen. Think of it like `console.log` in JavaScript:

```go
fmt.Println("Hello, world!")     // prints: Hello, world!
fmt.Printf("I am %d years old", 30)  // prints: I am 30 years old
```

`%d` means "put an integer here". `%s` means "put a string here". `%T` means "what TYPE is this?".

```go
name := "Scott"
age := 30
fmt.Printf("Name: %s, Age: %d\n", name, age)
// prints: Name: Scott, Age: 30
```

The `\n` is a newline — like pressing Enter.

---

## Running your code

Save a file as `main.go`, then in your terminal:

```bash
go run main.go
```

This compiles and runs in one step. No build step needed for learning.

---

## Senior mindset: "Compile early, compile often"

Don't write 50 lines before running. Write 3 lines. Run. See it work. Add 3 more. Run again.

Errors are not failures — they're the compiler teaching you.

Go's compiler is your strictest code reviewer. It will refuse to compile if:
- You declare a variable and don't use it
- You import a package and don't use it
- You have a typo

This is a **feature**, not a bug. The compiler catches what a linter would catch in JavaScript.

---

## Summary

| Concept | Syntax |
|---------|--------|
| Declare a variable | `var name string = "Scott"` |
| Shortcut declaration | `name := "Scott"` |
| Print with newline | `fmt.Println("hello")` |
| Formatted print | `fmt.Printf("Age: %d", 30)` |
| Run your code | `go run main.go` |

Your turn. Open DAY01/DRILL.md.
