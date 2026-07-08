# DAY21 — Project: Web Scraper

## Anecdote: Your third workshop build

You've built a stool and a tool rack. Now it's time to build a **radio** — a device that pulls signals from the air and turns them into something you can hear.

A web scraper is a radio for the internet. It sends a request (tunes the dial), receives HTML (captures the signal), and parses it (turns it into sound). Today you'll build a scraper that fetches a webpage and extracts specific information.

---

## Installing the tools

```bash
pip install requests beautifulsoup4
```

| Library | Purpose |
|---------|---------|
| `requests` | Send HTTP requests and receive responses |
| `beautifulsoup4` | Parse HTML and navigate the DOM tree |

---

## `requests` basics

```python
import requests

response = requests.get("https://example.com")
print(response.status_code)       # 200
print(response.text)              # Full HTML as a string
print(response.headers)           # Response headers
```

### Common patterns

```python
# With query parameters
params = {"q": "python", "page": 2}
response = requests.get("https://search.example.com", params=params)

# With headers (e.g., User-Agent to avoid blocking)
headers = {"User-Agent": "Mozilla/5.0"}
response = requests.get("https://site.com", headers=headers)

# POST with data
response = requests.post("https://api.example.com/login", data={"user": "admin", "pass": "secret"})

# Error handling
response.raise_for_status()  # Raises HTTPError for 4xx/5xx
```

---

## BeautifulSoup basics

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(html_text, "html.parser")

# Find the first matching tag
title = soup.find("title")
print(title.text)         # The page title

# Find all matching tags
links = soup.find_all("a")
for link in links:
    print(link.get("href"))     # The href attribute
    print(link.text)            # The visible text

# CSS selectors
soup.select("div.content p")
soup.select("#main-header")
soup.select(".product-price")
soup.select("a[href^='https']")
```

---

## The project: Quote scraper

We'll scrape quotes from `http://quotes.toscrape.com` — a site designed for learning to scrape.

```python
import requests
from bs4 import BeautifulSoup

url = "http://quotes.toscrape.com"
response = requests.get(url)
soup = BeautifulSoup(response.text, "html.parser")

quotes = soup.find_all("div", class_="quote")

for quote in quotes:
    text = quote.find("span", class_="text").text
    author = quote.find("small", class_="author").text
    tags = [tag.text for tag in quote.find_all("a", class_="tag")]
    print(f"\"{text}\" — {author}")
    print(f"Tags: {', '.join(tags)}")
    print()
```

### Pagination

```python
def scrape_all_pages(base_url):
    page = 1
    all_quotes = []
    while True:
        url = f"{base_url}/page/{page}/"
        response = requests.get(url)
        if response.status_code != 200:
            break
        soup = BeautifulSoup(response.text, "html.parser")
        quotes = soup.find_all("div", class_="quote")
        if not quotes:
            break
        # Process quotes...
        page += 1
    return all_quotes
```

---

## Error handling

```python
import time

def fetch_with_retry(url, max_retries=3):
    for attempt in range(max_retries):
        try:
            response = requests.get(url, timeout=5)
            response.raise_for_status()
            return response
        except requests.RequestException as e:
            print(f"Attempt {attempt + 1} failed: {e}")
            if attempt < max_retries - 1:
                time.sleep(2)
    return None
```

---

## Ethics of scraping

- Check `robots.txt` (e.g., `http://example.com/robots.txt`)
- Respect rate limits — add `time.sleep()` between requests
- Identify yourself with a descriptive User-Agent
- Cache results to avoid redundant requests
- Check terms of service

---

## Summary

| Component | Purpose |
|-----------|---------|
| `requests.get()` | Fetch a webpage |
| `BeautifulSoup()` | Parse HTML into a traversable tree |
| `.find()` / `.find_all()` | Locate tags by name, class, id, etc. |
| `.select()` | Use CSS selectors |
| `.text` | Get the visible text content |
| `.get("href")` | Get an attribute value |
| Pagination | Loop through numbered pages |
| Ethics | Respect `robots.txt`, rate limits, and ToS |
