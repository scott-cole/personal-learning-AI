# DAY17 Drill — The Post-it Note Office

## Task

Write a Python script (`drill.py`) using lambdas, `map`, `filter`, `reduce`, and `sorted`.

### Setup

```python
employees = [
    {"name": "Alice", "department": "Engineering", "salary": 95000, "years": 5},
    {"name": "Bob", "department": "Marketing", "salary": 72000, "years": 3},
    {"name": "Charlie", "department": "Engineering", "salary": 110000, "years": 8},
    {"name": "Diana", "department": "Sales", "salary": 85000, "years": 4},
    {"name": "Eve", "department": "Marketing", "salary": 65000, "years": 2},
    {"name": "Frank", "department": "Engineering", "salary": 120000, "years": 10},
]
```

### Requirements

1. **`map`** — Create a list of employee names in uppercase
2. **`filter`** — Get only Engineering employees
3. **`sorted`** — Sort employees by salary (descending) and print name + salary
4. **`reduce`** — Calculate the total salary bill using `reduce`
5. **`max` with key** — Find the employee with the most years of service
6. **Combo** — Calculate the average salary of Engineering employees with 5+ years

### Expected output

```
=== UPPERCASE NAMES (map) ===
['ALICE', 'BOB', 'CHARLIE', 'DIANA', 'EVE', 'FRANK']
=== ENGINEERING ONLY (filter) ===
['Alice', 'Charlie', 'Frank']
=== SORTED BY SALARY ===
Frank: $120000
Charlie: $110000
Alice: $95000
Diana: $85000
Bob: $72000
Eve: $65000
=== TOTAL SALARY (reduce) ===
Total salary bill: $547000
=== MOST TENURED (max with key) ===
Most tenured: Frank (10 years)
=== ENGINEERING 5+ YRS AVG ===
Engineering employees with 5+ years: average salary $108333.33
```

### Hints

- `map(lambda e: e["name"].upper(), employees)`
- `filter(lambda e: e["department"] == "Engineering", employees)`
- `reduce(lambda total, e: total + e["salary"], employees, 0)`
- For the combo: chain `filter` for Engineering, then `filter` for years >= 5, then `map` to get salaries, then compute average

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] Use `map` to transform data
- [ ] Use `filter` to select data
- [ ] Use `sorted` with `key` and `reverse`
- [ ] Use `reduce` to aggregate
- [ ] Use `max` with `key`
- [ ] Combine `map`/`filter` for a complex query
