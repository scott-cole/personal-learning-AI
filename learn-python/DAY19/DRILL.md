# DAY19 Drill — The Metal Detector

## Task

Write a Python script (`drill.py`) that uses regular expressions to process text data.

### Setup

```python
log_data = """
2024-01-15 08:23:45 ERROR User authentication failed for user 'alice'
2024-01-15 08:24:12 INFO User 'bob' logged in successfully
2024-01-15 08:25:01 WARN Disk space at 85%
2024-01-15 08:26:33 ERROR Database connection timeout
2024-01-15 08:27:15 INFO User 'alice' logged in successfully
2024-01-15 08:28:00 ERROR User authentication failed for user 'charlie'
2024-01-15 08:29:44 INFO User 'charlie' logged in successfully
2024-01-15 08:30:12 WARN Memory usage at 92%
2024-01-15 08:31:00 ERROR Service 'payment' unreachable
"""
```

### Requirements

1. **Count errors** — Use `findall()` to count lines with `ERROR`
2. **Extract usernames** — Use `findall()` with a group to extract all usernames (the text between single quotes)
3. **Warnings list** — Use `finditer()` to print each WARN line with its position
4. **Redact usernames** — Use `sub()` to replace usernames with `[REDACTED]`
5. **Date extraction** — Use `search()` with named groups to parse the first date into year, month, day

### Expected output

```
=== ERROR COUNT ===
Total ERROR lines: 4
=== USERNAMES ===
Usernames found: ['alice', 'bob', 'alice', 'charlie', 'charlie']
=== WARNINGS (with positions) ===
Line at 151: 2024-01-15 08:25:01 WARN Disk space at 85%
Line at 200: 2024-01-15 08:30:12 WARN Memory usage at 92%
=== REDACTED LOG (first 3 lines) ===
2024-01-15 08:23:45 ERROR User authentication failed for user '[REDACTED]'
2024-01-15 08:24:12 INFO User '[REDACTED]' logged in successfully
2024-01-15 08:25:01 WARN Disk space at 85%
=== DATE PARSING ===
First date: 2024-01-15
Year: 2024, Month: 01, Day: 15
```

### Hints

- `re.findall(r"'(\w+)'", log_data)` captures usernames
- `re.sub(r"'\w+'", "'[REDACTED]'", log_data)` for redaction
- Named group: `r"(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})"`
- `re.finditer(r"^.*WARN.*$", log_data, re.MULTILINE)` for warnings

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] Count ERROR lines with `findall`
- [ ] Extract usernames with a capturing group
- [ ] Iterate over WARN lines with `finditer`
- [ ] Redact usernames with `sub`
- [ ] Parse date with named groups
