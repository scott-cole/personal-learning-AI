# DAY03: Ownership — The Core Rust Concept

---

## The anecdote: "Ownership is a library book — only one borrower at a time"

Imagine a library. You check out a book. While you have it, no one else can take it.

When you're done, you return it to the library. If you try to give it to a friend, the library says "you don't own this book — you were just borrowing it."

Rust's ownership works exactly the same way:

```rust
let book = String::from("The Rust Programming Language");
let friend = book;  // you GIVE the book to your friend

println!("{}", book);  // ERROR! You no longer own the book
```

After `let friend = book;`, the variable `book` is **moved**. You can't use it anymore.

---

## The Three Rules of Ownership

1. **Each value in Rust has exactly one owner.**
2. **When the owner goes out of scope, the value is dropped (freed).**
3. **When you assign or pass a value, ownership moves (transfer).**

```rust
fn main() {
    let s1 = String::from("hello");  // s1 owns the string
    let s2 = s1;                     // ownership moves to s2
    // s1 is now INVALID — don't use it!
    println!("{}", s2);              // OK — s2 owns it now
}  // s2 goes out of scope, string is freed
```

---

## Why not just copy everything?

Copying takes memory and time. A `String` stores text on the heap. If Rust copied the heap data on every assignment, programs would be slow and memory-hungry.

Instead, Rust **moves** ownership. It's like handing someone the keys to a car rather than building a second car.

---

## The `.clone()` escape hatch

If you genuinely need a copy, you can ask for one explicitly:

```rust
let s1 = String::from("hello");
let s2 = s1.clone();  // deep copy — both s1 and s2 are valid

println!("{} {}", s1, s2);  // OK — both still own their data
```

This makes it obvious to anyone reading your code: "Ah, this is an expensive operation."

---

## Stack-only types: The Copy trait

Simple types like integers, floats, and booleans live entirely on the stack. They implement the `Copy` trait, which means they're **copied automatically**:

```rust
let x = 5;
let y = x;          // x is COPIED, not moved
println!("{}", x);  // x is still valid! i32 implements Copy
```

Types that implement `Copy`: integers, floats, booleans, chars, tuples of Copy types.

Types that DON'T implement `Copy`: `String`, `Vec`, `HashMap` — anything that owns heap data.

---

## Ownership in functions

Passing a value to a function **moves** ownership:

```rust
fn take_ownership(s: String) {
    println!("{}", s);
}  // s is dropped here

fn main() {
    let s = String::from("hello");
    take_ownership(s);   // s is MOVED into the function
    // println!("{}", s);  // ERROR! s is gone
}
```

To get the value back, return it:

```rust
fn give_back(s: String) -> String {
    println!("{}", s);
    s  // return ownership to the caller
}
```

But this is tedious. That's why we have **borrowing** (DAY04).

---

## Summary

| Concept | Rule |
|---------|------|
| Ownership | Every value has exactly one owner |
| Move | Assigning or passing transfers ownership |
| Copy | Stack-only types (i32, f64, bool) are copied automatically |
| Clone | `.clone()` makes an explicit deep copy |
| Scope | When owner goes out of scope, value is dropped |

Your turn. Open DAY03/DRILL.md.
