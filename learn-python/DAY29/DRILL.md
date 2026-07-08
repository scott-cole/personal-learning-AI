# DAY29 Drill — Build the URL Shortener

## Task

Build a complete URL shortener with Flask and SQLite.

### Structure

```
url_shortener/
├── app.py
└── templates/
    ├── index.html
    └── stats.html
```

### Requirements

1. **Home page** (`/`) — Form to submit a long URL
2. **Shorten endpoint** (`POST /`) — Generate short code and display the short URL
3. **Redirect** (`GET /<short_code>`) — Look up and redirect (302) to the original URL
4. **Stats page** (`/stats`) — Table of all URLs with short code, original URL, visits, and creation date
5. **Validation** — Ensure the submitted URL starts with `http://` or `https://`; show an error if not
6. **Duplicate handling** — If the same URL is submitted twice, return the existing short code instead of creating a new one
7. **404** — Show appropriate message for unknown short codes

### Bonus features (optional)

- Add a "Copy to clipboard" button for the short URL
- Add click count sorting on the stats page
- Use `string.ascii_letters + string.digits` for the short code (62 characters × 6 positions = ~56 billion combinations)

### Hints

- Use `request.form.get("url")` to get the submitted URL
- Check for existing URL: `SELECT short_code FROM urls WHERE original_url = ?`
- `redirect(url, code=302)` for the redirect
- `url_for("redirect_url", short_code=code, _external=True)` for the full short URL

### How to run

```bash
pip install flask
python3 app.py
# Open http://127.0.0.1:5000
```

## TODOs

- [ ] Set up Flask app and SQLite database
- [ ] Create database initialization function
- [ ] Implement `create_short_url()` with unique code generation
- [ ] Implement `get_original_url()` with visit counting
- [ ] Implement duplicate URL detection
- [ ] Create `index.html` template
- [ ] Create `stats.html` template
- [ ] Implement all routes
- [ ] Add URL validation
- [ ] Test end-to-end
