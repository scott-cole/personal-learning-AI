# DAY12 Drill — The Cookie Cutter Bakery

## Task

Write a Python script (`drill.py`) with a `Cookie` class and a `Bakery` class.

### `Cookie` class

- `__init__(self, shape, flavour, icing=None)` — shape (str), flavour (str), icing (optional str)
- `__str__` — returns `"A {icing} iced {flavour} {shape}"` (omit icing if None)
- `__repr__` — returns `"Cookie('{shape}', '{flavour}', '{icing}')"`
- `decorate(self, icing)` — sets the icing and returns `self` (for chaining)
- `@property` `description` — returns `"{flavour} {shape} biscuit"`

### `Bakery` class

- `__init__(self, name)` — sets the bakery name and an empty `cookies` list
- `bake(self, shape, flavour)` — creates a `Cookie`, adds to `cookies`, returns the cookie
- `menu(self)` — prints a numbered list of all cookies baked

### Expected output

```
=== BAKERY OPEN ===
Welcome to Python Bakery!
=== BAKING ===
Baked: A vanilla star
Baked: A chocolate iced chocolate circle
Baked: A strawberry iced strawberry heart
=== MENU ===
--- Python Bakery Menu ---
1. A vanilla star
2. A chocolate iced chocolate circle
3. A strawberry iced strawberry heart
=== CHAINING DEMO ===
Chained: A pink iced vanilla star
```

### Hints

- `__str__` checks `if self.icing` to decide the format
- Method chaining: `return self` from `decorate`
- The `@property` decorator makes `description` look like an attribute

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] Define `Cookie` class with `__init__`, `__str__`, `__repr__`
- [ ] Add `decorate()` method that returns `self`
- [ ] Add `description` property
- [ ] Define `Bakery` class with `bake()` and `menu()`
- [ ] Create bakery, bake cookies, print menu
