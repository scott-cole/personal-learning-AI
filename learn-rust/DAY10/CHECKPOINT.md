# DAY10: Checkpoint

**Closed book.** No looking back.

1. What macro creates a Vec with values: `vec![1, 2, 3]`?
   vec!

2. What does `v.get(10)` return if `v` has 3 elements?
   None

3. What is the difference between `String` and `&str`?
   String is owned and growable; &str is a borrowed immutable view

4. How do you insert a value into a HashMap only if the key doesn't already exist?
   entry(key).or_insert(value)

5. In `HashMap`, what type does `.get(&key)` return?
   Option<&V>

---

**Check your answers against the lesson after you finish.**
