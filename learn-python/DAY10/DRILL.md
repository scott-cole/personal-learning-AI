# DAY10 Drill — The Safety Net

## Task

Write a Python script (`drill.py`) that performs a series of risky operations with proper error handling.

### Requirements

1. **Safe division** — Write `safe_divide(a, b)` that returns `a / b` or handles `ZeroDivisionError` and `TypeError`
2. **Safe integer input** — Write `get_integer(prompt)` that keeps asking until the user enters a valid integer
3. **Safe file read** — Write `read_file_safe(filename)` that returns the file content or `None` if the file doesn't exist
4. **Custom exception** — Define `TemperatureError(Exception)` and write `check_temperature(temp)` that raises it if `temp > 50` or `temp < -50`
5. **Main block** — Demonstrate all functions

### Expected output

```
=== SAFE DIVISION ===
10 / 2 = 5.0
10 / 0 = Error: division by zero
10 / 'a' = Error: unsupported operand type(s)
=== INTEGER INPUT ===
Enter a number: abc
Invalid input. Try again.
Enter a number: 42
You entered: 42
=== FILE READ ===
Reading missing.txt: File not found, returning None
Reading drill.py: <contents of this file...>
=== TEMPERATURE CHECK ===
Checking 25: OK
Checking 100: TemperatureError: Temperature 100 is out of range!
Checking -60: TemperatureError: Temperature -60 is out of range!
```

### Hints

- `except (ZeroDivisionError, TypeError) as e:` catches both
- Use a `while` loop with `try/except` for `get_integer`
- `FileNotFoundError` is a subclass of `OSError`
- Custom exceptions are just `class MyError(Exception): pass`

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] `safe_divide(a, b)` with exception handling
- [ ] `get_integer(prompt)` with retry loop
- [ ] `read_file_safe(filename)` returning `None` on failure
- [ ] `TemperatureError` custom exception class
- [ ] `check_temperature(temp)` with raise
- [ ] Main block demonstrating all functions
