# DAY05: Slices (&str, &[T])

---

## The anecdote: "A slice is a viewing window into a bookshelf"

Imagine a long bookshelf filled with books. You don't want to take all the books — you just want to look at books 3 through 7.

You take a piece of cardboard, cut out a window, and place it over the shelf. Now you can only see books 3 through 7. That window is a **slice**.

In Rust, a slice is a **view** into a contiguous sequence of elements. It doesn't own the data — it just shows you a portion.

---

## String slices: &str

You've already seen `&str` — it's a slice of a `String`:

```rust
let s = String::from("hello world");

let hello = &s[0..5];    // "hello" — characters 0 to 4
let world = &s[6..11];   // "world" — characters 6 to 10
let whole = &s[..];      // "hello world" — everything
```

The syntax `[start..end]` means "from start (inclusive) to end (exclusive)."

```rust
let s = String::from("hello world");
let hello = &s[..5];     // same as [0..5]
let world = &s[6..];     // same as [6..len]
```

A string literal like `"hello"` is actually a `&str` — a slice pointing to read-only memory.

---

## Why slices exist

Before slices, you might write a function that takes a `&String`:

```rust
fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();
    for (i, &byte) in bytes.iter().enumerate() {
        if byte == b' ' {
            return &s[0..i];
        }
    }
    &s[..]
}
```

But this function is more useful if it accepts `&str` instead, because then it works with both `String` AND `&str`:

```rust
fn first_word(s: &str) -> &str {
    // same implementation
}

// Works with both:
first_word(&my_string[..]);  // String → &str
first_word(my_string_literal);  // &str directly
```

---

## Slice arrays

The same slicing works on arrays and vectors:

```rust
let arr = [1, 2, 3, 4, 5];
let slice = &arr[1..4];    // &[i32] — [2, 3, 4]
```

A `&[T]` is a slice of some type `T`. It's a fat pointer: it stores a pointer to the first element AND a length.

---

## Slices are safe

Slices have bounds checking at runtime. If you try to access beyond the slice:

```rust
let s = String::from("hello");
let slice = &s[0..10];  // runtime panic! String is only 5 chars
```

Rust won't let you create a slice past the end of the data.

---

## Summary

| Concept | Syntax | Notes |
|---------|--------|-------|
| String slice | `&s[start..end]` | View into a String |
| Full range | `&s[..]` | Entire string |
| From start | `&s[..end]` | From 0 to end |
| To end | `&s[start..]` | From start to len |
| Array slice | `&arr[start..end]` | Works on any array/vec |
| Type of string literal | `&str` | Immutable view |

Your turn. Open DAY05/DRILL.md.
