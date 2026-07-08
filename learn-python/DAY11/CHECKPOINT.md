# DAY11 Checkpoint

## Questions

1. What is the purpose of `if __name__ == "__main__"`?
2. How do you create a Python package (as opposed to a module)?
3. What command installs all dependencies listed in `requirements.txt`?
4. What does `from math import *` do, and why is it discouraged?
5. How can you see all directories where Python looks for modules?

<details>
<summary>Answers</summary>

1. It allows a file to be both importable (no side effects) and directly runnable (the guarded block executes).
2. Create a directory with an `__init__.py` file (can be empty).
3. `pip install -r requirements.txt`
4. It imports every name from `math` into the current namespace, which can silently overwrite existing names.
5. `import sys; print(sys.path)`

</details>
