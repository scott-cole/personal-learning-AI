# DAY28 — Logging & argparse

## Anecdote: Logging is a flight recorder; argparse is a waiter

A plane has a black box (orange, really) that records everything — altitude, speed, engine readings, pilot conversations. When something goes wrong, investigators don't guess. They pull the flight recorder and replay the data. That's logging — a permanent, timestamped, level-coded record of what your program did.

Now picture a waiter at a restaurant. They come to your table and ask: "Would you like soup or salad? Steak or fish? What temperature for the steak?" They take your preferences, relay them to the kitchen, and bring exactly what you ordered. That's argparse — it walks up to your program and asks: "What input file? What output format? Verbose mode?" Your program responds accordingly.

---

## The `logging` module

```python
import logging

# Basic configuration
logging.basicConfig(
    level=logging.DEBUG,
    format="%(asctime)s [%(levelname)s] %(message)s",
    datefmt="%Y-%m-%d %H:%M:%S",
)

logging.debug("Detailed debug info")
logging.info("General information")
logging.warning("Something unexpected but not critical")
logging.error("A problem occurred")
logging.critical("System is shutting down")
```

### Log levels

| Level | Numeric value | When to use |
|-------|--------------|-------------|
| `DEBUG` | 10 | Detailed diagnostic info |
| `INFO` | 20 | Confirmation things are working |
| `WARNING` | 30 | Something unexpected but okay |
| `ERROR` | 40 | A problem, but program continues |
| `CRITICAL` | 50 | Program cannot continue |

### Logging to a file

```python
logging.basicConfig(
    level=logging.INFO,
    filename="app.log",
    filemode="a",
    format="%(asctime)s [%(levelname)s] %(name)s: %(message)s",
)
```

### Logger objects (best practice)

```python
import logging

logger = logging.getLogger(__name__)
logger.setLevel(logging.DEBUG)

# Handler: console
ch = logging.StreamHandler()
ch.setLevel(logging.INFO)
ch.setFormatter(logging.Formatter("%(levelname)s: %(message)s"))

# Handler: file
fh = logging.FileHandler("app.log")
fh.setLevel(logging.DEBUG)
fh.setFormatter(logging.Formatter("%(asctime)s [%(levelname)s] %(message)s"))

logger.addHandler(ch)
logger.addHandler(fh)

logger.info("Application started")
```

---

## The `argparse` module

```python
import argparse

parser = argparse.ArgumentParser(description="Process some files.")

parser.add_argument("input", help="Input file path")
parser.add_argument("-o", "--output", help="Output file path", default="output.txt")
parser.add_argument("-v", "--verbose", action="store_true", help="Enable verbose output")
parser.add_argument("-n", "--count", type=int, default=1, help="Number of times to process")

args = parser.parse_args()

if args.verbose:
    print(f"Processing {args.input}...")

for i in range(args.count):
    print(f"Pass {i+1}: {args.input} -> {args.output}")
```

```bash
python3 script.py data.txt -o results.txt -v -n 3
```

### Argument types

```python
parser.add_argument("--name", type=str)          # String (default)
parser.add_argument("--age", type=int)            # Integer
parser.add_argument("--rate", type=float)         # Float
parser.add_argument("--enabled", action="store_true")  # Boolean flag
parser.add_argument("--color", choices=["red", "green", "blue"])  # Choices
parser.add_argument("files", nargs="+")           # One or more values
parser.add_argument("--verbose", action="count")  # -v, -vv, -vvv
```

### Subcommands

```python
parser = argparse.ArgumentParser()
subparsers = parser.add_subparsers(dest="command")

# add command
add_parser = subparsers.add_parser("add")
add_parser.add_argument("x", type=int)
add_parser.add_argument("y", type=int)

# sub command
sub_parser = subparsers.add_parser("sub")
sub_parser.add_argument("x", type=int)
sub_parser.add_argument("y", type=int)

args = parser.parse_args()

if args.command == "add":
    print(args.x + args.y)
elif args.command == "sub":
    print(args.x - args.y)
```

---

## Combining logging + argparse

```python
import argparse
import logging

def setup_logging(verbose):
    level = logging.DEBUG if verbose else logging.INFO
    logging.basicConfig(
        level=level,
        format="%(asctime)s [%(levelname)s] %(message)s",
    )

def main():
    parser = argparse.ArgumentParser(description="Process data")
    parser.add_argument("input", help="Input file")
    parser.add_argument("-o", "--output", default="out.txt")
    parser.add_argument("-v", "--verbose", action="store_true")
    args = parser.parse_args()

    setup_logging(args.verbose)
    logging.info(f"Starting with input: {args.input}")
    # ... process ...
    logging.info("Finished")

if __name__ == "__main__":
    main()
```

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| `logging.basicConfig()` | Quick setup for logging |
| Log levels | `DEBUG`, `INFO`, `WARNING`, `ERROR`, `CRITICAL` |
| Logger objects | Best practice — `logger = logging.getLogger(__name__)` |
| Handlers | Route logs to console, file, network, etc. |
| `argparse.ArgumentParser` | Define CLI arguments |
| `add_argument()` | Positional, optional, flags, types, choices |
| `parse_args()` | Parse sys.argv into a namespace |
| Verbose flag | `-v` / `--verbose` with `action="store_true"` |
