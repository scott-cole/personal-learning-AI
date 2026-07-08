# DAY10: Vectors, String vs &str, HashMaps

---

## The anecdote: "A Vec is an expandable suitcase; HashMap is a coat check"

Imagine packing for a trip. You have a suitcase that can expand when you add more clothes. It keeps everything in order, and you can access any item by its position. That's a **Vec**.

Now imagine a coat check at a restaurant. You hand over your coat, they give you a numbered ticket. Later, you hand back the ticket, and they return your coat. That's a **HashMap** — you store a value with a key, then look it up with that key.

---

## Vec<T> — The expandable suitcase

```rust
let mut v: Vec<i32> = Vec::new();
v.push(1);
v.push(2);
v.push(3);

// Or with the vec! macro:
let v = vec![1, 2, 3];
```

### Accessing elements

```rust
let v = vec![10, 20, 30];

// By index (panics if out of bounds):
let second = v[1];  // 20

// Safely with get (returns Option):
let second = v.get(1);  // Some(&20)
let tenth = v.get(9);   // None
```

### Iterating

```rust
for val in &v {
    println!("{}", val);
}

// Mutable iteration:
for val in &mut v {
    *val *= 2;
}
```

---

## String vs &str — The Kitchen Analogy

- **`String`** is a catering kitchen — you own it, you can modify it, you can add ingredients.
- **`&str`** is a plate of food served to you — you can eat it (read it), but you can't change it.

```rust
// String — owned, growable, mutable
let mut s = String::from("hello");
s.push_str(" world");  // ✅ modify

// &str — borrowed, fixed
let t: &str = "hello";
// t.push_str(" world");  // ❌ can't modify

// &String converts to &str automatically (deref coercion)
fn takes_str(s: &str) { }
let s = String::from("hello");
takes_str(&s);  // ✅ &String → &str automatically
```

---

## HashMap<K, V> — The coat check

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Red"), 20);

// Access
let score = scores.get("Blue");  // Option<&i32> — Some(&10) or None

// Iterate
for (key, value) in &scores {
    println!("{}: {}", key, value);
}

// Insert only if key doesn't exist
scores.entry(String::from("Blue")).or_insert(50);
```

---

## Common patterns

```rust
// Count words using a HashMap
let text = "hello world hello";
let mut counts = HashMap::new();

for word in text.split_whitespace() {
    let count = counts.entry(word).or_insert(0);
    *count += 1;
}
println!("{:?}", counts);  // {"hello": 2, "world": 1}
```

---

## Summary

| Type | Owned? | Mutable? | Description |
|------|--------|----------|-------------|
| `Vec<T>` | Yes | Yes | Growable array |
| `String` | Yes | Yes | Growable UTF-8 text |
| `&str` | No | No | Borrowed string view |
| `HashMap<K,V>` | Yes | Yes | Key-value store |

Your turn. Open DAY10/DRILL.md.
