# DAY07: Checkpoint

**Closed book.** No looking back.

1. What method signature would you use for a function that adds an item to a todo list?
   fn add(&mut self, title: &str)

2. Why does `list` take `&self` while `add` takes `&mut self`?
   list only reads data, add modifies it

3. What method on `Vec` can remove items that match a condition?
   retain

4. What does `for item in &self.items { }` borrow?
   a shared reference to the items vector, so we can read each item

5. Why might `complete(99)` not crash even though no item with ID 99 exists?
   because iter_mut().find() returns None, and doing nothing on None is handled gracefully

---

**Check your answers against the lesson after you finish.**
