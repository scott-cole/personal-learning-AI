# DAY02: Drill — "Temperature Converter"

Create a file called `main.rs` inside `DAY02/`.

## Requirements

```rust
fn main() {
    // 1. Call celsius_to_fahrenheit with 100.0 and print the result
    // 2. Call fahrenheit_to_celsius with 212.0 and print the result
    // 3. Call describe_temp for both temperatures
}

// Write a function that converts Celsius to Fahrenheit
// Formula: (c * 9.0 / 5.0) + 32.0
fn celsius_to_fahrenheit(c: f64) -> f64 {
    // TODO
}

// Write a function that converts Fahrenheit to Celsius
// Formula: (f - 32.0) * 5.0 / 9.0
fn fahrenheit_to_celsius(f: f64) -> f64 {
    // TODO
}

// Write a function that takes a celsius temp and prints:
// "[temp]°C is [description]"
// where description is:
//   "freezing" if temp <= 0
//   "cool" if temp < 15
//   "warm" if temp < 30
//   "hot" otherwise
fn describe_temp(c: f64) {
    // TODO
}
```

## Example output

```
100°C = 212°F
212°F = 100°C
100°C is hot
0°C is freezing
```

## Hints

<details>
<summary>Click for hints</summary>

- Use `f64` for all temperatures
- `if/else if/else` chains work just like other languages
- Use `println!("{}°C = {}°F", c, f);` to print the degree symbol
- Remember: the last expression without semicolon is the return value
</details>

## Bonus

Add a function `celsius_to_kelvin(c: f64) -> f64` (formula: `c + 273.15`) and print all three scales.

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-rust/DAY02
rustc main.rs && ./main
```

## Before you ask for review

- Does it compile?
- Do both conversions round-trip? (100C → 212F → should be 100C again)
- Did you forget semicolons in the right places?

## When you're done

Show me your `main.rs` file.
