# DAY08: Drill — "Shape Calculator"

Create `main.rs` in `DAY08/`.

## Requirements

```rust
enum Shape {
    Circle(f64),      // radius
    Rectangle(f64, f64), // width, height
    Square(f64),      // side length
}

impl Shape {
    fn area(&self) -> f64 {
        // TODO: calculate area for each variant
        // Circle: π * r^2
        // Rectangle: w * h
        // Square: side * side
    }

    fn perimeter(&self) -> f64 {
        // TODO: calculate perimeter for each variant
        // Circle: 2 * π * r
        // Rectangle: 2 * (w + h)
        // Square: 4 * side
    }
}

fn main() {
    let shapes = vec![
        Shape::Circle(5.0),
        Shape::Rectangle(4.0, 6.0),
        Shape::Square(3.0),
    ];

    for shape in &shapes {
        // TODO: print each shape with its area and perimeter
        // Use match to describe the shape type
    }
}
```

## Example output

```
Circle(r=5.0) — area: 78.54, perimeter: 31.42
Rectangle(4.0x6.0) — area: 24.00, perimeter: 20.00
Square(3.0) — area: 9.00, perimeter: 12.00
```

## Hints

<details>
<summary>Click for hints</summary>

- `std::f64::consts::PI` gives you π
- Format to 2 decimal places with `{:.2}`
- Use `match shape { Shape::Circle(r) => ..., ... }` inside the loop
- For the description, use nested match or handle it in the println
</details>

## Bonus

Add a `Triangle(f64, f64, f64)` variant (three sides) and implement area using Heron's formula.

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-rust/DAY08
rustc main.rs && ./main
```

## Before you ask for review

- Does it compile?
- Are the areas and perimeters correct?
- Did you handle all variants in every match?
- Try adding a new shape — does the compiler tell you what to update?

## When you're done

Show me your `main.rs`.
