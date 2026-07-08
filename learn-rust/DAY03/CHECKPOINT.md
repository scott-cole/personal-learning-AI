# DAY03: Checkpoint

**Closed book.** No looking back.

1. After `let s1 = String::from("a"); let s2 = s1;`, can you use `s1`? Why or why not?
   no, because ownership moved to s2

2. After `let x = 5; let y = x;`, can you use `x`? Why or why not?
   yes, because i32 implements Copy and is copied automatically

3. What does `.clone()` do?
   creates a deep copy so both variables own their own data

4. When does Rust free (drop) a value's memory?
   when the owner goes out of scope

5. You call `fn take(s: String) { }` with `take(my_string)`. After the call, is `my_string` usable? Why?
   no, because ownership moved into the function and was dropped when the function ended

---

**Check your answers against the lesson after you finish.**
