# DAY08: Enums & Pattern Matching (match, if let)

---

## The anecdote: "An enum is a vending machine; match is pressing the right button"

Imagine a vending machine. Each slot has an item — but different slots have different things. Slot A1 has chips, B2 has a soda can, C3 has a candy bar.

You press a button, and the machine gives you whatever was in that slot, in the right way — it drops chips, rolls a can, or slides a candy bar.

An enum in Rust is exactly this: a type that can be **one of several variants**, each potentially holding different data.

```rust
enum Snack {
    Chips(u32),     // holds a gram count
    Soda(String),   // holds a flavor
    Candy(String),  // holds a brand
}
```

And `match` is pressing the right button — it handles each variant differently.

---

## Defining an enum

```rust
enum Direction {
    Up,
    Down,
    Left,
    Right,
}
```

The simplest enum — each variant holds no extra data.

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```

Variants can hold named fields (like a struct), unnamed data (like a tuple), or nothing.

---

## Using match

```rust
fn move_player(dir: Direction) {
    match dir {
        Direction::Up => println!("Moving up"),
        Direction::Down => println!("Moving down"),
        Direction::Left => println!("Moving left"),
        Direction::Right => println!("Moving right"),
    }
}
```

`match` is exhaustive — you MUST handle every variant. This is a superpower. If you add a variant to an enum, the compiler tells you every place that needs updating.

---

## Match with data

```rust
fn handle_message(msg: Message) {
    match msg {
        Message::Quit => println!("Goodbye"),
        Message::Move { x, y } => println!("Move to ({}, {})", x, y),
        Message::Write(text) => println!("Message: {}", text),
        Message::ChangeColor(r, g, b) => println!("Color: {} {} {}", r, g, b),
    }
}
```

Rust destructures the enum variant and binds the inner data to variables.

---

## The Option enum

Rust has no `null`. Instead, it has `Option<T>`:

```rust
enum Option<T> {
    None,
    Some(T),
}
```

This is so fundamental it's in the prelude (no need to import):

```rust
fn find_user(id: i32) -> Option<String> {
    if id == 1 {
        Some(String::from("Scott"))
    } else {
        None
    }
}

match find_user(1) {
    Some(name) => println!("Found: {}", name),
    None => println!("User not found"),
}
```

You MUST handle the `None` case — no null pointer exceptions!

---

## if let

When you only care about one variant, `if let` is shorter:

```rust
let config = Some(3);

if let Some(x) = config {
    println!("Config value: {}", x);
}
```

This is a more concise version of:

```rust
match config {
    Some(x) => println!("Config value: {}", x),
    None => (),  // do nothing — boilerplate
}
```

Use `match` when multiple variants matter. Use `if let` when you only care about one.

---

## Summary

| Concept | Syntax | Notes |
|---------|--------|-------|
| Define enum | `enum Name { Var1, Var2 }` | — |
| Enum with data | `enum Name { Var1(Type), Var2 { f: Type } }` | — |
| Match | `match val { Pat => code, _ => code }` | Must be exhaustive |
| Option | `Option<T>` = `Some(T)` or `None` | No null! |
| if let | `if let Some(x) = val { }` | Shorthand for single variant |
| Wildcard | `_` | Matches anything |

Your turn. Open DAY08/DRILL.md.
