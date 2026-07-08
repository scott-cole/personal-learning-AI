# DAY09: Checkpoint

**Closed book.** No looking back.

1. What are the two variants of `Result<T, E>`?
   Ok(T) and Err(E)

2. What does the `?` operator do when used on a `Result`?
   if Ok, unwraps the value; if Err, returns early from the function

3. What is the difference between `unwrap()` and `expect()`?
   expect takes a custom error message, unwrap uses a default message

4. Your function calls `something()` that returns `Result<i32, Error>` but the function returns `i32`. Can you use `?`?
   no, because ? requires the function to return Result or Option

5. What does `.ok()` do when called on a `Result<T, E>`?
   converts Result to Option, mapping Ok(T) to Some(T) and Err(_) to None

---

**Check your answers against the lesson after you finish.**
