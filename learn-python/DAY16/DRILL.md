# DAY16 Drill — The Vending Machine Pipeline

## Task

Write a Python script (`drill.py`) that builds a data processing pipeline using generators.

### Requirements

1. **`integers(start=1)`** — infinite generator yielding integers starting from `start`
2. **`evens(nums)`** — yields only even numbers from the input sequence
3. **`multiply_by(factor, nums)`** — yields each input value multiplied by `factor`
4. **`take(n, seq)`** — yields the first `n` items from the sequence, then stops
5. **`fibonacci()`** — infinite generator yielding Fibonacci numbers

### Pipeline demonstrations

**Pipeline A**: First 10 even integers, each multiplied by 3:

```python
pipeline = take(10, multiply_by(3, evens(integers())))
print(list(pipeline))
# [6, 12, 18, 24, 30, 36, 42, 48, 54, 60]
```

**Pipeline B**: First 8 Fibonacci numbers:

```python
fibs = take(8, fibonacci())
print(list(fibs))
# [0, 1, 1, 2, 3, 5, 8, 13]
```

### Expected output

```
=== PIPELINE A: First 10 even ints × 3 ===
[6, 12, 18, 24, 30, 36, 42, 48, 54, 60]
=== PIPELINE B: First 8 Fibonacci ===
[0, 1, 1, 2, 3, 5, 8, 13]
=== CUSTOM ITERATOR: Countdown 5 ===
5 4 3 2 1 Blastoff!
```

**Bonus**: Write a `Countdown` class (custom iterator) that counts down and prints `"Blastoff!"` when done.

### Hints

- `integers()` uses `while True: yield i; i += 1`
- `evens` checks `if n % 2 == 0`
- `take` uses a `for` loop with a counter and `break`
- Generators are single-use — recreate for each pipeline

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] Implement `integers()` generator
- [ ] Implement `evens()` generator
- [ ] Implement `multiply_by()` generator
- [ ] Implement `take()` generator
- [ ] Implement `fibonacci()` generator
- [ ] Build and run both pipelines
- [ ] (Bonus) Write `Countdown` iterator class
