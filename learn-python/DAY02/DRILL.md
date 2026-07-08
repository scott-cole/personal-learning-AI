# DAY02 Drill — The Kitchen Blender

## Task

Write a Python script (`drill.py`) that defines and uses several functions around a kitchen theme.

### Requirements

1. **`make_smoothie(ingredients)`** — takes a list of strings and returns a sentence like `"Blending apple, banana, spinach into a smoothie!"`
2. **`bake(temp, time, item="cookies")`** — returns `"Baking cookies at 350°F for 15 minutes"` (default temp=350, time=15)
3. **`kitchen_timer(minutes)`** — prints a countdown from `minutes` to 1, then prints `"DING! Ready!"` and returns `"Done"`
4. **`describe_meal(name, *ingredients)`** — uses `*args` to accept any number of ingredients and returns a sentence like `"Pasta with tomato, basil, garlic"`

### Main block

Call each function with sample arguments and print the results.

### Expected output

```
=== SMOOTHIE ===
Blending apple, banana, spinach into a smoothie!
=== BAKING ===
Baking cookies at 350°F for 15 minutes
Baking bread at 400°F for 45 minutes
=== TIMER ===
3...
2...
1...
DING! Ready!
Timer returned: Done
=== MEAL ===
Pasta with tomato, basil, garlic
```

### Hints

- Use `for` loop with `range()` for the countdown (we cover loops properly on Day 6, but you can handle it)
- `*ingredients` collects extra positional arguments into a tuple
- Use f-strings for all formatting

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] Define `make_smoothie(ingredients)` and return a formatted string
- [ ] Define `bake(temp, time, item)` with default values
- [ ] Define `kitchen_timer(minutes)` with a loop and return value
- [ ] Define `describe_meal(name, *ingredients)`
- [ ] Call all functions and print their results
