# DAY17: Modules, Packages, Crates, use

---

## The anecdote: "Modules are rooms in a house; crates are neighbouring houses"

Imagine a house. It has rooms — a kitchen, a bedroom, a living room. Each room has a door. You can see into a room from the hallway, but you need permission to enter.

In Rust:
- A **module** is a room in your house
- A **crate** is the whole house (a library or binary)
- An **external crate** is a neighbour's house
- `pub` is an open door
- `use` is walking into a room

---

## Your house (your crate)

Every Cargo project is a crate. `src/main.rs` and `src/lib.rs` are **crate roots** — they form the root module.

```rust
// src/main.rs — the front door
fn main() {
    println!("Hello");
}
```

---

## Defining modules (rooms)

```rust
// src/main.rs
mod kitchen {
    pub fn cook() {
        println!("Cooking...");
    }
}

mod bedroom {
    pub fn sleep() {
        println!("Sleeping...");
    }
}

fn main() {
    kitchen::cook();
    bedroom::sleep();
}
```

---

## Module privacy

By default, everything is **private** (only accessible within the module and its children). Use `pub` to make it public.

```rust
mod kitchen {
    fn wash_dishes() {
        // Private — only kitchen functions can call this
    }

    pub fn cook() {
        wash_dishes();  // OK — same module
        println!("Cooking...");
    }
}

fn main() {
    // kitchen::wash_dishes();  // ERROR! private function
    kitchen::cook();  // OK — public
}
```

---

## Module files

For larger projects, put each module in its own file:

```rust
// src/kitchen.rs
pub fn cook() {
    println!("Cooking...");
}

// src/main.rs
mod kitchen;  // looks for src/kitchen.rs

fn main() {
    kitchen::cook();
}
```

---

## External crates (neighbouring houses)

Add dependencies in `Cargo.toml`:

```toml
[dependencies]
serde = "1"
```

Then import in your code:

```rust
use serde::Serialize;
```

---

## The use keyword

```rust
// Long form
std::collections::HashMap;

// With use
use std::collections::HashMap;
let map = HashMap::new();

// Nested paths
use std::{cmp::Ordering, collections::HashMap};

// Glob — import everything
use std::collections::*;
```

---

## Re-exports

```rust
// In src/lib.rs
mod kitchen;

// Re-export so users can use my_crate::cook directly
pub use kitchen::cook;
```

---

## Summary

| Concept | Syntax | Purpose |
|---------|--------|---------|
| Define module | `mod name { }` | Create a namespace |
| Module from file | `mod name;` | Load from name.rs or name/mod.rs |
| Make public | `pub fn` | Export from module |
| Import item | `use path::to::Item` | Bring into scope |
| Nested use | `use {A, B}` | Import multiple items |
| External crate | `crate-name` in Cargo.toml | Use library from crates.io |

Your turn. Open DAY17/DRILL.md.
