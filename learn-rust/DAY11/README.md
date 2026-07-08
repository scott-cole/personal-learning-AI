# DAY11: Generics

---

## The anecdote: "Generics are a universal remote"

A universal remote works with any brand of TV — Samsung, LG, Sony, it doesn't matter. The remote is **generic** over the TV type.

In Rust, generics let you write code that works with **any type**:

```rust
fn largest<T>(list: &[T]) -> &T {
    &list[0]  // return the first element, works for any type T
}
```

The `<T>` says "this function is generic over some type T." You can think of T as a placeholder that gets filled in when you call the function.

---

## Generic functions

```rust
fn get_first<T>(slice: &[T]) -> &T {
    &slice[0]
}

fn main() {
    let numbers = vec![1, 2, 3];
    let first_num = get_first(&numbers);   // T = i32

    let words = vec!["hello", "world"];
    let first_word = get_first(&words);    // T = &str

    println!("{} {}", first_num, first_word);  // 1 hello
}
```

---

## Generic structs

```rust
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let int_point = Point { x: 5, y: 10 };       // Point<i32>
    let float_point = Point { x: 1.0, y: 4.0 };   // Point<f64>
}
```

But what if you want x and y to be different types?

```rust
struct Point<T, U> {
    x: T,
    y: U,
}

let mixed = Point { x: 5, y: 4.0 };  // Point<i32, f64>
```

---

## Generic methods

```rust
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

// Method only available for Point<f64>:
impl Point<f64> {
    fn distance_from_origin(&self) -> f64 {
        (self.x.powi(2) + self.y.powi(2)).sqrt()
    }
}
```

---

## Monomorphization

When you compile, Rust generates **concrete code** for each type you use. If you use `Point<i32>` and `Point<f64>`, Rust generates two copies of the code.

This means generics in Rust have **zero runtime cost**. No boxing, no dynamic dispatch (unless you use trait objects, which we'll cover later).

---

## Where you've already seen generics

- `Vec<T>` — a vector of any type
- `Option<T>` — a value that might be Some or None
- `Result<T, E>` — a value that might be Ok or Err
- `HashMap<K, V>` — a map with generic key and value types

---

## Summary

| Concept | Syntax | Example |
|---------|--------|---------|
| Generic function | `fn name<T>(x: T)` | `fn id<T>(x: T) -> T { x }` |
| Generic struct | `struct Name<T> { field: T }` | `struct Point<T> { x: T, y: T }` |
| Generic method | `impl<T> Type<T> { fn method(&self) }` | `impl<T> Point<T> { fn x(&self) }` |
| Multiple generics | `<T, U>` | `struct Pair<T, U> { first: T, second: U }` |

Your turn. Open DAY11/DRILL.md.
