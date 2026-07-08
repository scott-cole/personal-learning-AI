# DAY15: Drill — "Closure Factory"

Create `main.rs` in `DAY15/`.

## Requirements

Build a program that uses closures for filtering, transforming, and factory functions.

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

    // TODO: create a closure that returns true if a number is even
    let is_even = |n: &i32| -> bool { /* TODO */ };

    // TODO: create a closure that returns true if a number is greater than 5
    let greater_than_5 = |n: &i32| -> bool { /* TODO */ };

    // Filter using closures (simulate with for loop)
    let evens: Vec<i32> = numbers.iter().filter(|&n| is_even(n)).cloned().collect();
    println!("Evens: {:?}", evens);

    let big: Vec<i32> = numbers.iter().filter(|&n| greater_than_5(n)).cloned().collect();
    println!("Greater than 5: {:?}", big);

    // TODO: Write a function make_multiplier that takes an i32
    // and returns a closure that multiplies its input by that value
    let multiply_by_3 = make_multiplier(3);
    println!("6 * 3 = {}", multiply_by_3(6));
}

// TODO: implement make_multiplier using a move closure
fn make_multiplier(factor: i32) -> impl Fn(i32) -> i32 {
    // TODO
}
```

## Example output

```
Evens: [2, 4, 6, 8, 10]
Greater than 5: [6, 7, 8, 9, 10]
6 * 3 = 18
```

## Hints

<details>
<summary>Click for hints</summary>

- `n % 2 == 0` for even numbers
- `*n > 5` — remember `n` is `&i32`, so dereference with `*`
- `make_multiplier` returns `move |x| x * factor`
- The `.filter()` method on iterators takes a closure — but for this drill you can do it manually with a for loop
</details>

## Bonus

Write a function `compose` that takes two closures `f` and `g` and returns a new closure that applies `f(g(x))`.

## How to run

```bash
cd ~/Dev/learn-rust/DAY15
rustc main.rs && ./main
```

## Before you ask for review

- Does it compile?
- Did `make_multiplier` need `move`? What happens without it?
- Are the even numbers correct?
- What type does `make_multiplier(3)` return?

## When you're done

Show me your `main.rs`.
