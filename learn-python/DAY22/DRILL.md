# DAY22 Drill — The Suitcase Packer

## Task

Write a Python script (`drill.py`) that works with JSON, CSV, and YAML formats.

### Setup

```python
inventory = [
    {"id": 1, "item": "Laptop", "qty": 5, "price": 1200.00, "tags": ["electronics", "computers"]},
    {"id": 2, "item": "Mouse", "qty": 20, "price": 25.00, "tags": ["electronics", "peripherals"]},
    {"id": 3, "item": "Keyboard", "qty": 15, "price": 75.00, "tags": ["electronics", "peripherals"]},
    {"id": 4, "item": "Monitor", "qty": 8, "price": 350.00, "tags": ["electronics", "displays"]},
    {"id": 5, "item": "Desk Lamp", "qty": 12, "price": 45.00, "tags": ["office", "lighting"]},
]
```

### Requirements

1. **Write JSON** — Serialize `inventory` to `inventory.json` with `indent=2`
2. **Read JSON** — Read `inventory.json` back and print the first item
3. **Write CSV** — Write a flat version (without tags) to `inventory.csv`
4. **Read CSV** — Read `inventory.csv` back and print each row
5. **Write YAML** — Serialize `inventory` to `inventory.yaml`
6. **Read YAML** — Read `inventory.yaml` back and print the item count
7. **Filter** — After loading from JSON, print items with `qty < 10`

### Expected output

```
=== JSON ===
Written inventory.json (5 items)
First item: Laptop (qty: 5, price: $1200.00)
=== CSV ===
Written inventory.csv
CSV rows:
  Laptop,5,1200.0
  Mouse,20,25.0
  Keyboard,15,75.0
  Monitor,8,350.0
  Desk Lamp,12,45.0
=== YAML ===
Written inventory.yaml
Loaded YAML: 5 items
=== LOW STOCK (qty < 10) ===
Laptop: 5
Monitor: 8
```

### Hints

- JSON: `json.dump(inventory, f, indent=2)`
- CSV: Use `csv.DictWriter` with `fieldnames=["id", "item", "qty", "price"]` (skip tags)
- YAML: `yaml.dump(inventory, f)` and `yaml.safe_load(f)`
- For low stock: list comprehension `[item for item in data if item["qty"] < 10]`

### How to run

```bash
pip install pyyaml
python3 drill.py
```

## TODOs

- [ ] Write JSON file
- [ ] Read JSON file and access data
- [ ] Write CSV file (flat, no nested data)
- [ ] Read CSV file
- [ ] Write YAML file
- [ ] Read YAML file
- [ ] Filter and print low-stock items
