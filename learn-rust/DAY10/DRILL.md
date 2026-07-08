# DAY10: Drill — "Word Frequency Counter"

Create `main.rs` in `DAY10/`.

## Requirements

Build a program that reads a hardcoded paragraph and prints a word frequency table.

```rust
use std::collections::HashMap;

fn main() {
    let text = "the quick brown fox jumps over the lazy dog the fox barks at the moon";
    
    // TODO: Split text into words and count frequency
    // Use a HashMap<String, i32> (or HashMap<&str, i32>)
    
    // TODO: Print results sorted by frequency (highest first)
    // Convert to Vec of tuples and sort
    
    // TODO: Also find the longest word in the text
}
```

## Example output

```
Word frequency:
  the:     4
  fox:     2
  quick:   1
  brown:   1
  jumps:   1
  over:    1
  lazy:    1
  dog:     1
  barks:   1
  at:      1
  moon:    1

Longest word: jumps (5 letters)
```

## Hints

<details>
<summary>Click for hints</summary>

- Use `text.split_whitespace()` to get words
- `counts.entry(word).or_insert(0)` and then `*count += 1`
- To sort: collect into `Vec<(&str, &i32)>` and use `.sort_by(|a, b| b.1.cmp(a.1))`
- For longest word: use `.max_by_key(|w| w.len())` on the split iterator
- Format with `"{:width$}"` to right-align — e.g. `println!("{:>10}", "hello")`
</details>

## Bonus

Filter out words shorter than 3 characters and see how the output changes.

## How to run

```bash
cd ~/Dev/learn-rust/DAY10
rustc main.rs && ./main
```

## Before you ask for review

- Does it compile?
- Are the frequencies correct?
- Is the output sorted by frequency?
- Did you use `entry()` or direct insert?

## When you're done

Show me your `main.rs`.
