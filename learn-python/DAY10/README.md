# DAY10 — Error Handling

## Anecdote: Exceptions are a safety net

A tightrope walker performs high above the crowd. The rope is her code. If she loses her balance (an *error*), she doesn't just fall to the ground — there's a safety net below (the `try/except` block). The net catches her, and she climbs back up to try again. The audience doesn't see a tragedy; they see a recoverable stumble.

Python programs have safety nets too. Code that might fail goes inside `try`. The recovery plan goes in `except`. Code that *must* run regardless goes in `finally`. And if you want to signal that something is wrong? You `raise` your own alarm.

---

## Basic `try` / `except`

```python
try:
    num = int(input("Enter a number: "))
    print(10 / num)
except ValueError:
    print("That's not a valid number!")
except ZeroDivisionError:
    print("Can't divide by zero!")
```

If `int()` raises `ValueError`, the `except ValueError` block runs. If `10 / num` raises `ZeroDivisionError`, the second block runs. If no error occurs, both except blocks are skipped.

---

## Catching multiple exceptions

```python
try:
    result = risky_operation()
except (ValueError, TypeError, RuntimeError) as e:
    print(f"Caught: {e}")
```

### Catching all exceptions (use sparingly)

```python
try:
    risky()
except Exception as e:
    print(f"Something went wrong: {e}")
```

Never use bare `except:` — it catches `KeyboardInterrupt` (Ctrl+C) and `SystemExit` too, making your program impossible to kill.

---

## `else` and `finally`

```python
try:
    file = open("data.txt", "r")
    content = file.read()
except FileNotFoundError:
    print("File not found!")
else:
    print(f"Read {len(content)} characters")  # runs if no exception
finally:
    file.close()  # always runs — cleanup
```

| Block | When it runs |
|-------|-------------|
| `try` | Always — contains the risky code |
| `except` | Only if an exception occurs |
| `else` | Only if no exception occurred |
| `finally` | Always — cleanup, even after `return` |

---

## Raising exceptions

```python
def withdraw(balance, amount):
    if amount < 0:
        raise ValueError("Amount cannot be negative")
    if amount > balance:
        raise ValueError("Insufficient funds")
    return balance - amount

# Caller must handle it
try:
    new_balance = withdraw(100, 200)
except ValueError as e:
    print(f"Error: {e}")
```

---

## Custom exception classes

```python
class InsufficientFundsError(Exception):
    """Raised when withdrawal exceeds balance."""
    pass

class NegativeAmountError(Exception):
    """Raised when amount is negative."""
    def __init__(self, amount):
        self.amount = amount
        super().__init__(f"Negative amount: {amount}")

def withdraw(balance, amount):
    if amount < 0:
        raise NegativeAmountError(amount)
    if amount > balance:
        raise InsufficientFundsError()
    return balance - amount
```

---

## The exception hierarchy

BaseException
├── SystemExit
├── KeyboardInterrupt
├── GeneratorExit
└── Exception
    ├── StopIteration
    ├── ArithmeticError
    │   ├── ZeroDivisionError
    │   └── FloatingPointError
    ├── LookupError
    │   ├── IndexError
    │   └── KeyError
    ├── ValueError
    ├── TypeError
    ├── FileNotFoundError (OSError)
    └── ...

Always catch the most specific exception first. `except Exception` as the last catch-all.

---

## `assert` for debugging

```python
def divide(a, b):
    assert b != 0, "Division by zero!"
    return a / b
```

`assert` raises `AssertionError` if the condition is `False`. Use for debugging/invariants, not for input validation (assertions can be disabled with `-O` flag).

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| `try/except` | Catch and handle errors gracefully |
| `else` | Runs when no exception occurred |
| `finally` | Always runs — perfect for cleanup |
| `raise` | Signal an error intentionally |
| Custom exceptions | Inherit from `Exception` |
| `assert` | Debug-only checks (disable with `-O`) |
| Specificity | Catch specific exceptions before general ones |
