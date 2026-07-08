# DAY22 — JSON, CSV & YAML Serialization

## Anecdote: Serialization is a suitcase

You're going on a trip. Your belongings — clothes, toiletries, books — are scattered around the room (Python objects in memory). You can't carry a room with you. So you pack everything into a suitcase (serialize to a file), zip it up (write bytes), and carry it on the plane. At the destination, you unzip (read bytes) and unpack (deserialize back into objects). Everything is exactly as you left it.

A suitcase is a *portable format*. JSON, CSV, and YAML are different kinds of suitcases — different zippers, different pockets — but they all serve the same purpose: convert in-memory objects to a storable/transmittable format, and back again.

---

## JSON — JavaScript Object Notation

The universal data exchange format. `json` module is in the standard library.

### Serialization (Python → JSON)

```python
import json

data = {
    "name": "Alice",
    "age": 28,
    "skills": ["Python", "SQL", "Flask"],
    "employed": True,
    "salary": None,
}

# To string
json_str = json.dumps(data, indent=2)
print(json_str)

# To file
with open("data.json", "w") as f:
    json.dump(data, f, indent=2)
```

### Deserialization (JSON → Python)

```python
# From string
data = json.loads(json_str)

# From file
with open("data.json", "r") as f:
    data = json.load(f)
```

### JSON ↔ Python type mapping

| JSON | Python |
|------|--------|
| `string` | `str` |
| `number` | `int` / `float` |
| `boolean` | `bool` |
| `null` | `None` |
| `array` | `list` |
| `object` | `dict` |

---

## CSV — Comma-Separated Values

Tabular data. `csv` module is standard library.

```python
import csv

# Reading as dicts
with open("employees.csv", "r") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row["name"], row["salary"])

# Writing dicts
with open("output.csv", "w", newline="") as f:
    fieldnames = ["name", "salary", "department"]
    writer = csv.DictWriter(f, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerow({"name": "Alice", "salary": 95000, "department": "Engineering"})
    writer.writerows([
        {"name": "Bob", "salary": 72000, "department": "Marketing"},
        {"name": "Charlie", "salary": 110000, "department": "Engineering"},
    ])
```

---

## YAML — YAML Ain't Markup Language

Human-readable data serialization. Requires `pyyaml` package.

```bash
pip install pyyaml
```

```python
import yaml

data = {
    "app": "my-app",
    "version": "2.0",
    "database": {
        "host": "localhost",
        "port": 5432,
        "name": "mydb",
    },
    "features": ["auth", "logging", "api"],
}

# To string
yaml_str = yaml.dump(data, default_flow_style=False)
print(yaml_str)
# app: my-app
# database:
#   host: localhost
#   name: mydb
#   port: 5432
# features:
#   - auth
#   - logging
#   - api
# version: '2.0'

# To file
with open("config.yaml", "w") as f:
    yaml.dump(data, f)

# Read
with open("config.yaml", "r") as f:
    parsed = yaml.safe_load(f)
```

---

## Custom serialization

JSON and YAML don't handle custom objects by default.

```python
import json
from datetime import datetime

def serialize(obj):
    if isinstance(obj, datetime):
        return obj.isoformat()
    raise TypeError(f"Object of type {type(obj)} is not JSON serializable")

data = {"event": "meeting", "time": datetime.now()}
json_str = json.dumps(data, default=serialize)
```

---

## Which format when?

| Format | Strengths | Weaknesses |
|--------|-----------|------------|
| JSON | Universal, fast, standard library | No comments, no dates |
| CSV | Tabular, spreadsheet-friendly | Nested data is awkward |
| YAML | Human-readable, comments, configs | Slower, needs `pyyaml` |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| `json.dumps()` / `json.loads()` | String ↔ Python object |
| `json.dump()` / `json.load()` | File ↔ Python object |
| `csv.DictReader` / `csv.DictWriter` | Read/write CSV as dicts |
| `yaml.dump()` / `yaml.safe_load()` | String/file ↔ Python object |
| Custom encoders | Handle non-standard types |
| Format choice | JSON for APIs, YAML for configs, CSV for tables |
