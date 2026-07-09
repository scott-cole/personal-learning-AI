# DAY16: Drill — "Pipeline Processing"

Create `main.rs` in `DAY16/`.

## Requirements

Process a list of integers through an iterator pipeline.

```rust
fn main() {
    let data = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15];

    // 1. Use an iterator pipeline to:
    //    - Filter to only odd numbers
    //    - Multiply each by 3
    //    - Keep only those less than 30
    //    - Collect into Vec<i32>
    let result: Vec<i32> = data.iter()
        // TODO
        .collect();
    println!("Pipeline result: {:?}", result);

    // 2. Calculate the sum of squares of even numbers
    let sum_of_squares: i32 = data.iter()
        // TODO: filter evens, map to square, sum
        ;
    println!("Sum of squares of evens: {}", sum_of_squares);

    // 3. Find the first number greater than 10
    let first_big = data.iter()
        // TODO
        ;
    println!("First number > 10: {:?}", first_big);

    // 4. Check if ALL numbers are positive
    let all_positive = data.iter()
        // TODO
        ;
    println!("All positive: {}", all_positive);

    // 5. Use fold to calculate factorial of 10
    let factorial: i32 = (1..=10)
        // TODO: use fold to multiply all numbers
        ;
    println!("10! = {}", factorial);
}
```

## Example output

```
Pipeline result: [3, 9, 15, 21, 27]
Sum of squares of evens: 220
First number > 10: Some(11)
All positive: true
10! = 3628800
```

## Hints

<details>
<summary>Click for hints</summary>

- `.filter(|&&x| x % 2 != 0)` — note the double reference
- `.map(|x| x * 3)`
- `.filter(|&&x| *x < 30)` or `x < 30`
- `.sum()` consumes the iterator
- `.find(|&&x| x > 10)` returns `Option<&i32>`
- `.all(|&x| x > 0)`
- `.fold(1, |acc, x| acc * x)`
- `(1..=10)` creates a range inclusive iterator
</details>

## Bonus

Calculate the sum of all numbers in the Fibonacci sequence under 100 using `std::iter::successors`.

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-rust/DAY16
rustc main.rs && ./main
```

## Before you ask for review

- Does it compile?
- Are the pipeline results correct?
- Could you write the pipeline as a single chain without intermediate variables?
- Did `sum_of_squares` use `.sum()` or `.fold()`?

## When you're done

Show me your `main.rs`.
