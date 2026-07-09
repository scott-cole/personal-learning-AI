# DAY03: Drill — "Ownership Puzzle"

Create `main.rs` in `DAY03/`.

## Requirements

Fix this program. It has 5 ownership bugs. Your job is to make it compile and produce the correct output.

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;
    println!("s1: {}", s1);  // BUG 1 — s1 was moved

    let x = 5;
    let y = x;
    println!("x: {}, y: {}", x, y);  // BUG 2 — maybe not a bug?

    let s3 = String::from("world");
    take_string(s3);
    println!("s3: {}", s3);  // BUG 3 — s3 was moved

    let result = give_string();
    println!("result: {}", result);  // BUG 4 — what's wrong here?

    let s4 = String::from("copy me");
    let s5 = s4;  // I want to keep using s4 too!
    println!("s4: {}, s5: {}", s4, s5);  // BUG 5
}

fn take_string(s: String) {
    println!("got: {}", s);
}

fn give_string() -> String {
    let s = String::from("given");
    // BUG 4 is here — one character missing
}
```

## Expected output

```
x: 5, y: 5
got: world
result: given
copy me: copy me, copy me: copy me
```

Note: The program should compile cleanly with no warnings about unused variables.

## Hints

<details>
<summary>Click for hints</summary>

- BUG 1: Fix by not using s1 after the move, or use `.clone()`
- BUG 2: Actually, `i32` is `Copy` — no bug here! Remove the comment or adjust
- BUG 3: Return the string from `take_string`, or pass a reference (you haven't learned that yet — try `.clone()` or return it)
- BUG 4: `give_string` is missing the return — add `s` (no semicolon) at the end
- BUG 5: Use `.clone()` when assigning to s5
</details>

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-rust/DAY03
rustc main.rs && ./main
```

## Before you ask for review

- Does it compile?
- Does the output match exactly?
- Did you fix all 5 bugs?
- Remove any unused variable warnings

## When you're done

Show me your fixed `main.rs`.
