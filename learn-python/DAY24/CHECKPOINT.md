# DAY24 Checkpoint

## Questions

1. What HTTP method is typically used to create a new resource via REST?
2. What does `jsonify()` do in Flask?
3. How do you parse a JSON request body in a Flask view?
4. What HTTP status code should a successful `POST` return?
5. What does returning a tuple `(dict, 404)` from a Flask view do?

<details>
<summary>Answers</summary>

1. `POST`
2. It converts a Python dict (or list) into a `Response` object with `Content-Type: application/json`.
3. `data = request.get_json()` — this parses the incoming JSON body into a Python dict.
4. `201 Created`
5. It returns a JSON response (the dict) with the specified HTTP status code (404).

</details>
