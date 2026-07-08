# DAY01 Drill — The Workshop Inventory

## Task

You're managing a small workshop. Write a Python script (`drill.py`) that:

1. Creates variables for:
   - `item_name` (str) — name of a tool, e.g. `"Wrench"`
   - `quantity` (int) — how many you have, e.g. `12`
   - `price` (float) — cost per unit, e.g. `8.99`
   - `in_stock` (bool) — `True` if available
2. Prints each variable with a label (e.g., `Item: Wrench`)
3. Prints the type of each variable using `type()`
4. Calculates and prints the total value (`quantity * price`)
5. Uses an f-string to print a summary line

### Expected output (with your chosen values):

```
=== WORKSHOP INVENTORY ===
Item: Wrench
Quantity: 12
Price: $8.99
In stock: True
--- Types ---
Item type: <class 'str'>
Quantity type: <class 'int'>
Price type: <class 'float'>
In stock type: <class 'bool'>
--- Calculation ---
Total value: $107.88
--- Summary ---
12 x Wrench @ $8.99 each = $107.88
```

### Hints

- Use `print()` with `===` separators for readability
- Multiplication works between `int` and `float`: `quantity * price`
- f-strings: `f"{quantity} x {item_name} @ ${price} each = ${total}"`
- You can round floats: `f"{total:.2f}"`

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] Create the four variables
- [ ] Print each variable with a label
- [ ] Print each variable's type using `type()`
- [ ] Compute total = quantity * price and print it
- [ ] Print a one-line summary with f-string formatting
