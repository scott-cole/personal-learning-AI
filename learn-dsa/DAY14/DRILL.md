# DRILL — Browser Navigation

## Task

Implement a browser history system using two stacks.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

type Browser struct {
    current string
    back    []string
    forward []string
}

// TODO: Create a new browser with the given homepage.
func NewBrowser(homepage string) *Browser {
    return nil
}

// TODO: Visit a new URL. Clear the forward stack.
func (b *Browser) Visit(url string) {
}

// TODO: Go back one page. Return the new current page.
func (b *Browser) Back() (string, bool) {
    return "", false
}

// TODO: Go forward one page. Return the new current page.
func (b *Browser) Forward() (string, bool) {
    return "", false
}

func (b *Browser) Current() string {
    return b.current
}

func main() {
    b := NewBrowser("google.com")
    fmt.Println("Home:", b.Current())

    b.Visit("example.com")
    b.Visit("openai.com")
    b.Visit("github.com")
    fmt.Println("After visits:", b.Current())

    page, ok := b.Back()
    fmt.Printf("Back to %s (ok=%v)\n", page, ok)

    page, ok = b.Back()
    fmt.Printf("Back to %s (ok=%v)\n", page, ok)

    page, ok = b.Forward()
    fmt.Printf("Forward to %s (ok=%v)\n", page, ok)

    b.Visit("stackoverflow.com")
    fmt.Println("After new visit:", b.Current())

    _, ok = b.Forward()
    fmt.Println("Forward after new visit:", ok) // should be false
}
```

## Expected Output

```
Home: google.com
After visits: github.com
Back to openai.com (ok=true)
Back to example.com (ok=true)
Forward to openai.com (ok=true)
After new visit: stackoverflow.com
Forward after new visit: false
```

## Hints

- `Visit`: push current onto back, set new current, clear forward.
- `Back`: push current onto forward, pop from back to new current.
- `Forward`: push current onto back, pop from forward to new current.

## Run

```bash
go run main.go
```
