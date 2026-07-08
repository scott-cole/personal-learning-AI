# DAY03 Drill — The Jewellery Workshop

## Task

Write a Python script (`drill.py`) that processes a string as if it were a necklace of beads.

### Setup

```python
necklace = "Ruby-Emerald-Sapphire-Diamond-Pearl"
```

### Requirements

1. Print the original necklace
2. Print the first gem and the last gem using indexing
3. Print gems at even positions (index 0, 2, 4, …) using slicing
4. Convert to lowercase, then split by `"-"` into a list
5. Join the list back with `" 💎 "` (diamond emoji) and print
6. Replace `"Sapphire"` with `"Amethyst"` and print the new necklace
7. Count how many times the letter `"a"` appears (case-insensitive)
8. Print a summary f-string showing the first gem, last gem, total gems, and the replaced version

### Expected output

```
Original: Ruby-Emerald-Sapphire-Diamond-Pearl
First gem: Ruby
Last gem: Pearl
Even-indexed gems: RubySapphirePearl
Lowercase + split: ['ruby', 'emerald', 'sapphire', 'diamond', 'pearl']
Joined with emoji: ruby 💎 emerald 💎 sapphire 💎 diamond 💎 pearl
After replacement: Ruby-Emerald-Amethyst-Diamond-Pearl
'a' count (case-insensitive): 5
Summary: Necklace starts with Ruby, ends with Pearl, has 5 gems, now with Amethyst instead of Sapphire!
```

### Hints

- `split("-")` returns a list
- Use `.lower().count("a")` to count case-insensitively
- Use `len()` on the list to get the gem count
- `"Ruby-Emerald-Sapphire-Diamond-Pearl"[::2]` gives characters at even indices — but you need the gems. For even gem positions, slice the list after split.

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] Index the first and last character/gem
- [ ] Slice to get gems at even positions
- [ ] Split the string into a list
- [ ] Join with a separator
- [ ] Replace a substring
- [ ] Count occurrences of a character
- [ ] Print a formatted summary
