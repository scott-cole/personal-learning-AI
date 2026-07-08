# DAY13: Checkpoint

**Closed book.** No looking back.

1. What does `'a` represent in a function signature?
   a lifetime parameter tying the validity of references together

2. What is lifetime elision?
   Rust's rules that let you omit explicit lifetime annotations in common patterns

3. Write the lifetime annotation for a function that takes two `&str` and returns the longer one.
   fn longest<'a>(x: &'a str, y: &'a str) -> &'a str

4. What does `'static` mean?
   the reference is valid for the entire program duration

5. Why does `fn invalid() -> &str { let s = String::from("hi"); &s }` not compile?
   because s is dropped when the function ends, but the return reference would outlive it

---

**Check your answers against the lesson after you finish.**
