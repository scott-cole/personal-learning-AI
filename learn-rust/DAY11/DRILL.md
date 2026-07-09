# DAY11: Drill — "Generic Container"

Create `main.rs` in `DAY11/`.

## Requirements

Build a generic container that holds one value and provides basic operations.

```rust
// A generic container that holds a single value of any type
struct Container<T> {
    value: T,
}

impl<T> Container<T> {
    // TODO: create a new container
    fn new(value: T) -> Container<T> {
        // TODO
    }

    // TODO: return a reference to the value
    fn get(&self) -> &T {
        // TODO
    }

    // TODO: replace the value and return the old one
    fn replace(&mut self, new_value: T) -> T {
        // TODO
    }

    // TODO: consume the container and return the value
    fn into_inner(self) -> T {
        // TODO
    }
}

// Special implementation for numeric types
// Hint: what trait bounds do you need for >, +, etc.?
impl Container<i32> {
    fn is_positive(&self) -> bool {
        self.value > 0
    }
}

fn main() {
    let mut num = Container::new(42);
    println!("Value: {}", num.get());           // 42
    println!("Is positive: {}", num.is_positive());  // true

    let old = num.replace(100);
    println!("Old: {}, New: {}", old, num.get());  // Old: 42, New: 100

    let s = Container::new("hello".to_string());
    println!("String: {}", s.get());  // hello

    let extracted = s.into_inner();
    println!("Extracted: {}", extracted);  // hello
    // s is no longer valid here
}
```

## Example output

```
Value: 42
Is positive: true
Old: 42, New: 100
String: hello
Extracted: hello
```

## Hints

<details>
<summary>Click for hints</summary>

- `new` just returns `Container { value }`
- `get` returns `&self.value`
- `replace` uses `std::mem::replace(&mut self.value, new_value)`
- Or manually: `let old = self.value; self.value = new_value; old`
- `into_inner` returns `self.value` — this moves ownership out
</details>

## Bonus

Implement a generic `Pair<T, U>` struct with methods `first(&self) -> &T` and `second(&self) -> &U`.

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-rust/DAY11
rustc main.rs && ./main
```

## Before you ask for review

- Does it compile?
- Does `into_inner` consume the container correctly?
- Can you use `num` after calling `into_inner`? (Shouldn't compile)
- Why can `str` Container not call `is_positive`?

## When you're done

Show me your `main.rs`.
