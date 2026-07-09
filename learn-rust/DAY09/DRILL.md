# DAY09: Drill — "Safe Calculator"

Create `main.rs` in `DAY09/`.

## Requirements

Build a calculator that handles errors gracefully using `Result` and `Option`.

```rust
// TODO: Write a safe divide function that returns Result
// Return Err if dividing by zero
fn divide(a: f64, b: f64) -> Result<f64, String> {
    // TODO
}

// TODO: Write a function that parses a string to f64 and returns Option
fn parse_number(s: &str) -> Option<f64> {
    // TODO
}

// TODO: Write a function that takes two strings, parses them,
// divides them, and returns the result using the ? operator
fn divide_strings(a: &str, b: &str) -> Result<f64, String> {
    // TODO
}

fn main() {
    // Test divide
    println!("10 / 2 = {:?}", divide(10.0, 2.0));
    println!("10 / 0 = {:?}", divide(10.0, 0.0));

    // Test parse_number
    println!("parse '42': {:?}", parse_number("42"));
    println!("parse 'abc': {:?}", parse_number("abc"));

    // Test divide_strings
    match divide_strings("15", "3") {
        Ok(result) => println!("15 / 3 = {}", result),
        Err(e) => println!("Error: {}", e),
    }
    match divide_strings("15", "0") {
        Ok(result) => println!("15 / 0 = {}", result),
        Err(e) => println!("Error: {}", e),
    }
    match divide_strings("abc", "3") {
        Ok(result) => println!("abc / 3 = {}", result),
        Err(e) => println!("Error: {}", e),
    }
}
```

## Example output

```
10 / 2 = Ok(5)
10 / 0 = Err("Cannot divide by zero")
parse '42': Some(42)
parse 'abc': None
15 / 3 = 5
Error: Cannot divide by zero
Error: Failed to parse 'abc' as number
```

## Hints

<details>
<summary>Click for hints</summary>

- `divide`: `if b == 0.0 { Err(...) } else { Ok(a / b) }`
- `parse_number`: `s.parse::<f64>().ok()` (converts Result to Option)
- `divide_strings`: use `?` on both parse and divide
- Parse returns `Result<f64, ParseFloatError>` — use `.map_err()` to convert to String
- `.map_err(|_| format!("Failed to parse '{}' as number", a))`
</details>

## Bonus

Add a `sqrt` function that returns `Option<f64>` (returns `None` for negative numbers).

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-rust/DAY09
rustc main.rs && ./main
```

## Before you ask for review

- Does it compile?
- Does divide by zero return an error instead of crashing?
- Does parsing "abc" return None instead of crashing?
- Did you use `?` in `divide_strings`?

## When you're done

Show me your `main.rs`.
