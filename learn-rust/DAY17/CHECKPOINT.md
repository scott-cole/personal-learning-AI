# DAY17: Checkpoint

**Closed book.** No looking back.

1. What keyword makes an item accessible outside its module?
   pub

2. If you have `src/kitchen.rs`, how do you declare it as a module in `src/main.rs`?
   mod kitchen;

3. What does `use std::collections::HashMap;` do?
   brings HashMap into the current scope so you can use it without the full path

4. What is the difference between a crate and a module?
   a crate is a library or binary (compilation unit); a module is a namespace within a crate

5. Write the syntax to import both `HashMap` and `VecDeque` from `std::collections` in one use statement.
   use std::collections::{HashMap, VecDeque};

---

**Check your answers against the lesson after you finish.**
