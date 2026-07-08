# DAY30 Checkpoint

## Questions

1. What does `app.app.test_client()` provide in Flask testing?
2. Why use a separate test database instead of the production one?
3. What is the difference between `logging.INFO` and `logging.WARNING`?
4. What is the purpose of `requirements.txt` in a project?
5. Why is it important to validate URLs before storing them in a URL shortener?

<details>
<summary>Answers</summary>

1. It creates a test client that can make requests to the Flask app without running a server.
2. To isolate tests from real data — test data should never mix with production data, and the test DB can be safely deleted after tests run.
3. `INFO` is for general operational messages (things are working); `WARNING` is for unexpected events that aren't errors (e.g., a missing short code).
4. It lists all dependencies with version constraints so anyone can install them with a single `pip install -r requirements.txt` command.
5. To prevent invalid data in the database, protect against malicious input (e.g., JavaScript URLs that could cause XSS), and provide clear feedback to users.

</details>
