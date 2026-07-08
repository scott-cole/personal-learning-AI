# DAY13: Lifetimes (Basic Annotations, Elision Rules)

---

## The anecdote: "Lifetimes are a library due-date stamp"

When you borrow a book from the library, the librarian stamps a due date on it. The book is only valid until that due date. After that, you must return it.

Lifetimes in Rust are like due-date stamps for references. A lifetime annotation says "this reference is valid until this point."

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

The `'a` (pronounced "tick-a" or "lifetime a") says: "the returned reference lives as long as the shorter of x and y."

---

## Why do we need lifetimes?

Every reference has a lifetime — the scope for which it's valid. Most of the time, Rust can figure it out (elision). But sometimes it can't, and you need to annotate.

```rust
fn main() {
    let s1 = String::from("long string");
    let result;
    {
        let s2 = String::from("short");
        result = longest(&s1, &s2);  // s2 might not live long enough
    }
    println!("{}", result);  // ERROR! s2 was dropped
}
```

The lifetime annotation `'a` ensures the return value doesn't outlive the shorter input.

---

## Lifetime annotation syntax

```rust
&i32        // a reference
&'a i32     // a reference with an explicit lifetime
&'a mut i32 // a mutable reference with an explicit lifetime
```

The `'a` is just a name. You could call it `'z` or `'foo`, but convention is `'a`, `'b`, `'c`.

---

## Functions with lifetimes

```rust
// Two references, one comes back — lifetimes needed
fn first<'a>(x: &'a str, y: &'a str) -> &'a str {
    x
}

// One input, one output — lifetimes are obvious (elision)
fn first_word(s: &str) -> &str {
    // Rust figures it out automatically
}

// Multiple lifetimes
fn longest_with_announcement<'a, 'b>(
    x: &'a str,
    y: &'a str,
    announcement: &'b str,
) -> &'a str {
    println!("{}", announcement);
    if x.len() > y.len() { x } else { y }
}
```

---

## Lifetime elision rules

Rust has three rules that let you omit lifetimes in most cases:

1. **Each input reference gets its own lifetime.** `fn f(x: &i32)` → `fn f<'a>(x: &'a i32)`
2. **If there's one input lifetime, all output references get it.** `fn f(x: &i32) -> &i32` → `fn f<'a>(x: &'a i32) -> &'a i32`
3. **If there's `&self` or `&mut self`, its lifetime is assigned to all output references.** This applies to methods.

---

## Lifetimes in structs

When a struct holds references, it needs lifetime annotations:

```rust
struct Excerpt<'a> {
    part: &'a str,  // Excerpt can't outlive the string it references
}

fn main() {
    let novel = String::from("Call me Ishmael.");
    let first_sentence = novel.split('.').next().expect("no sentence");
    let excerpt = Excerpt { part: first_sentence };
    // excerpt can't outlive novel
}
```

---

## The 'static lifetime

`'static` means the reference lives for the entire program. String literals are `'static`:

```rust
let s: &'static str = "I live forever";
```

But be careful — `'static` doesn't mean "this value is immutable." Use it sparingly.

---

## Summary

| Concept | Syntax | Purpose |
|---------|--------|---------|
| Lifetime annotation | `'a` | Names a reference's valid scope |
| Function with lifetimes | `fn f<'a>(x: &'a str) -> &'a str` | Ties output to input |
| Lifetime in struct | `struct S<'a> { f: &'a T }` | Struct with references |
| Static lifetime | `'static` | Lives for entire program |
| Elision | Omit lifetimes | Rust figures them out |

Your turn. Open DAY13/DRILL.md.
