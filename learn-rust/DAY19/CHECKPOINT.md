# DAY19: Checkpoint

**Closed book.** No looking back.

1. What does `Box<T>` do?
   allocates T on the heap and stores a pointer to it

2. When would you use `Rc<T>` instead of a regular reference?
   when you need multiple ownership and the data is single-threaded

3. What does `Rc::clone(&rc)` do that's different from a regular `.clone()`?
   only increments the reference count, doesn't deep-copy the data

4. What is interior mutability?
   the ability to mutate data through a shared (immutable) reference

5. What is the risk of using `RefCell<T>` incorrectly?
   it panics at runtime instead of catching the error at compile time

---

**Check your answers against the lesson after you finish.**
