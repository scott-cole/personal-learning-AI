# DAY14 Drill — Build the CSV Processor

## Task

Write a complete CSV Data Processor (`processor.py`) and a test input file.

### Step 1: Create `sales.csv`

```csv
product,region,units,price,total
Widget,North,10,12.50,125.00
Gadget,South,25,8.00,200.00
Widget,East,5,12.50,62.50
Doohickey,West,3,25.00,75.00
Gadget,North,15,8.00,120.00
Widget,South,20,12.50,250.00
Doohickey,East,7,25.00,175.00
Gadget,West,10,8.00,80.00
```

### Step 2: Implement `CSVProcessor` class

See the README for the full spec, or implement:

- `load()` — reads CSV into list of dicts
- `filter(field, op, value)` — supports `==`, `!=`, `>`, `<`
- `sort(field, reverse=False)`
- `select(*fields)` — keep only given columns
- `save(output)` — writes to new CSV

### Step 3: Write a `main()` that:

1. Loads `sales.csv`
2. Filters rows where `region == "North"`
3. Sorts by `total` descending
4. Selects `product`, `units`, `total`
5. Saves to `north_sales.csv`

### Expected output (console)

```
Loaded 8 rows from sales.csv
Filtered: kept 2 rows (region == North)
Sorted by total descending
Saved 2 rows to north_sales.csv
```

### Expected `north_sales.csv`

```csv
product,units,total
Widget,10,125.00
Gadget,15,120.00
```

### Hints

- `.filter()` needs to return `self` for chaining
- Numeric comparison: try `float(a)` conversion in the filter lambda
- Use `newline=""` when writing CSV on Windows

### How to run

```bash
python3 processor.py
```

## TODOs

- [ ] Create `sales.csv` with sample data
- [ ] Implement `CSVProcessor.load()`
- [ ] Implement `CSVProcessor.filter()`
- [ ] Implement `CSVProcessor.sort()`
- [ ] Implement `CSVProcessor.select()`
- [ ] Implement `CSVProcessor.save()`
- [ ] Build a processing pipeline and test
