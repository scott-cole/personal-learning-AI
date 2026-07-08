# DAY09 Drill — The Library System

## Task

Write a Python script (`drill.py`) that acts as a simple library catalogue manager using file I/O.

### Requirements

1. **Create a catalogue** — Write a list of books to a file called `catalogue.txt` in the format `"Title by Author"`, one per line
2. **Read and display** — Read the file and print each line with a line number
3. **Add a book** — Append a new book to the file
4. **Search** — Ask the user for a search term, read the file, and print lines that contain the term (case-insensitive)
5. **Count** — Print the total number of books in the catalogue
6. **Backup** — After modifying the catalogue, create a backup copy called `catalogue_backup.txt`

### Starting catalogue

Use this list to create the initial file:

```python
initial_books = [
    "The Pragmatic Programmer by Andrew Hunt",
    "Clean Code by Robert C. Martin",
    "Python Crash Course by Eric Matthes",
    "Fluent Python by Luciano Ramalho",
    "Automate the Boring Stuff by Al Sweigart",
]
```

### Expected output (after adding "Design Patterns by Gang of Four")

```
=== CATALOGUE ===
1. The Pragmatic Programmer by Andrew Hunt
2. Clean Code by Robert C. Martin
3. Python Crash Course by Eric Matthes
4. Fluent Python by Luciano Ramalho
5. Automate the Boring Stuff by Al Sweigart
6. Design Patterns by Gang of Four
Total books: 6
Search term: python
Matches:
  - Python Crash Course by Eric Matthes
  - Fluent Python by Luciano Ramalho
Backup created: catalogue_backup.txt
```

### Hints

- Use `"w"` mode to create the initial file
- Use `"a"` mode to append a book
- Use `in` for substring matching: `search_term.lower() in line.lower()`
- Use `shutil.copy()` or manual read/write for backup

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] Write initial book list to `catalogue.txt`
- [ ] Read and display with line numbers
- [ ] Append a new book
- [ ] Search with case-insensitive matching
- [ ] Count and report total books
- [ ] Create a backup file
