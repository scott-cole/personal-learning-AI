# DAY 14 — Project: Browser Navigation with Two Stacks

## Back and Forward

A browser's back and forward buttons are implemented with two stacks. When you visit a page, the current page is pushed onto the back stack. When you click "back," the current page is pushed onto the forward stack and the top of the back stack becomes the current page. When you click "forward," the reverse happens. If you visit a new page after going back, the forward stack is cleared.

This week you learned stacks and queues. Now combine them to build a simulated browser history.

## Design

```
Current: Page C
Back:    [A, B]
Forward: [D, E]

Click Back:
  Current: Page B
  Back:    [A]
  Forward: [C, D, E]

Visit new page F:
  Current: Page F
  Back:    [A, B, C]
  Forward: []   ← cleared!
```

## Interface

```go
type Browser struct {
    current  string
    back     []string  // stack
    forward  []string  // stack
}

func NewBrowser(homepage string) *Browser
func (b *Browser) Visit(url string)
func (b *Browser) Back() (string, bool)
func (b *Browser) Forward() (string, bool)
func (b *Browser) Current() string
```

## Summary

This is a real-world use of stacks. Browser navigation, undo/redo in editors, and command history all use the same two-stack pattern.
