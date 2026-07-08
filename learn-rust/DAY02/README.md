# DAY02: Functions, Expressions vs Statements, Return Values

---

## The anecdote: "A function is a recipe"

Imagine you're baking a cake.

A recipe takes **ingredients** (flour, eggs, sugar) and produces a **cake**. You follow the same recipe every time, but the result depends on what you put in.

A Rust function works the same way:

```rust
fn make_cake(flour: i32, eggs: i32, sugar: i32) -> i32 {
    flour + eggs * 2 + sugar * 3
}
```

- `fn` means "this is a function" (like a recipe card)
- `flour: i32` means "this recipe needs flour, measured in grams"
- `-> i32` means "the result is a number" (cake weight in grams)
- The body `{ }` is the list of steps

---

## Calling a function

Once you've written the recipe, you use it:

```rust
let cake = make_cake(200, 3, 100);
println!("Cake weighs {}g", cake);
```

You put in `200g flour, 3 eggs, 100g sugar`. You get a number back.

---

## Expressions vs Statements

This is the single most important concept in Rust, and it trips everyone up.

- **Statement:** An instruction that does something but returns nothing.
- **Expression:** Something that evaluates to a value.

```rust
let x = 5;      // statement — it's an instruction, returns nothing
x + 2           // expression — it evaluates to 7
```

The key difference: **expressions have semicolons; expressions DON'T.**

```rust
fn main() {
    let y = {
        let x = 3;
        x + 1   // expression — no semicolon! This block evaluates to 4
    };
    println!("{}", y); // prints: 4
}
```

Notice `x + 1` has no semicolon. That means it's the **return value** of the block. If you put a semicolon:

```rust
    let y = {
        let x = 3;
        x + 1;  // statement now — returns () (unit/void)
    };
    // y is (), not 4 !
```

This is the #1 source of confusion for new Rust programmers. **Remember: no semicolon = return this value.**

---

## Return values

Functions can return a value two ways:

### Way 1: Expression (no semicolon, no return keyword)

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b   // no semicolon — this is the return value
}
```

### Way 2: Early return with `return` keyword

```rust
fn add(a: i32, b: i32) -> i32 {
    if a < 0 {
        return 0;  // early exit — return keyword needs semicolon
    }
    a + b   // normal return — no semicolon
}
```

Use `return` only for early exits. The idiomatic Rust style is the expression form.

---

## Functions that return nothing

If a function doesn't return anything, you can omit the `->`:

```rust
fn greet(name: &str) {
    println!("Hello, {}!", name);
    // no return value — this is ()
}
```

The type of "nothing" in Rust is `()` (called "unit"). It's like `void` in other languages.

---

## Multiple parameters

```rust
fn describe_person(name: &str, age: i32, city: &str) -> String {
    format!("{} is {} years old and lives in {}", name, age, city)
}
```

`format!` is like `println!` but returns a `String` instead of printing it.

---

## Summary

| Concept | Syntax |
|---------|--------|
| Define a function | `fn name(param: Type) -> Type { }` |
| Return a value (expression) | Last line, no semicolon |
| Return early | `return value;` |
| Expression | `x + 1` (has a value) |
| Statement | `let x = 5;` (no value) |
| Function with no return | `fn name(param: Type) { }` |
| Return nothing explicitly | `return;` or just end the block |

Your turn. Open DAY02/DRILL.md.
