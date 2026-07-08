# DAY22 Checkpoint

## Questions

1. What Python data types do JSON arrays and objects map to?
2. How do you write a Python object to a JSON file?
3. What is the difference between `json.dumps()` and `json.dump()`?
4. Which Python module is needed for YAML, and how do you install it?
5. Why would you choose YAML over JSON for a configuration file?

<details>
<summary>Answers</summary>

1. JSON arrays → Python `list`; JSON objects → Python `dict`.
2. `with open("file.json", "w") as f: json.dump(data, f, indent=2)`
3. `dumps()` returns a string; `dump()` writes directly to a file object.
4. The `pyyaml` package: `pip install pyyaml`
5. YAML supports comments, is more human-readable for nested configs, and doesn't require quotes around keys.

</details>
