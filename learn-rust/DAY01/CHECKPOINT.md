# DAY01: Checkpoint

**Closed book.** No looking back at the lesson. Answer in your own words.

1. What does `mut` do in `let mut x = 5;`?
   x mutable

2. What would `println!("{} + {} = {}", 2, 3, 2 + 3);` print?
   2 + 3 = 5

3. Your program has a bug: `rustc main.rs` gives `"cannot assign to immutable variable"`. What's wrong?
   trying to change a variable that wasn't declared with mut

4. What is the difference between `let x = 5;` followed by `let x = x + 1;` vs just changing `x`?
   shadowing creates a new variable, not mutation

5. Why would a senior engineer NEVER use `f64` to store a monetary value in production?
   floats are approximations and can give wrong results with rounding

---

**Check your answers against the lesson after you finish.**
