# DAY19: Drill — "Linked List with Rc"

Create `main.rs` in `DAY19/`.

## Requirements

Build a simple immutable linked list using `Rc` to share nodes.

```rust
use std::rc::Rc;

// A node in a linked list
#[derive(Debug)]
enum List {
    Cons(i32, Rc<List>),
    Nil,
}

use List::{Cons, Nil};

fn main() {
    // Create list a: [5, 10]
    let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
    println!("Reference count after creating a: {}", Rc::strong_count(&a));

    // TODO: Create list b that starts with 3 and shares the rest of a
    // b: [3, 5, 10]
    let b = Cons(3, Rc::clone(&a));
    println!("Reference count after creating b: {}", Rc::strong_count(&a));

    // TODO: Create list c that starts with 4 and shares the rest of a
    // c: [4, 5, 10]
    let c = Cons(4, Rc::clone(&a));
    println!("Reference count after creating c: {}", Rc::strong_count(&a));

    // Print all lists
    print_list(Some(&b), "b");
    print_list(Some(&c), "c");

    println!("Reference count at end: {}", Rc::strong_count(&a));
}

// Helper to print a list
fn print_list(list: Option<&List>, name: &str) {
    print!("{}: ", name);
    // TODO: walk the list and print each element
    // Use a loop with a mutable cursor
}
```

## Example output

```
Reference count after creating a: 1
Reference count after creating b: 2
Reference count after creating c: 3
b: 3 -> 5 -> 10 -> nil
c: 4 -> 5 -> 10 -> nil
Reference count at end: 3
```

## Hints

<details>
<summary>Click for hints</summary>

- `Rc::clone(&a)` increments the reference count — this is cheap!
- For `print_list`, start with `let mut current = list;`
- Then loop: `while let Some(node) = current { match node { Cons(val, next) => { print!("{} -> ", val); current = Some(next); }, Nil => { println!("nil"); break; } } }`
- Actually, since `a` is `Rc<List>`, you'll need to adjust types. The `Cons` variant stores `Rc<List>`, so `current` should be `Option<&List>` or `&List`
</details>

## Bonus

Add a method `fn len(&self) -> usize` to `List` that counts the elements recursively.

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-rust/DAY19
rustc main.rs && ./main
```

## Before you ask for review

- Does it compile?
- Are the reference counts correct?
- Do b and c share nodes with a?
- What happens to the reference count when b and c go out of scope?

## When you're done

Show me your `main.rs`.
