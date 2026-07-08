# DAY21 Drill — Build the Web Scraper

## Task

Write a Python script (`scraper.py`) that scrapes quotes from `http://quotes.toscrape.com` and prints them in a formatted way.

### Requirements

1. Fetch the first page of `http://quotes.toscrape.com`
2. Parse the HTML with BeautifulSoup
3. For each quote, extract:
   - The quote text (inside `<span class="text">`)
   - The author (inside `<small class="author">`)
   - The tags (inside `<a class="tag">` elements)
4. Print each quote in the format below
5. Print a summary at the end

### Expected output

```
=== QUOTES.TOSCRAPE.COM ===
Page 1 of 10

"“The world as we have created it is a process of our thinking. It cannot be changed without changing our thinking.”"
— Albert Einstein
Tags: change, deep-thoughts, thinking, world

"“It is our choices, Harry, that show what we truly are, far more than our abilities.”"
— J.K. Rowling
Tags: abilities, choices

... (all quotes on page 1)

=== SUMMARY ===
Quotes found: 10
Authors: Albert Einstein, J.K. Rowling, ... (unique authors)

=== AUTHORS ===
Albert Einstein: 1 quote(s)
J.K. Rowling: 1 quote(s)
... (count per author)
```

### Bonus (optional)

- Scrape all 10 pages
- Save the quotes to a JSON file
- Add a `--page N` CLI argument via `sys.argv`

### Hints

- `soup.find_all("div", class_="quote")` finds all quote blocks
- `.find("span", class_="text").text` for the quote text
- `.find("small", class_="author").text` for the author
- Use `collections.Counter` to count authors
- `requests.get(url, timeout=10)` to avoid hanging

### How to run

```bash
pip install requests beautifulsoup4
python3 scraper.py
```

## TODOs

- [ ] Fetch the page with `requests.get()`
- [ ] Parse with `BeautifulSoup`
- [ ] Extract quote text, author, and tags
- [ ] Print formatted output
- [ ] Count unique authors and print summary
- [ ] (Bonus) Scrape all pages and save to JSON
