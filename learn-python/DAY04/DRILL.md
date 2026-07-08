# DAY04 Drill — The Locker Bank Manager

## Task

Write a Python script (`drill.py`) that manages a row of lockers.

### Setup

```python
lockers = ["empty", "empty", "empty", "empty", "empty"]
```

### Requirements

1. Print the initial row of lockers
2. Assign items to specific locker indices:
   - Locker 0: `"backpack"`
   - Locker 2: `"gym clothes"`
   - Locker 4: `"water bottle"`
3. Print the lockers after assignment
4. `append()` a new locker at the end: `"towel"`
5. `insert()` `"snack"` at position 1 (shifting everything right)
6. `pop()` the last locker and print what was removed
7. `pop(0)` the first locker and print what was removed
8. Sort the remaining lockers alphabetically with `.sort()` and print
9. Reverse the order with `.reverse()` and print
10. Print the final count using `len()`

### Expected output

```
Initial lockers: ['empty', 'empty', 'empty', 'empty', 'empty']
After assignment: ['backpack', 'empty', 'gym clothes', 'empty', 'water bottle']
After append: ['backpack', 'empty', 'gym clothes', 'empty', 'water bottle', 'towel']
After insert: ['backpack', 'snack', 'empty', 'gym clothes', 'empty', 'water bottle', 'towel']
Popped last: towel
Popped first: backpack
Sorted: ['empty', 'empty', 'gym clothes', 'snack', 'water bottle']
Reversed: ['water bottle', 'snack', 'gym clothes', 'empty', 'empty']
Final locker count: 5
```

### Hints

- Print after each mutation to see the changes
- Remember `.pop()` returns the removed element
- `.sort()` modifies the list in place and returns `None`

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] Create the initial lockers list
- [ ] Assign items by index
- [ ] Use `.append()` to add an item
- [ ] Use `.insert()` to add at a specific position
- [ ] Use `.pop()` and print the returned value
- [ ] Use `.sort()` and `.reverse()`
- [ ] Print results and final count
