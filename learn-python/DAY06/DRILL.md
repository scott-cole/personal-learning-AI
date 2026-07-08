# DAY06 Drill — The Forest Hike Simulator

## Task

Write a Python script (`drill.py`) that simulates a hike using conditionals and loops.

### Requirements

1. Create a list `trail_markers = ["Oak Tree", "Stream", "Cabin", "Waterfall", "Summit"]`
2. Use a `for` loop to walk the trail — print `"Reached: {marker}"` for each marker
3. Use `enumerate()` to print `"Stop {n}: {marker}"`
4. Ask for weather input (simulate with a variable `weather = "sunny"`)
5. Use `if/elif/else` to decide the route:
   - `"sunny"` → `"Taking the summit trail!"`
   - `"rainy"` → `"Taking the waterfall trail!"`
   - otherwise → `"Staying at the cabin"`
6. Use a `while` loop for a countdown timer: start at 5, count down to 1, then print `"Go!"`
7. Use a `for` loop with `range()` to print even numbers from 2 to 10
8. Use `break` to stop when you reach `"Cabin"` (print `"Resting at Cabin"`)

### Expected output

```
=== HIKING TRAIL ===
Reached: Oak Tree
Reached: Stream
Reached: Cabin
Reached: Waterfall
Reached: Summit
=== WITH ENUMERATE ===
Stop 0: Oak Tree
Stop 1: Stream
Stop 2: Cabin
Stop 3: Waterfall
Stop 4: Summit
=== WEATHER CHECK ===
Weather: sunny
Taking the summit trail!
=== COUNTDOWN ===
5... 4... 3... 2... 1... Go!
=== EVENS ===
2 4 6 8 10
=== EARLY STOP ===
Reached: Oak Tree
Reached: Stream
Reached: Cabin — resting!
```

### Hints

- `print("...", end=" ")` to keep output on the same line
- `range(2, 11, 2)` produces 2, 4, 6, 8, 10
- Use `if marker == "Cabin": break` for the early stop

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] Loop over a list with `for`
- [ ] Use `enumerate()` to get index + value
- [ ] Write an `if/elif/else` block
- [ ] Write a `while` loop
- [ ] Use `range()` with step
- [ ] Use `break` to exit a loop
