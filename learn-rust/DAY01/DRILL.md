# DAY01: Drill — "How old are you, really?"

Write a Rust program from scratch. Create a file called `main.rs` inside the `DAY01/` folder.

You can run it with:

```bash
rustc main.rs && ./main
```

## Requirements

```rust
fn main() {
    // 1. Store your name in a variable (use &str — just "Scott" in quotes)
    // 2. Store your age in a variable (use let with type i32)
    // 3. Calculate your approximate age in days (age * 365)
    // 4. Print: "Hello, I'm [name] and I'm about [days] days old!"
}
```

## Example output

```
Hello, I'm Scott and I'm about 10950 days old!
```

## Hints (try without these first)

<details>
<summary>Click for hints</summary>

- Use `let` for all your variables
- Your age in days is `age * 365`
- Use `println!` with `{}` placeholders
- `println!("Hello, I'm {} and I'm about {} days old!", name, days);`
</details>

## Bonus (if you finish early)

Add a second print that shows your age in **hours**:

```
I'm approximately 262800 hours old!
```

## How to run

```bash
cd ~/Dev/learn-rust/DAY01
rustc main.rs && ./main
```

## Before you ask for review

- Does it compile? No errors from `rustc`
- Does the output match the example?
- Try changing the age — does it still work?

## When you're done

Show me your `main.rs` file. I'll review every line.
