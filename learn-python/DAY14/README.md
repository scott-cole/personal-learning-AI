# DAY14 — Project: CSV Data Processor

## Anecdote: Your second workshop build

The stool you built on Day 7 is holding up well. Now it's time to build a tool rack — something that takes in raw lumber (messy CSV data), cuts it to size (filters), shapes it (transforms), and stacks it neatly on the shelf (writes output).

A **CSV data processor** reads a file of comma-separated values, lets you filter and manipulate rows, and writes the result. It's a tool you'll use constantly in real-world Python work — data analysis, report generation, ETL pipelines. Today you'll build one from scratch.

---

## What is CSV?

CSV = Comma-Separated Values. A plain-text tabular format:

```csv
name,age,city,score
Alice,28,London,85.5
Bob,32,Paris,72.0
Charlie,25,London,91.0
Diana,30,Berlin,88.5
```

Python's `csv` module handles parsing and writing:

```python
import csv

# Reading
with open("data.csv", "r") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row)  # Each row is an OrderedDict

# Writing
with open("output.csv", "w", newline="") as f:
    fieldnames = ["name", "age", "city", "score"]
    writer = csv.DictWriter(f, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerow({"name": "Alice", "age": 28, "city": "London", "score": 85.5})
```

---

## Building the processor

Our processor has three stages:

1. **Read** — load CSV into a list of dicts
2. **Process** — filter, sort, transform
3. **Write** — save to a new CSV

### Architecture

```python
class CSVProcessor:
    def __init__(self, filename):
        self.filename = filename
        self.data = []

    def load(self):
        """Read CSV file into self.data as list of dicts."""
        with open(self.filename, "r", newline="") as f:
            reader = csv.DictReader(f)
            self.data = list(reader)
        print(f"Loaded {len(self.data)} rows from {self.filename}")

    def filter(self, field, operator, value):
        """Keep rows where field matches condition."""
        ops = {
            "==": lambda a, b: a == b,
            "!=": lambda a, b: a != b,
            ">": lambda a, b: float(a) > float(b),
            "<": lambda a, b: float(a) < float(b),
        }
        self.data = [
            row for row in self.data
            if ops[operator](row[field], value)
        ]
        return self

    def sort(self, field, reverse=False):
        """Sort by a field."""
        self.data.sort(key=lambda row: row[field], reverse=reverse)
        return self

    def select(self, *fields):
        """Keep only specified fields."""
        self.data = [
            {f: row[f] for f in fields}
            for row in self.data
        ]
        return self

    def save(self, output_filename):
        """Write processed data to a new CSV."""
        if not self.data:
            print("No data to save!")
            return
        fieldnames = list(self.data[0].keys())
        with open(output_filename, "w", newline="") as f:
            writer = csv.DictWriter(f, fieldnames=fieldnames)
            writer.writeheader()
            writer.writerows(self.data)
        print(f"Saved {len(self.data)} rows to {output_filename}")
```

### Usage

```python
proc = CSVProcessor("students.csv")
proc.load()
proc.filter("city", "==", "London")\
    .sort("score", reverse=True)\
    .select("name", "score")\
    .save("london_students.csv")
```

Method chaining (returning `self`) makes the pipeline read like a sentence.

---

## Handling edge cases

- Empty CSV → handle gracefully
- Missing fields → provide a clear error
- Numeric comparison → convert to `float()` in filter

---

## Summary

| Component | Purpose |
|-----------|---------|
| `csv.DictReader` | Read CSV rows as dicts |
| `csv.DictWriter` | Write dicts to CSV |
| Method chaining | `return self` for fluent API |
| `filter()` | Keep rows matching condition |
| `sort()` | Order rows by field |
| `select()` | Choose which columns to keep |
| `save()` | Write output to file |
