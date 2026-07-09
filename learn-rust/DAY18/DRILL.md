# DAY18: Drill — "Test-Driven Calculator"

Create a new library crate:

```bash
cargo new --lib calculator
cd calculator
```

## Requirements

Write a calculator library with these functions, then write tests for ALL of them (write the tests first if you want — try TDD!).

### src/lib.rs

```rust
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

pub fn subtract(a: i32, b: i32) -> i32 {
    a - b
}

pub fn multiply(a: i32, b: i32) -> i32 {
    a * b
}

pub fn divide(a: i32, b: i32) -> Result<i32, String> {
    if b == 0 {
        Err(String::from("division by zero"))
    } else {
        Ok(a / b)
    }
}

pub fn factorial(n: u32) -> u32 {
    // TODO: implement factorial (n!)
    // Hint: use match or a loop
    // 0! = 1, 1! = 1, 5! = 120
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_add() {
        assert_eq!(add(2, 3), 5);
        assert_eq!(add(-1, 1), 0);
        assert_eq!(add(0, 0), 0);
    }

    #[test]
    fn test_subtract() {
        // TODO: test subtract
    }

    #[test]
    fn test_multiply() {
        // TODO: test multiply
    }

    #[test]
    fn test_divide() {
        // TODO: test normal division
        // TODO: test division by zero returns Err
    }

    #[test]
    fn test_factorial() {
        // TODO: test 0! = 1, 1! = 1, 5! = 120
    }

    #[test]
    #[should_panic]
    fn test_factorial_panic() {
        // TODO: what input would make factorial panic?
        // Think about u32 overflow...
    }
}
```

## Expected test results

```
running 6 tests
test tests::test_add ... ok
test tests::test_subtract ... ok
test tests::test_multiply ... ok
test tests::test_divide ... ok
test tests::test_factorial ... ok
test tests::test_factorial_panic ... ok

test result: ok. 6 passed; 0 failed; 0 ignored; 0 measured
```

## Hints

<details>
<summary>Click for hints</summary>

- Implement factorial with recursion: `if n <= 1 { 1 } else { n * factorial(n - 1) }`
- Or with a range: `(1..=n).product()`
- `test_factorial_panic`: call `factorial(100)` which overflows u32
- For division by zero: `assert!(divide(10, 0).is_err())`
</details>

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-rust/DAY18/calculator
cargo test
```

## Before you ask for review

- Do all 6 tests pass?
- Did you test edge cases (zero, negative, overflow)?
- Does `cargo test` show a clean output with no failures?
- Try adding a test that fails — what does the output look like?

## When you're done

Show me your `src/lib.rs` and the `cargo test` output.
