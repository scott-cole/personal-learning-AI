# DAY02: Drill — "The Age Calculator, Refactored"

Take your DAY01 program and **refactor it** into functions.

## Requirements

Write a new `main.go` in `DAY02/` with this structure:

```go
package main

import "fmt"

func main() {
    name := "Scott"
    age := 30

    days := ageToDays(age)
    printGreeting(name, days)
}

// ageToDays converts years to approximate days
func ageToDays(years int) int {
    // TODO: implement
}

// printGreeting prints the greeting message
func printGreeting(name string, days int) {
    // TODO: implement
}
```

## Rules

- `ageToDays` receives years, returns days
- `printGreeting` receives name and days, prints the message, returns nothing
- `main` calls both functions

## Bonus

Add a third function that validates the age:

```go
func validateAge(age int) error {
    if age <= 0 {
        return errors.New("age must be positive")
    }
    if age > 150 {
        return errors.New("age seems unrealistic")
    }
    return nil  // nil = no error
}
```

Call it from `main` and check the error before proceeding:

```go
err := validateAge(age)
if err != nil {
    fmt.Println("Error:", err)
    return  // stops the program
}
```

## Expected output

Same as DAY01:

```
Hello, I'm Scott and I'm about 10950 days old!
```

If you add the validation bonus, an invalid age (like `-5`) should print:

```
Error: age must be positive
```

## Run it

```bash
cd ~/Dev/learn-go/DAY02
go run main.go
```
