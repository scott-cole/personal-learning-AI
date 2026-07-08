# DAY16: Iterators (iter, map, filter, collect)

---

## The anecdote: "Iterators are a conveyor belt"

Imagine a factory conveyor belt. Items move along the belt one at a time. You can:
- **Look at each item** as it passes (`.iter()`)
- **Transform each item** (`.map()`) — like a paint station
- **Decide which items to keep** (`.filter()`) — like a quality control station
- **Package the results** (`.collect()`)

Rust iterators work the same way — they process sequences lazily, one element at a time, until you "collect" them.

---

## Creating iterators

```rust
let numbers = vec![1, 2, 3];

let iter = numbers.iter();     // iterates over &i32
let iter = numbers.iter_mut(); // iterates over &mut i32
let iter = numbers.into_iter(); // iterates over i32 (consumes vec)
```

---

## The Iterator trait

```rust
trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
}
```

Every iterator has a `next()` method. The `for` loop is syntactic sugar for calling `next()` repeatedly.

---

## map — transform each element

```rust
let numbers = vec![1, 2, 3, 4, 5];
let doubled: Vec<i32> = numbers.iter()
    .map(|x| x * 2)
    .collect();

println!("{:?}", doubled);  // [2, 4, 6, 8, 10]
```

---

## filter — keep elements that match

```rust
let numbers = vec![1, 2, 3, 4, 5, 6];
let evens: Vec<i32> = numbers.iter()
    .filter(|&x| x % 2 == 0)
    .cloned()
    .collect();

println!("{:?}", evens);  // [2, 4, 6]
```

---

## Chaining iterators

The power of iterators is chaining:

```rust
let numbers = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

let result: Vec<i32> = numbers.iter()
    .filter(|&x| x % 2 == 0)     // keep evens
    .map(|x| x * 10)              // multiply by 10
    .collect();

println!("{:?}", result);  // [20, 40, 60, 80, 100]
```

---

## Other useful iterator methods

```rust
let numbers = vec![1, 2, 3, 4, 5];

// sum, product
let sum: i32 = numbers.iter().sum();
let product: i32 = numbers.iter().product();

// any, all
let has_even = numbers.iter().any(|&x| x % 2 == 0);  // true
let all_positive = numbers.iter().all(|&x| x > 0);     // true

// find
let first_even = numbers.iter().find(|&&x| x % 2 == 0);  // Some(&2)

// max, min
let max = numbers.iter().max();  // Some(&5)

// fold (reduce)
let sum = numbers.iter().fold(0, |acc, x| acc + x);
```

---

## Lazy evaluation

Iterators are **lazy** — nothing happens until you call `collect()` or another consuming method:

```rust
let numbers = vec![1, 2, 3];
let iter = numbers.iter().map(|x| {
    println!("Processing {}", x);
    x * 2
});
// Nothing printed yet!
// let result: Vec<_> = iter.collect();  // NOW processing happens
```

---

## Summary

| Method | What it does | Returns |
|--------|-------------|---------|
| `.iter()` | Creates iterator over `&T` | Iterator |
| `.map(f)` | Transform each element | Iterator |
| `.filter(f)` | Keep matching elements | Iterator |
| `.collect()` | Collect into collection | Vec, String, etc. |
| `.sum()` | Sum all elements | Number |
| `.fold(init, f)` | Reduce to single value | Any type |
| `.any(f)` | Any element matches? | bool |
| `.all(f)` | All elements match? | bool |
| `.find(f)` | First matching element | Option\<T\> |

Your turn. Open DAY16/DRILL.md.
