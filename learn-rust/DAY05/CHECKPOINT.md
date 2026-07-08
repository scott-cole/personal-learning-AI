# DAY05: Checkpoint

**Closed book.** No looking back.

1. What is a `&str`?
   a string slice, an immutable view into a string

2. Given `let s = String::from("abcdef");`, what does `&s[1..4]` evaluate to?
   "bcd"

3. What is the difference between `String` and `&str`?
   String owns its data on the heap, &str is a borrowed view

4. If you try to create a slice `&s[start..end]` where `end` is past the length of the string, what happens?
   the program panics at runtime with an out of bounds error

5. What two pieces of data does a slice pointer store?
   a pointer to the start and a length

---

**Check your answers against the lesson after you finish.**
