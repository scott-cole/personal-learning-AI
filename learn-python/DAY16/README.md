# DAY16 — Iterators & Generators

## Anecdote: Generators are a vending machine that restocks on demand

A regular vending machine holds 30 bags of chips at once. You buy one, the stock drops by one. Eventually it runs out. That's a list — all items loaded upfront, consuming space.

Now imagine a *magical* vending machine. It has no chips inside at all. Instead, behind the glass is a tiny factory. When you insert coins and press "B3", the factory whirs to life, produces exactly one bag of chips, and drops it into the tray. The shelf never holds more than one bag. You want 30 bags? You press the button 30 times. The factory produces them one by one. No warehouse. No inventory. Infinite supply.

That's a generator. It doesn't store all values in memory — it *yields* them one at a time, on demand.

---

## Iterators

Any object that implements `__iter__()` and `__next__()` is an iterator.

```python
nums = [1, 2, 3]
it = iter(nums)         # get an iterator
print(next(it))         # 1
print(next(it))         # 2
print(next(it))         # 3
# print(next(it))       # StopIteration
```

`for` loops work by calling `iter()` on the iterable and `next()` on the iterator until `StopIteration`.

```python
for x in [1, 2, 3]:    # Python does this internally:
    print(x)            # it = iter([1, 2, 3]); while True: x = next(it)
```

### Building a custom iterator

```python
class Countdown:
    def __init__(self, start):
        self.current = start

    def __iter__(self):
        return self

    def __next__(self):
        if self.current <= 0:
            raise StopIteration
        val = self.current
        self.current -= 1
        return val

for n in Countdown(5):
    print(n)  # 5, 4, 3, 2, 1
```

---

## Generators — iterators made simple

A generator is a function that uses `yield` instead of `return`.

```python
def countdown(n):
    while n > 0:
        yield n
        n -= 1

for x in countdown(5):
    print(x)  # 5, 4, 3, 2, 1
```

When Python sees `yield`, the function becomes a generator. Calling it returns a generator object:

```python
gen = countdown(3)
print(gen)        # <generator object countdown at 0x...>
print(next(gen))  # 3
print(next(gen))  # 2
print(next(gen))  # 1
# print(next(gen))  # StopIteration
```

---

## Generator pipelines

Chaining generators creates a pipeline — each step processes items from the previous.

```python
def integers():
    i = 1
    while True:
        yield i
        i += 1

def squares(nums):
    for n in nums:
        yield n ** 2

def take(n, seq):
    for _ in range(n):
        yield next(seq)

# Pipeline: first 5 squares
result = take(5, squares(integers()))
print(list(result))  # [1, 4, 9, 16, 25]
```

No intermediate lists are created. Each value flows through the pipeline one at a time.

---

## Generator expressions

```python
# Generator expression (lazy)
squares = (x**2 for x in range(10))

# vs list comprehension (eager)
squares_list = [x**2 for x in range(10)]
```

Generator expressions are the same syntax as list comprehensions but with `()` instead of `[]`.

---

## `yield from` — delegating to another generator

```python
def chain(*iterables):
    for it in iterables:
        yield from it

print(list(chain("ABC", [1, 2, 3])))
# ['A', 'B', 'C', 1, 2, 3]
```

---

## Infinite generators

```python
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

fib = fibonacci()
print([next(fib) for _ in range(10)])
# [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

---

## Generator methods: `send()`, `throw()`, `close()`

```python
def echo():
    while True:
        val = yield
        print(f"Got: {val}")

g = echo()
next(g)          # prime the generator
g.send("hello")  # Got: hello
g.send(42)       # Got: 42
g.close()        # StopIteration
```

`send()` sends a value back into the generator, which becomes the result of the `yield` expression.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| Iterator | Object with `__iter__` and `__next__` |
| Generator | Function with `yield` — produces values lazily |
| `next()` | Gets the next value from an iterator |
| `StopIteration` | Raised when the iterator is exhausted |
| Pipeline | Chain generators to process data stream |
| `yield from` | Delegate to a sub-generator |
| `send()` | Pass a value back into a generator |
| Memory | Generators store no history — only the current state |
