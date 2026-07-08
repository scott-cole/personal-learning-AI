# DAY17: Drill — "Modular Math Library"

For this drill, create a proper Cargo project with multiple files.

```bash
cargo new --bin math_tools
cd math_tools
```

## Requirements

Create a modular math library with the following structure:

```
src/
  main.rs
  operations.rs
  utils.rs
```

### src/operations.rs

```rust
// TODO: make this module visible outside
mod operations {  // Make pub
    // TODO: function that returns a + b
    fn add(a: i32, b: i32) -> i32 { a + b }

    // TODO: function that returns a * b
    fn multiply(a: i32, b: i32) -> i32 { a * b }

    // Private helper — should not be accessible from main
    fn internal_helper() {
        println!("This is internal");
    }
}
```

### src/utils.rs

```rust
// TODO: make this module visible outside
mod utils {  // Make pub
    // TODO: function that returns true if n is even
    fn is_even(n: i32) -> bool { n % 2 == 0 }

    // TODO: function that returns the absolute value
    fn abs(n: i32) -> i32 {
        if n < 0 { -n } else { n }
    }
}
```

### src/main.rs

```rust
// TODO: declare the modules
mod operations;
mod utils;

// TODO: use the functions from both modules
// use operations::{add, multiply};
// use utils::is_even;

fn main() {
    let sum = add(10, 5);
    let product = multiply(10, 5);
    let even_check = is_even(10);
    let abs_val = utils::abs(-7);

    println!("10 + 5 = {}", sum);
    println!("10 * 5 = {}", product);
    println!("10 is even: {}", even_check);
    println!("|-7| = {}", abs_val);
}
```

## Example output

```
10 + 5 = 15
10 * 5 = 50
10 is even: true
|-7| = 7
```

## Hints

<details>
<summary>Click for hints</summary>

- In `main.rs`: `mod operations;` and `mod utils;` declare the modules
- In each file, the module declaration should be `pub mod operations { ... }` or just `pub fn` inside
- Actually, since these are separate files, the file IS the module. Don't wrap in `mod operations { }` in the file — just put `pub fn` directly
- In `main.rs`, `use operations::add;` to bring `add` into scope
- Or call `operations::add(10, 5)` directly
</details>

## How to run

```bash
cd ~/Dev/learn-rust/DAY17/math_tools
cargo run
```

## Before you ask for review

- Does it compile?
- Can you call `operations::add` from main?
- Can you call `internal_helper` from main? (Should not be able to)
- Try `cargo doc --open` to see auto-generated docs

## When you're done

Show me all three files.
