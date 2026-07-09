# DAY12: Drill — "Shape Traits"

Create `main.rs` in `DAY12/`.

## Requirements

Define and implement traits for geometric shapes.

```rust
trait Shape {
    fn area(&self) -> f64;
    fn perimeter(&self) -> f64;
    fn description(&self) -> String {
        // Default implementation
        format!("Area: {:.2}, Perimeter: {:.2}", self.area(), self.perimeter())
    }
}

struct Circle {
    radius: f64,
}

struct Rectangle {
    width: f64,
    height: f64,
}

// TODO: implement Shape for Circle
// TODO: implement Shape for Rectangle

// TODO: Write a function print_shape_info that takes any type implementing Shape
fn print_shape_info(shape: &impl Shape) {
    println!("{}", shape.description());
}

// TODO: Write a generic function total_area that takes a slice of Shapes
// and returns the sum of their areas
fn total_area(shapes: &[impl Shape]) -> f64 {
    // TODO
}

fn main() {
    let circle = Circle { radius: 5.0 };
    let rect = Rectangle { width: 4.0, height: 6.0 };

    print_shape_info(&circle);
    print_shape_info(&rect);

    let shapes = vec![circle, rect];  // Wait, this won't work — why?
    // println!("Total area: {:.2}", total_area(&shapes));
}
```

## Example output

```
Area: 78.54, Perimeter: 31.42
Area: 24.00, Perimeter: 20.00
```

## Hints

<details>
<summary>Click for hints</summary>

- Circle area: `std::f64::consts::PI * self.radius * self.radius`
- Circle perimeter: `2.0 * std::f64::consts::PI * self.radius`
- Rectangle area: `self.width * self.height`
- Rectangle perimeter: `2.0 * (self.width + self.height)`
- For `total_area`, iterate and sum `shape.area()`
- The `vec![circle, rect]` issue: different types can't be in the same Vec directly. Use a trait object `Box<dyn Shape>` or just comment that out
</details>

## Bonus

Implement a `Display` trait for both shapes using `impl fmt::Display for Circle { }`.

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-rust/DAY12
rustc main.rs && ./main
```

## Before you ask for review

- Does it compile?
- Did you implement both required methods for both types?
- Can `print_shape_info` accept both `Circle` and `Rectangle`?
- What error do you get with `vec![circle, rect]`? Can you explain it?

## When you're done

Show me your `main.rs`.
