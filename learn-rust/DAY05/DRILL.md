# DAY05: Drill — "The Log Parser"

Create `main.rs` in `DAY05/`.

## Requirements

Write a program that parses a log line and extracts pieces of it using slices.

```rust
fn main() {
    let log = "ERROR 2024-01-15: Disk space low on /dev/sda1";
    
    // TODO: Extract the log level (first word before space)
    let level = &log[..5];
    
    // TODO: Extract the date (characters 6 to 16)
    let date = ?????;
    
    // TODO: Extract the message (everything after ": ")
    let message = ?????;
    
    println!("Level: {}", level);
    println!("Date: {}", date);
    println!("Message: {}", message);
    
    // TODO: Write a function first_token(s: &str) -> &str
    // that returns everything before the first space
    let first = first_token(log);
    println!("First token: {}", first);
}
```

## Example output

```
Level: ERROR
Date: 2024-01-15
Message: Disk space low on /dev/sda1
First token: ERROR
```

## Hints

<details>
<summary>Click for hints</summary>

- `"ERROR"` is 5 characters — `&log[..5]`
- The date starts at index 6 (after the space) — count carefully
- `&log[start..end]` where end is `start + 10` for the date
- For the message, find `": "` in the string — try `log.find(": ")` which returns an `Option<usize>`
- `first_token` can use `s.find(' ')` and return a slice
</details>

## Bonus

Write a function `last_token(s: &str) -> &str` that returns everything after the last space.

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-rust/DAY05
rustc main.rs && ./main
```

## Before you ask for review

- Does it compile?
- Are all slices within bounds?
- Did your `first_token` function work with both `&str` and `&String`?
- What happens if the log format changes?

## When you're done

Show me your `main.rs`.
