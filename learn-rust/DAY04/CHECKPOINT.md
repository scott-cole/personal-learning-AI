# DAY04: Checkpoint

**Closed book.** No looking back.

1. What does `&` do in front of a variable name?
   creates a reference to the variable without taking ownership

2. How many mutable references can exist at the same time?
   one

3. What happens if you try to use a reference after the owner has gone out of scope?
   the compiler rejects it with a borrow checker error

4. You have `let mut s = String::from("a"); let r1 = &s; let r2 = &mut s;`. Will this compile?
   no, because you can't have a shared and mutable reference at the same time

5. What is the borrow checker?
   the part of the Rust compiler that enforces the borrowing rules at compile time

---

**Check your answers against the lesson after you finish.**
