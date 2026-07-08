# DAY30 Drill — Polish the URL Shortener

## Task

Add tests, error handling, logging, and a requirements.txt to the Day 29 URL shortener.

### Step 1: Add validation

In `app.py`, add a `validate_url()` function that:

- Raises `ValueError` if the URL is empty
- Raises `ValueError` if it doesn't start with `http://` or `https://`
- Returns the trimmed URL otherwise

### Step 2: Add logging

Configure a logger that outputs to the console with format:

```python
import logging
logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] %(message)s",
    datefmt="%H:%M:%S"
)
logger = logging.getLogger(__name__)
```

Add log calls:
- `INFO` when a URL is shortened
- `INFO` when a redirect happens
- `WARNING` when a short code is not found
- `ERROR` if the database operation fails

### Step 3: Write tests

Create `test_shortener.py` with:

1. **`test_home_page`** — GET `/` returns 200 with "URL Shortener" in the body
2. **`test_shorten_url`** — POST `/` with a valid URL returns 200 with short URL displayed
3. **`test_redirect`** — Create a short URL, then GET `/<code>` and verify redirect (302)
4. **`test_invalid_url`** — POST `/` with `"not-a-url"` returns error message
5. **`test_empty_url`** — POST `/` with empty URL returns error message
6. **`test_unknown_code`** — GET `/nonexistent` returns 404

### Step 4: Create `requirements.txt`

```
flask>=3.0
pytest>=8.0
pytest-cov>=5.0
```

### Expected output

```
$ pytest test_shortener.py -v
==================== test session starts ====================
test_shortener.py::test_home_page PASSED
test_shortener.py::test_shorten_url PASSED
test_shortener.py::test_redirect PASSED
test_shortener.py::test_invalid_url PASSED
test_shortener.py::test_empty_url PASSED
test_shortener.py::test_unknown_code PASSED
==================== 6 passed in 0.XXs =====================
```

### Running the app with logging:

```
$ python3 app.py
14:30:01 [INFO] Starting URL Shortener
14:30:05 [INFO] Home page accessed
14:30:10 [INFO] Received URL: https://example.com/very/long/path
14:30:10 [INFO] Created short code: aB3xYz
14:30:15 [INFO] Redirect request for code: aB3xYz
14:30:15 [INFO] Redirecting aB3xYz -> https://example.com/very/long/path
```

### Hints

- Use `app.app.test_client()` for Flask test client
- Use `with app.app.app_context():` to initialize DB in tests
- Use a separate test DB file and clean it up in a fixture's teardown
- For `test_redirect`, check `rv.status_code == 302` and `rv.headers["Location"]`

### How to run

```bash
pip install -r requirements.txt
pytest test_shortener.py -v
pytest --cov=app test_shortener.py -v
```

## TODOs

- [ ] Add `validate_url()` with proper error messages
- [ ] Integrate validation into the Flask route
- [ ] Configure logging with format and levels
- [ ] Add log calls in all routes
- [ ] Write tests for all 6 cases
- [ ] Create `requirements.txt`
- [ ] Run tests and verify all pass
- [ ] Run the app and verify logging output
