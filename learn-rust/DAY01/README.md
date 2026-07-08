# DAY01: Variables, Mutability, and Your First Rust Program

---

## The anecdote: "Variables are labelled boxes on a shelf; mutability is a lockable lid"

Imagine you have a shelf in your garage. You put boxes on it.

Each box has:
- A **label** so you know what's inside (`name`, `age`, `price`)
- A **size** so you know what fits (`String`, `i32`, `f64`)
- A **lid** that can be locked or unlocked

In Rust, **every box lid is locked by default**:

```rust
let name = "Scott".to_string();   // locked — cannot change
let age = 30;                     // locked — cannot change
```

If you want to change what's inside later, you need an **unlocked lid**:

```rust
let mut age = 30;    // unlocked — can change later
age = 31;            // OK, the lid was unlocked
```

The `mut` keyword (short for mutable) makes the variable changeable. Without it, the compiler refuses to let you modify it.

**Why does Rust do this?** Because locked boxes are easier to reason about. When you read code and see `let x = 5`, you know `x` will ALWAYS be `5`. You don't need to trace through 500 lines to see if someone changed it.

---

## The types you need today

| Type | What it holds | Example |
|------|--------------|---------|
| `i32` | Whole numbers (32-bit) | `30`, `-5`, `0` |
| `f64` | Decimal numbers (64-bit) | `3.14`, `-0.5`, `99.99` |
| `bool` | True or false | `true`, `false` |
| `String` | Growable text | `"Hello".to_string()` |
| `&str` | Fixed text (string slice) | `"Hello"` |

**Production rule #1:** Use `i64` for money (pence/cents), never `f64`. Floats are approximations — `0.1 + 0.2` might be `0.30000000000000004`. But for today, plain `i32` is fine.

---

## Your first Rust program

Every Rust program has the same skeleton:

```rust
fn main() {  // The front door — execution starts here
    // your code goes here
}
```

This is like the foundation of a house. You always start here.

### Printing to the screen

```rust
println!("Hello, world!");        // prints: Hello, world!
println!("I am {} years old", 30); // prints: I am 30 years old
```

The `{}` is a placeholder — like a blank on a form. Rust fills it in with the value you provide.

```rust
let name = "Scott";
let age = 30;
println!("Name: {}, Age: {}", name, age);
// prints: Name: Scott, Age: 30
```

---

## Type inference

Rust is smart enough to figure out most types on its own:

```rust
let x = 5;          // Rust infers i32
let y = 3.14;       // Rust infers f64
let z = true;       // Rust infers bool
```

But you can be explicit when you want:

```rust
let x: i32 = 5;
let y: f64 = 3.14;
```

The colon syntax `: type` means "this variable is of this type".

---

## Shadowing

Rust lets you **redeclare** a variable with the same name. This is called shadowing:

```rust
let x = 5;
let x = x + 1;  // shadows the previous x
println!("{}", x); // prints: 6
```

This is NOT mutation. It creates a brand-new box that happens to have the same label as the old one. The old box is gone.

Shadowing is useful when you want to transform a value without making it mutable:

```rust
let name = "scott";
let name = name.to_uppercase();
println!("{}", name); // prints: SCOTT
```

---

## Running your code

Save a file as `main.rs`, then:

```bash
rustc main.rs
./main
```

Or, for a proper project:

```bash
cargo new day01 --bin
cd day01
cargo run
```

But for today, a single `main.rs` file is enough.

---

## Senior mindset: "The compiler is your pair programmer"

Don't write 50 lines before running. Write 3 lines. Run. See it work. Add 3 more. Run again.

Rust's compiler is the strictest you'll ever meet. It will refuse to compile if:
- You declare a variable and don't use it (prefix with `_` to silence)
- You try to modify an immutable variable
- You have a type mismatch

This is a **feature**, not a bug. The compiler catches what would be runtime bugs in other languages.

---

## Summary

| Concept | Syntax |
|---------|--------|
| Declare a variable | `let name = "Scott";` |
| Mutable variable | `let mut age = 30;` |
| Type annotation | `let x: i32 = 5;` |
| Print with newline | `println!("hello");` |
| Print with placeholders | `println!("{} and {}", a, b);` |
| Shadowing | `let x = x + 1;` |
| Run a single file | `rustc main.rs && ./main` |
| Run a cargo project | `cargo run` |

Your turn. Open DAY01/DRILL.md.
