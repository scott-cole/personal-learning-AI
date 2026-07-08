# DAY19 Checkpoint

## Questions

1. What is the difference between `re.search()` and `re.match()`?
2. What does `r"\d{3}-\d{2}-\d{4}"` match?
3. How do you capture a specific part of a match and refer to it later?
4. What does `re.sub(r"\d+", "N", "a1b22c333", count=2)` return?
5. Why should you use raw strings (`r"..."`) for regex patterns?

<details>
<summary>Answers</summary>

1. `re.search()` looks for a match anywhere in the string; `re.match()` only looks at the beginning.
2. A pattern like `123-45-6789` — three digits, a hyphen, two digits, a hyphen, four digits (e.g., a US SSN format).
3. Use parentheses `()` to create a capturing group, then access with `.group(1)`, `.group(2)`, etc.
4. `"aN bN c333"` — the first two numbers are replaced, the third (with count=2) is left alone.
5. Because backslashes (`\d`, `\s`, etc.) would otherwise need to be escaped as `\\d`, `\\s`, making patterns hard to read.

</details>
