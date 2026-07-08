# DAY13: Drill — "Lifetime Fixer"

Create `main.rs` in `DAY13/`.

## Requirements

Fix the lifetime bugs in this program. There are 4 problems to solve.

```rust
// PROBLEM 1: Function needs lifetime annotations
fn longest(x: &str, y: &str) -> &str {
    if x.len() > y.len() { x } else { y }
}

// PROBLEM 2: Struct uses references without lifetimes
struct Highlight {
    text: &str,
}

// PROBLEM 3: This function has a lifetime error — fix it
fn invalid_return() -> &str {
    let s = String::from("I will disappear");
    &s
}

fn main() {
    // PROBLEM 4: This usage of longest should compile
    let s1 = String::from("long");
    let result;
    {
        let s2 = String::from("short");
        result = longest(&s1, &s2);
    }
    // println!("{}", result);  // BUG: s2 might not live long enough

    // This should compile:
    let s3 = String::from("hello world");
    let highlight = Highlight { text: &s3[0..5] };
    println!("Highlight: {}", highlight.text);
}
```

## Expected behavior

After fixing:
1. `longest` should accept two string slices with a shared lifetime and return the longer one
2. `Highlight` should store a reference with a lifetime
3. `invalid_return` should be deleted or rewritten to accept a reference as parameter
4. The `main` function should compile and print properly

## Hints

<details>
<summary>Click for hints</summary>

- PROBLEM 1: Add `<'a>` and annotate both parameters and return as `&'a str`
- PROBLEM 2: Add `<'a>` to the struct: `struct Highlight<'a> { text: &'a str }`
- PROBLEM 3: Can't return a reference to a local — either return `String` or take a `&str` parameter
- PROBLEM 4: Move `result` usage inside the inner scope, or restructure so both strings live long enough
</details>

## How to run

```bash
cd ~/Dev/learn-rust/DAY13
rustc main.rs && ./main
```

## Before you ask for review

- Does it compile with zero errors?
- Are all lifetime annotations correct?
- Can you explain why `invalid_return` can never work?
- Did you have to restructure main, or just add annotations?

## When you're done

Show me your `main.rs`.
