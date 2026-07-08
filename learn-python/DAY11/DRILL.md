# DAY11 Drill — The Toolbox Builder

## Task

Create a small Python package with two modules, then use them from a main script.

### Structure

```
drill/
    my_tools/
        __init__.py
        strings.py
        numbers.py
    drill.py
```

### `my_tools/strings.py`

Provide these functions:

- `reverse(s)` — returns the reversed string
- `count_vowels(s)` — returns the count of vowels (a, e, i, o, u, case-insensitive)
- `is_palindrome(s)` — returns `True` if the string reads the same forwards and backwards

### `my_tools/numbers.py`

Provide these functions:

- `factorial(n)` — returns `n!` (use a loop, not recursion)
- `is_prime(n)` — returns `True` if `n` is prime
- `gcd(a, b)` — returns the greatest common divisor

### `__init__.py`

Make `reverse` and `factorial` available at the package level:

```python
from .strings import reverse
from .numbers import factorial

__all__ = ["reverse", "factorial"]
```

### `drill.py`

Import from `my_tools` and demonstrate all functions.

### Expected output

```
=== STRING TOOLS ===
reverse("Python") = nohtyP
count_vowels("Hello World") = 3
is_palindrome("racecar") = True
is_palindrome("hello") = False
=== NUMBER TOOLS ===
factorial(5) = 120
is_prime(17) = True
is_prime(15) = False
gcd(48, 18) = 6
=== PACKAGE LEVEL ===
reverse("package") = egakcap
factorial(3) = 6
```

### Hints

- Use `from my_tools.strings import ...` style imports
- `__init__.py` uses relative imports: `from .strings import reverse`
- Run `drill.py` from the `drill/` directory: `python3 drill.py`
- Palindrome: compare `s.lower()` to `s.lower()[::-1]`

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] Create directory structure
- [ ] Implement `strings.py` with three functions
- [ ] Implement `numbers.py` with three functions
- [ ] Create `__init__.py` with explicit imports
- [ ] Write `drill.py` that uses all functions
