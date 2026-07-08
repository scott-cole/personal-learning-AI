# DAY09 Checkpoint

## Questions

1. What does the `"w"` mode do if the file already exists?
2. Why should you always use `with open(...)` instead of just `open()`?
3. What is the difference between `f.read()` and `f.readlines()`?
4. In what mode would you open a file to add content to the end without erasing existing content?
5. What does `Path("data/file.txt").suffix` return?

<details>
<summary>Answers</summary>

1. It truncates the file — overwrites it completely.
2. `with` guarantees the file is closed even if an exception occurs; without it, forgetting to call `.close()` can leak file descriptors.
3. `read()` returns the entire content as a single string; `readlines()` returns a list of strings, one per line.
4. `"a"` (append mode).
5. `".txt"` — the file extension.

</details>
