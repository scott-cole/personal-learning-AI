# DAY01: Drill — "How old are you, really?"

Write a Go program from scratch. Create a file called `main.go` inside the `DAY01/` folder.

## Requirements

```go
package main

import "fmt"

func main() {
    // 1. Store your name in a variable
    // 2. Store your age in a variable
    // 3. Calculate your approximate age in days (age * 365)
    // 4. Print: "Hello, I'm [name] and I'm about [days] days old!"
}
```

## Example output

```
Hello, I'm Scott and I'm about 10950 days old!
```

## Hints (try without these first)

<details>
<summary>Click for hints</summary>

- Use `:=` for all your variables
- Your age in days is `age * 365`
- Use `fmt.Printf` with `%s` for the name and `%d` for the number
- A newline is `\n`
</details>

## Bonus (if you finish early)

Add a line that also prints your age in **hours**:

```
I'm approximately 262800 hours old!
```

## How to run

```bash
cd ~/Dev/learn-go/DAY01
go run main.go
```

## Before you ask for review

- Does it compile? (`go run main.go` with no errors)
- Does the output look right?
- Try changing the values — does it still work?

## When you're done

Show me your `main.go` file. I'll review every line.
