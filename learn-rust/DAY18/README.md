# DAY18: Testing (#[test], assert!, Test Organization)

---

## The anecdote: "Tests are a fire drill"

Schools run fire drills so that when a real fire happens, nobody panics. Everyone knows the procedure, the exits, the meeting point.

Tests in Rust are the same — they verify that your code behaves correctly under controlled conditions so that when it's in production (the "real fire"), you can trust it.

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[test]
fn test_add() {
    assert_eq!(add(2, 2), 4);  // Like a drill — we know the expected outcome
}
```

---

## Writing tests

Create a library crate:

```bash
cargo new --lib my_lib
```

Or add tests to an existing project.

```rust
// src/lib.rs
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_add() {
        assert_eq!(add(2, 2), 4);
    }

    #[test]
    fn test_add_negative() {
        assert_eq!(add(-1, 1), 0);
    }
}
```

---

## Common assertions

```rust
#[test]
fn test_assertions() {
    assert!(true);                         // Must be true
    assert_eq!(5, 5);                      // Equality
    assert_ne!(5, 6);                      // Not equal
    assert!(result.is_ok());               // On Results
    assert!(result.is_err());              // On Results
    assert_eq!(result.unwrap(), expected); // Unwrap in tests is OK!
}
```

---

## Custom failure messages

```rust
#[test]
fn test_with_message() {
    let result = add(2, 2);
    assert!(
        result == 4,
        "Expected 4 but got {}",
        result
    );
}
```

---

## Test organization

```rust
// Unit tests — in the same file as the code
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn unit_test() { }
}

// Integration tests — in tests/ directory
// tests/integration_test.rs
use my_lib;

#[test]
fn integration_test() {
    // Tests that use your library as an external consumer
}
```

---

## Running tests

```bash
cargo test              # Run all tests
cargo test test_name    # Run specific test
cargo test -- --nocapture  # Show println! output
cargo test -- --ignored     # Run ignored tests
```

---

## should_panic

```rust
#[test]
#[should_panic(expected = "divide by zero")]
fn test_divide_by_zero() {
    divide(10, 0);  // This should panic
}
```

---

## Summary

| Concept | Syntax | Purpose |
|---------|--------|---------|
| Test function | `#[test]` | Marks a function as a test |
| Assert true | `assert!(expr)` | Expression must be true |
| Assert equals | `assert_eq!(a, b)` | a must equal b |
| Assert not equals | `assert_ne!(a, b)` | a must not equal b |
| Expected panic | `#[should_panic]` | Test must panic |
| Test module | `#[cfg(test)] mod tests { }` | Test-only code |
| Run tests | `cargo test` | Execute all tests |

Your turn. Open DAY18/DRILL.md.
