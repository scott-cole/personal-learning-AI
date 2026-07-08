# DAY12: Traits

---

## The anecdote: "Traits are a driver's license — proves you can drive any car"

Think about getting a driver's license. The DMV tests you on one specific car, but the license proves you can drive **any** car (that has the same controls — no stick shift jokes, please).

A trait in Rust is exactly that: it defines a set of behaviors (methods) that a type must implement. Once a type implements a trait, you can call those methods on it, just like a license lets you drive any car.

```rust
trait Drive {
    fn start(&self) -> String;
    fn stop(&self) -> String;
}

struct Car {
    make: String,
}

impl Drive for Car {
    fn start(&self) -> String {
        format!("{} engine roars to life", self.make)
    }
    fn stop(&self) -> String {
        format!("{} brakes squeak", self.make)
    }
}
```

---

## Defining a trait

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}
```

This says: "Any type that implements `Summary` must have a method `summarize` that takes `&self` and returns a `String`."

---

## Implementing a trait

```rust
struct Article {
    headline: String,
    content: String,
}

impl Summary for Article {
    fn summarize(&self) -> String {
        format!("{}...", &self.headline)
    }
}

struct Tweet {
    text: String,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{} (via Twitter)", &self.text)
    }
}
```

---

## Default implementations

```rust
trait Summary {
    fn summarize(&self) -> String {
        String::from("(Read more...)")  // default
    }
}

// Types can use the default or override it
impl Summary for Article {}  // uses default
```

---

## Traits as parameters

```rust
fn notify(item: &impl Summary) {
    println!("Breaking: {}", item.summarize());
}
```

This says: "`item` can be any type as long as it implements `Summary`."

The longer form (trait bound) is:

```rust
fn notify<T: Summary>(item: &T) {
    println!("Breaking: {}", item.summarize());
}
```

---

## Multiple trait bounds

```rust
fn some_function(item: &(impl Summary + Display)) { }

// Or with generics:
fn some_function<T: Summary + Display>(item: &T) { }
```

---

## The where clause

For complex bounds, `where` is more readable:

```rust
fn some_function<T, U>(t: &T, u: &U)
where
    T: Summary + Clone,
    U: Display + Debug,
{
}
```

---

## Return traits

```rust
fn returns_summarizable() -> impl Summary {
    Tweet {
        text: String::from("hello world"),
    }
}
```

Note: you can only return a single concrete type this way.

---

## Summary

| Concept | Syntax |
|---------|--------|
| Define trait | `trait Name { fn method(&self); }` |
| Implement trait | `impl Name for Type { }` |
| Default method | `fn method(&self) { default }` |
| Trait parameter | `fn f(item: &impl Trait)` |
| Trait bound | `fn f<T: Trait>(item: &T)` |
| Multiple bounds | `fn f(item: &(impl A + B))` |
| Where clause | `fn f<T>(t: &T) where T: Trait` |

Your turn. Open DAY12/DRILL.md.
