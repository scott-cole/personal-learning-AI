# DAY15: Closures (|| Syntax, Capturing, move)

---

## The anecdote: "Closures are a Polaroid camera — captures the moment"

A Polaroid camera captures a moment in time — the people, the lighting, the background — and freezes it in a photo.

A closure captures the **environment** around it — variables that were in scope when the closure was created — and freezes them for later use.

```rust
let favorite_color = String::from("blue");

let print_color = || {
    println!("My favorite color is {}", favorite_color);
    // favorite_color was "in the room" when this closure was born
};

print_color();  // prints: My favorite color is blue
```

---

## Closure syntax

```rust
// No parameters
let greet = || println!("Hello!");

// One parameter
let square = |x: i32| -> i32 { x * x };

// Multiple parameters (type inference)
let add = |a, b| a + b;

println!("{}", square(5));   // 25
println!("{}", add(2, 3));   // 5
```

The `||` pipes are the "lens" of the Polaroid camera — they define what parameters the closure accepts. The body is what happens when you press the button.

---

## Capturing by reference vs value

Closures can capture variables in three ways:

```rust
let x = String::from("hello");

// By reference (&T) — default for reading
let print = || println!("{}", x);
print();
println!("{}", x);  // still valid

// By mutable reference (&mut T) — default for modifying
let mut x = String::from("hello");
let mut append = || x.push_str(" world");
append();
// println!("{}", x);  // ERROR: borrowed as mutable

// By value (move) — when you use move keyword
let x = String::from("hello");
let take = move || {
    println!("{}", x);
    // x is MOVED into the closure
};
// println!("{}", x);  // ERROR: x was moved
take();
```

---

## The move keyword

`move` forces the closure to take ownership of captured variables. This is essential when:
- The closure will outlive the current function (e.g., spawning a thread)
- You're returning the closure

```rust
fn make_adder(x: i32) -> impl Fn(i32) -> i32 {
    move |y| x + y  // must move x into the closure
}

let add_five = make_adder(5);
println!("{}", add_five(3));  // 8
```

---

## Closure traits: Fn, FnMut, FnOnce

Every closure implements one or more of these traits:

| Trait | Captures | Can call | Notes |
|-------|----------|----------|-------|
| `Fn` | By shared ref | Multiple times | Read-only capture |
| `FnMut` | By mut ref | Multiple times | Can modify captured values |
| `FnOnce` | By value | Once | Consumes captured values |

```rust
fn call_fn(f: impl Fn()) {
    f();
}

fn call_fn_mut(mut f: impl FnMut()) {
    f();
}

fn call_fn_once(f: impl FnOnce()) {
    f();
}
```

---

## Summary

| Concept | Syntax | Notes |
|---------|--------|-------|
| Basic closure | `\|x\| x + 1` | Type inference |
| Closure with body | `\|x\| { x + 1 }` | Multiple statements |
| Move closure | `move \|x\| x + y` | Takes ownership |
| Fn trait | `impl Fn() -> T` | Read-only capture |
| FnMut trait | `impl FnMut() -> T` | Mutable capture |
| FnOnce trait | `impl FnOnce() -> T` | Consuming capture |

Your turn. Open DAY15/DRILL.md.
