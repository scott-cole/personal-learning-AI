# DAY06: Drill — "The Counter"

You're building a counter that multiple functions can modify.

## Requirements

Write a `main.go` in `DAY06/`:

```go
package main

import "fmt"

// 1. increment adds 1 to the counter. Must modify the original.
func increment(counter *int) {
    // TODO
}

// 2. reset sets the counter to 0. Must modify the original.
func reset(counter *int) {
    // TODO
}

// 3. createCounter returns a pointer to a new int initialized to 0
func createCounter() *int {
    // TODO
}

func main() {
    // Create a counter using createCounter
    c := createCounter()
    
    fmt.Println("Initial:", *c)
    
    increment(c)
    fmt.Println("After increment:", *c)
    
    increment(c)
    fmt.Println("After another increment:", *c)
    
    reset(c)
    fmt.Println("After reset:", *c)
}
```

## Expected output

```
Initial: 0
After increment: 1
After another increment: 2
After reset: 0
```

## Bonus: The Swap

Write a function `swap(a, b *int)` that swaps the values of two integers:

```go
x, y := 10, 20
swap(&x, &y)
fmt.Println(x, y) // 20 10
```

## Why this matters

This is exactly how the Monzo exercise works with pointers to accounts:
```go
s.accounts[id] = &Account{Balance: 10000}
acc.Balance += tx.Amount   // modifies the original, not a copy
```

## Run it

```bash
cd ~/Dev/learn-go/DAY06
go run main.go
```
