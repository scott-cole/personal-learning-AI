# DAY11: Checkpoint

**Closed book.** No looking back.

1. What does `<T>` mean in a function signature?
   the function is generic over type T

2. What is monomorphization?
   the compile-time process of generating concrete code for each type a generic is used with

3. How does Rust's generic implementation differ from Java's (in terms of runtime cost)?
   Rust monomorphizes at compile time with zero cost; Java uses boxed types at runtime

4. Use `impl<T> Foo<T> { }` vs `impl Foo<i32> { }` — what's the difference?
   first defines methods for all T, second defines methods only for Foo<i32>

5. What are some standard library types you've seen that use generics?
   Vec<T>, Option<T>, Result<T, E>, HashMap<K, V>

---

**Check your answers against the lesson after you finish.**
