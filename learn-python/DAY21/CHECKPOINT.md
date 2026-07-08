# DAY21 Checkpoint

## Questions

1. What library is used to send HTTP requests in Python?
2. How do you extract the visible text from a BeautifulSoup tag?
3. What method finds all HTML elements matching a tag name and class?
4. What is `response.raise_for_status()` used for?
5. Name two ethical considerations when scraping a website.

<details>
<summary>Answers</summary>

1. `requests` (or the standard library's `urllib`).
2. `.text` (or `.get_text()`).
3. `.find_all("tag", class_="classname")` or `.select("tag.classname")`.
4. It raises an `HTTPError` if the response status code is 4xx or 5xx, so you don't accidentally process error pages.
5. Check `robots.txt`, respect rate limits (add delays), identify with a User-Agent, and check terms of service.

</details>
