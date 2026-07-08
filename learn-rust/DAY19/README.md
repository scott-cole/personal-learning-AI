# DAY19: Smart Pointers (Box&lt;T&gt;, Rc&lt;T&gt;, RefCell&lt;T&gt;)

---

## The anecdote: "Box is a shipping container; Rc is a club membership card"

Imagine a **shipping container** — you put something inside, seal it, and send it off. The container owns the contents. You don't care about the size of the contents — you just need to know the container's size. That's **Box\<T\>** — it puts data on the heap.

Now imagine a **country club membership card**. Multiple people can hold a copy of the card, and as long as anyone holds it, the club stays open. When the last person leaves, the club closes. That's **Rc\<T\>** — reference counting.

---

## Box<T> — Heap allocation

Use `Box` when you have a type whose size isn't known at compile time (like recursive types):

```rust
// This won't compile — List contains itself, size unknown
// enum List {
//     Cons(i32, List),
//     Nil,
// }

// This works — Box adds a layer of indirection
enum List {
    Cons(i32, Box<List>),
    Nil,
}

fn main() {
    let list = Cons(1, Box::new(Cons(2, Box::new(Cons(3, Box::new(Nil))))));
}
```

Also useful for:
- Large data you want to move cheaply (Box is just a pointer)
- Trait objects (more on that later)

---

## Rc<T> — Reference Counting

`Rc` (Reference Counted) allows **multiple ownership**. It keeps track of how many references exist. When the count reaches zero, the value is dropped.

```rust
use std::rc::Rc;

let a = Rc::new(String::from("hello"));
let b = Rc::clone(&a);  // reference count: 2
let c = Rc::clone(&a);  // reference count: 3

println!("Count: {}", Rc::strong_count(&a));  // 3
```

**Note:** `Rc` is only for single-threaded use. For threads, use `Arc`.

---

## RefCell<T> — Interior Mutability

`RefCell` lets you mutate data even when you only have a shared (immutable) reference. It enforces borrowing rules at **runtime** instead of compile time.

```rust
use std::cell::RefCell;

let data = RefCell::new(5);

// Borrow immutably
let borrowed = data.borrow();
println!("{}", borrowed);
drop(borrowed);  // End the borrow

// Borrow mutably — even though data is immutable!
let mut borrowed_mut = data.borrow_mut();
*borrowed_mut += 1;
```

**Warning:** `RefCell` panics at runtime if you break borrowing rules (e.g., borrow twice mutably).

---

## Combining Rc and RefCell

A common pattern: `Rc<RefCell<T>>` — multiple owners who can mutate.

```rust
use std::rc::Rc;
use std::cell::RefCell;

let shared = Rc::new(RefCell::new(5));

let a = Rc::clone(&shared);
let b = Rc::clone(&shared);

*shared.borrow_mut() += 1;
*a.borrow_mut() += 1;
*b.borrow_mut() += 1;

println!("{}", shared.borrow());  // 8
```

---

## Summary

| Smart Pointer | What it does | When to use |
|--------------|-------------|-------------|
| `Box<T>` | Heap allocation | Recursive types, trait objects |
| `Rc<T>` | Reference counting (single-threaded) | Multiple ownership |
| `RefCell<T>` | Runtime borrow checking | Interior mutability |
| `Rc<RefCell<T>>` | Multi-owner mutable data | Graphs, shared state |

Your turn. Open DAY19/DRILL.md.
