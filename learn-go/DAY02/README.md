# DAY02: Functions, Returns, and Errors

---

## The anecdote: "A function is a blender"

You put ingredients in. You press a button. Something different comes out.

```
Ingredients (inputs) → [ BLENDER ] → Smoothie (output)
```

A function is the same:

```go
func greet(name string) string {
    return "Hello, " + name
}
```

```
"Scott" (input) → [ greet ] → "Hello, Scott" (output)
```

**Parts of a function:**

```go
func greet(name string) string {
    // ^     ^       ^         ^
    // |     |       |         |
    // keyword  name  input type  output type
    return "Hello, " + name
}
```

---

## Multiple returns

Go functions can return **more than one thing**. This is special — many languages can't do this.

The most common use: return a **value** AND an **error**.

```go
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, errors.New("can't divide by zero")
    }
    return a / b, nil
}
```

`nil` means "no error". It's Go's `null` specifically for errors.

**Calling a function that returns two values:**

```go
result, err := divide(10, 2)
if err != nil {
    fmt.Println("Something went wrong:", err)
    return
}
fmt.Println("Result:", result)
```

**This is the most important pattern in Go.** You will write this hundreds of times:

```go
result, err := SomeFunction()
if err != nil {
    // handle it
}
```

---

## Named return values

You can name the return values. Go creates those variables automatically:

```go
func getCoordinates() (x int, y int) {
    x = 10   // no := needed, x already exists
    y = 20
    return   // returns x, y automatically
}
```

Use this **sparingly** — it can make code harder to follow. Only use it in small functions.

---

## Blank identifier `_`

Sometimes a function returns something you don't need. Use `_` to throw it away:

```go
result, _ := divide(10, 2)  // I don't care about the error
```

**Senior rule:** Only ignore errors when you KNOW it can't fail. And then leave a comment why.

---

## Senior mindset: "Never ignore an error"

Look at the `divide` function. The caller **must** check the error. The compiler won't enforce this — but your code quality will.

```go
result, err := divide(10, 0)
// If you don't check err, result is 0 and you don't know why
```

**Production rule #2:** Every `err` variable must be checked or explicitly ignored (`_`). No exceptions. A senior engineer spots an unchecked error in code review instantly.

---

## Importing "errors"

To create errors, you need:

```go
import "errors"

var err = errors.New("something went wrong")
```

---

## Summary

| Concept | Code |
|---------|------|
| Function with one return | `func add(a int, b int) int` |
| Function with two returns | `func div(a int, b int) (int, error)` |
| Create an error | `errors.New("message")` |
| Call and check | `result, err := div(10, 2)` |
| Ignore a return | `result, _ := div(10, 2)` |

Open DAY02/DRILL.md.
