# DAY04: Borrowing & References (&T, &mut T)

---

## The anecdote: "Borrowing is reading a book in the library vs taking notes in it"

Yesterday you learned about ownership — a library book that only one person can have at a time.

But what if you just want to **look at** the book? You don't need to take it home. You can read it in the library. That's a **shared reference** (`&T`).

And what if you want to **add notes** to the book? You need a special desk where no one else can look at the book while you write. That's a **mutable reference** (`&mut T`).

```rust
let book = String::from("Rust Book");

let reader = &book;      // shared reference — you can look but not touch
let note_taker = &mut book;  // mutable reference — you can edit, but no one else can look
```

---

## Shared references: &T

A shared reference lets you **read** the data without taking ownership:

```rust
fn calculate_length(s: &String) -> usize {
    s.len()  // we can read s, but not modify it
}  // s does NOT go out of scope — we only borrowed it

fn main() {
    let s = String::from("hello");
    let len = calculate_length(&s);
    println!("'{}' has length {}", s, len);  // s is still valid!
}
```

The `&` means "borrow, don't take." The borrower gets to look, but the owner keeps the keys.

---

## Mutable references: &mut T

A mutable reference lets you **read AND write** the borrowed data:

```rust
fn add_exclamation(s: &mut String) {
    s.push_str("!");  // we can modify the borrowed string
}

fn main() {
    let mut s = String::from("hello");  // must be mutable!
    add_exclamation(&mut s);
    println!("{}", s);  // prints: hello!
}
```

The original variable must be `mut`, and you pass `&mut` to the function.

---

## The Borrowing Rules (THE MOST IMPORTANT RULES IN RUST)

1. **You may have ONE mutable reference OR any number of shared references, but NOT BOTH at the same time.**
2. **A reference must never outlive its owner.**

### Rule 1 in action:

```rust
let mut s = String::from("hello");

let r1 = &s;      // OK — shared ref #1
let r2 = &s;      // OK — shared ref #2
// let r3 = &mut s; // ERROR! Can't have shared and mutable at the same time

println!("{} {}", r1, r2);  // shared refs used here
// The shared refs are done — now we can have a mutable ref

let r3 = &mut s;  // OK — shared refs are no longer in use
println!("{}", r3);
```

### Rule 2 in action:

```rust
let r: &String;
{
    let s = String::from("hello");
    r = &s;
}  // s is dropped here
// println!("{}", r);  // ERROR! r points to dropped data
```

Rust literally will not let you write dangling pointer bugs. The compiler catches them at compile time.

---

## Why these rules?

Data races happen when:
1. Two or more pointers access the same data at the same time
2. At least one of them is writing
3. There's no synchronization

Rust's borrowing rules prevent data races at **compile time**. Not at runtime. Not with locks. At compile time. This is Rust's superpower.

---

## Summary

| Concept | Syntax | What you can do |
|---------|--------|-----------------|
| Shared reference | `&T` | Read only |
| Mutable reference | `&mut T` | Read and write |
| Borrow a value | `&val` or `&mut val` | — |
| Rule 1 | Either N shared XOR 1 mutable | Prevent data races |
| Rule 2 | Reference must not outlive owner | Prevent dangling pointers |

Your turn. Open DAY04/DRILL.md.
