# DAY20 Checkpoint

## Questions

1. What command creates a virtual environment named `venv`?
2. How do you activate a virtual environment on macOS/Linux?
3. How do you deactivate a virtual environment?
4. What does `pip freeze > requirements.txt` do?
5. Why should you use a virtual environment instead of installing packages globally?

<details>
<summary>Answers</summary>

1. `python3 -m venv venv`
2. `source venv/bin/activate`
3. `deactivate`
4. It writes the list of all installed packages and their exact versions to `requirements.txt`.
5. To isolate project dependencies so different projects can use different versions of the same package without conflicts.

</details>
