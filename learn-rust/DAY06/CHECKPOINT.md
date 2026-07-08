# DAY06: Checkpoint

**Closed book.** No looking back.

1. What syntax defines a struct?
   struct Name { field: Type }

2. What is `impl` used for?
   to define methods and associated functions for a struct

3. What's the difference between a method and an associated function?
   a method takes &self as first parameter and is called with dot notation; an associated function does not and is called with ::

4. What does `&self` mean in a method signature?
   it's short for self: &Self — a shared reference to the struct instance

5. You write `User { name, email }` instead of `User { name: name, email: email }`. What is this called?
   field init shorthand

---

**Check your answers against the lesson after you finish.**
