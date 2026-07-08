# DAY19 — Regular Expressions

## Anecdote: Regex is a metal detector for text

Imagine a wide, sandy beach. Buried somewhere beneath the sand is a silver coin. You could dig randomly with your hands (string methods), but that would take forever. Instead, you pick up a metal detector (regex), wave it across the sand, and listen for the beep. The beep tells you exactly where to dig. You can tune the detector to ignore bottle caps (match only digits) and find rings too (capture groups).

Regular expressions are that metal detector. You describe a *pattern* of characters — `r"\d{3}-\d{4}"` for a phone number — and Python scans the text for you. When it finds a match, it beeps and hands you the coordinates.

---

## The `re` module

```python
import re
```

---

## `re.search()` — find first match

```python
text = "Call me at 555-1234 or 555-5678"
match = re.search(r"\d{3}-\d{4}", text)

if match:
    print(match.group())      # 555-1234
    print(match.start())      # 12
    print(match.end())        # 20
```

Returns a `Match` object or `None`.

---

## `re.match()` — match at the beginning

```python
print(re.match(r"\d+", "123abc"))    # <match '123'>
print(re.match(r"\d+", "abc123"))    # None
```

---

## `re.findall()` — all matches as a list

```python
text = "emails: alice@test.com, bob@test.org, charlie@test.net"
emails = re.findall(r"\w+@\w+\.\w+", text)
print(emails)  # ['alice@test.com', 'bob@test.org', 'charlie@test.net']
```

---

## `re.finditer()` — iterator of match objects

```python
for m in re.finditer(r"\d+", "a1 b22 c333"):
    print(m.group(), m.start(), m.end())
# 1 1 2
# 22 4 6
# 333 8 11
```

---

## `re.sub()` — find and replace

```python
text = "My phone is 555-1234"
result = re.sub(r"\d{3}-\d{4}", "[REDACTED]", text)
print(result)  # My phone is [REDACTED]

# With count limit
result2 = re.sub(r"\d", "X", "a1b2c3", count=2)
print(result2)  # aXbXc3
```

---

## Common patterns

| Pattern | Matches |
|---------|---------|
| `.` | Any character except newline |
| `\d` | Any digit (`0-9`) |
| `\w` | Any word character (letter, digit, underscore) |
| `\s` | Any whitespace (space, tab, newline) |
| `\b` | Word boundary |
| `^` | Start of string |
| `$` | End of string |
| `*` | Zero or more |
| `+` | One or more |
| `?` | Zero or one |
| `{n}` | Exactly n |
| `{n,}` | n or more |
| `{n,m}` | Between n and m |
| `[abc]` | Any of a, b, or c |
| `[a-z]` | Any lowercase letter |
| `[^abc]` | Not a, b, or c |
| `\|` | Alternation (OR) |
| `(...)` | Capturing group |
| `(?:...)` | Non-capturing group |

---

## Capturing groups

```python
text = "Name: Alice, Age: 28, City: London"
match = re.search(r"Name: (\w+), Age: (\d+)", text)
if match:
    print(match.group(0))    # Name: Alice, Age: 28
    print(match.group(1))    # Alice
    print(match.group(2))    # 28
    print(match.groups())    # ('Alice', '28')
```

### Named groups

```python
match = re.search(r"Name: (?P<name>\w+), Age: (?P<age>\d+)", text)
print(match.group("name"))  # Alice
print(match.group("age"))   # 28
```

---

## `re.compile()` — reuse a pattern

```python
phone_pattern = re.compile(r"\d{3}-\d{4}")
matches = phone_pattern.findall("555-1234, 555-5678")
print(matches)  # ['555-1234', '555-5678']
```

Compiled patterns are faster when used repeatedly.

---

## Raw strings (`r"..."`)

Always use raw strings for regex patterns to avoid escaping backslashes:

```python
# Without raw string — painful
p = "\\d{3}-\\d{4}"

# With raw string — clean
p = r"\d{3}-\d{4}"
```

---

## Flags

```python
# Case-insensitive
re.findall(r"python", "Python PYTHON python", re.IGNORECASE)

# Multiline — ^ and $ match line boundaries
re.search(r"^hello", "first\nhello", re.MULTILINE)

# Dot matches newline
re.search(r"a.b", "a\nb", re.DOTALL)
```

---

## Summary

| Function | What it does |
|----------|-------------|
| `re.search()` | First match anywhere in string |
| `re.match()` | Match at the beginning |
| `re.findall()` | List of all matches |
| `re.finditer()` | Iterator of match objects |
| `re.sub()` | Find and replace |
| `re.compile()` | Pre-compile a pattern for reuse |
| Groups | `()` for capturing; `(?P<name>)` for named |
| Flags | `re.IGNORECASE`, `re.MULTILINE`, `re.DOTALL` |
