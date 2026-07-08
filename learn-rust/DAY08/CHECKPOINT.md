# DAY08: Checkpoint

**Closed book.** No looking back.

1. What does `match` require that `if let` does not?
   match must be exhaustive (handle all variants)

2. What is `Option<T>` and why does it exist?
   it's an enum with Some(T) or None variants, used instead of null

3. What does `_ =>` do in a match expression?
   matches any value not caught by earlier arms (wildcard)

4. Given `enum Color { Rgb(i32,i32,i32), Hex(String) }`, write a match that extracts both variants.
   match c { Color::Rgb(r,g,b) => ..., Color::Hex(s) => ... }

5. What is `if let Some(x) = val` a shorthand for?
   match val { Some(x) => { }, None => () }

---

**Check your answers against the lesson after you finish.**
