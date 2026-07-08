# DAY15: Checkpoint

**Closed book.** No looking back.

1. What does the `||` syntax mean in Rust?
   closure parameter list — defines what arguments the closure takes

2. When would you use the `move` keyword before a closure?
   when the closure needs to take ownership of captured variables (e.g., spawning a thread or returning it)

3. Name the three closure traits from least to most restrictive.
   FnOnce, FnMut, Fn (or from most to least: FnOnce, FnMut, Fn)

4. What does `impl Fn(i32) -> i32` mean as a return type?
   returns a closure that takes i32, returns i32, and can be called multiple times

5. Given `let x = 5; let c = || x + 1; c();`, how does the closure capture `x`?
   by shared reference (default for reading)

---

**Check your answers against the lesson after you finish.**
