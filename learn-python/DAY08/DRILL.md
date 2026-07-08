# DAY08 Drill — The Assembly Line

## Task

Write a Python script (`drill.py`) that uses comprehensions and generator expressions to process a list of product orders.

### Setup

```python
orders = [
    {"id": 101, "item": "Widget", "qty": 5, "price": 12.50},
    {"id": 102, "item": "Gadget", "qty": 12, "price": 8.00},
    {"id": 103, "item": "Doohickey", "qty": 3, "price": 25.00},
    {"id": 104, "item": "Thingamajig", "qty": 0, "price": 50.00},
    {"id": 105, "item": "Whatsit", "qty": 8, "price": 15.00},
]
```

### Requirements

1. **List comp**: Extract item names in uppercase for orders with `qty > 0`
2. **Dict comp**: Create a `{item_name: total_value}` dict for orders with `qty > 0`
3. **Generator expr**: Compute the total value of all orders (use `sum()`)
4. **List comp with `if/else`**: Label each order as `"In Stock"` or `"Backordered"` based on `qty > 0`
5. **Set comp**: Find all unique first letters of item names

### Expected output

```
In-stock items (uppercase): ['WIDGET', 'GADGET', 'DOOHICKEY', 'WHATSIT']
Order values: {'Widget': 62.5, 'Gadget': 96.0, 'Doohickey': 75.0, 'Whatsit': 120.0}
Total value: $353.50
Stock status: ['In Stock', 'In Stock', 'In Stock', 'Backordered', 'In Stock']
First letters: {'W', 'G', 'D', 'T'}
```

### Hints

- `qty * price` for total value
- Generator: `sum(order["qty"] * order["price"] for order in orders)`
- First letters: `item[0]` for each `item` in orders
- Use f-strings for the total value formatting

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] List comprehension with filter
- [ ] Dict comprehension
- [ ] Generator expression with `sum()`
- [ ] List comprehension with `if/else` ternary
- [ ] Set comprehension
