# DAY23 Checkpoint

## Questions

1. What does `@app.route("/user/<int:uid>")` do?
2. Where does Flask look for HTML templates by default?
3. How do you access URL query parameters (e.g., `?q=hello`) in Flask?
4. What is the purpose of `base.html` with `{% block content %}{% endblock %}`?
5. What does `app.run(debug=True)` do?

<details>
<summary>Answers</summary>

1. It maps URLs like `/user/42` to the decorated function, converting `42` to an integer and passing it as the `uid` argument.
2. In a `templates/` directory in the same folder as the app.
3. `request.args.get("q")` — the `request` object is imported from `flask`.
4. It provides template inheritance — child templates override blocks to fill in page-specific content while sharing the common layout.
5. It enables debug mode: auto-reload on code changes and a detailed error page when exceptions occur.

</details>
