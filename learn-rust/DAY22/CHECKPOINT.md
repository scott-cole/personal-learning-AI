# DAY22: Checkpoint

**Closed book.** No looking back.

1. What function reads an entire file into a String?
   fs::read_to_string()

2. What function writes a string to a file (overwriting)?
   fs::write()

3. How do you append to a file in Rust?
   OpenOptions::new().append(true).open(filename)

4. What does `path.exists()` return?
   a bool indicating whether the path exists

5. What does `fs::create_dir_all("a/b/c")` do that `fs::create_dir("a")` does not?
   creates intermediate directories recursively

---

**Check your answers against the lesson after you finish.**
