# DAY05 Drill — The Nightclub Manager

## Task

Write a Python script (`drill.py`) that runs a virtual nightclub using dicts, tuples, and sets.

### Requirements

1. **Coat check** — Create a dict `coats` mapping ticket numbers (int) to guest names (str). Populate with:
   - `42: "Ada"`
   - `7: "Marie"`
   - `99: "Alan"`
2. Print the full coat check dict
3. A new guest `"Grace"` arrives with ticket `13` — add them
4. Guest `7` retrieves their coat — `pop()` them out and print `"{name} got their coat"`
5. Print the updated coat check
6. **VIP list** — Create a set `vip_list` with `"Ada"`, `"Grace"`, `"Charles"`
7. Check if `"Marie"` is VIP and print the result
8. Add `"Alan"` to the VIP list
9. Print the final VIP list (sorted)
10. **Coordinates** — Create a tuple `stage_coords` with `(10, 20)` and unpack it into `x, y`, then print

### Expected output

```
=== COAT CHECK ===
{42: 'Ada', 7: 'Marie', 99: 'Alan'}
Added Grace with ticket 13
Marie got their coat!
Updated coats: {42: 'Ada', 99: 'Alan', 13: 'Grace'}
=== VIP LIST ===
Is Marie VIP? False
Alan added to VIP list
Final VIPs (sorted): ['Ada', 'Alan', 'Charles', 'Grace']
=== STAGE COORDS ===
Stage is at x=10, y=20
```

### Hints

- `pop(key)` removes and returns the *value*
- Use `sorted(vip_list)` to get a sorted list from a set
- Tuples are created with parentheses; unpack with `x, y = coords`

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] Create a dict with ticket-number → name mapping
- [ ] Add and remove entries with `pop()`
- [ ] Create a set and test membership with `in`
- [ ] Add to a set with `.add()`
- [ ] Create and unpack a tuple
