# DAY04: Drill — "The Word Counter"

Create `main.rs` in `DAY04/`.

## Requirements

Write a program that:
1. Takes a `String` and passes it to a function that **counts** the words (using a shared reference)
2. Then passes the **same** string to a function that **adds** a word to it (using a mutable reference)
3. Then prints the final string

```rust
fn count_words(s: &String) -> usize {
    // TODO: count the words (split by whitespace, count them)
    // Use s.split_whitespace() and .count()
}

fn add_word(s: &mut String, word: &str) {
    // TODO: add a space and the word to the string
    // Use s.push_str()
}

fn main() {
    let mut text = String::from("hello world");
    
    // TODO: call count_words with a shared reference
    let count = ?????;
    println!("Word count: {}", count);
    
    // TODO: call add_word with a mutable reference
    ?????;
    
    // TODO: print the final string
    println!("Final: {}", text);
}
```

## Example output

```
Word count: 2
Final: hello world rust
```

## Hints

<details>
<summary>Click for hints</summary>

- Shared reference: `&text`
- Mutable reference: `&mut text`
- `split_whitespace()` returns an iterator — call `.count()` on it
- `push_str()` adds a string slice to the end
- `.split_whitespace().count()` is all you need for counting
</details>

## Bonus

Write a function that takes a **mutable reference** and converts it to uppercase using `s.make_ascii_uppercase()`. Call it before printing.

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-rust/DAY04
rustc main.rs && ./main
```

## Before you ask for review

- Does it compile?
- Does the count match the number of words?
- Can you borrow immutably and mutably in the right order?
- Try passing `&mut text` before the `count_words` call — does it compile?

## When you're done

Show me your `main.rs`.
