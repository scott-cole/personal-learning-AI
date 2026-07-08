# DAY05: Drill — "The Phone Book"

You're building a phone book using maps.

## Requirements

Write a `main.go` in `DAY05/`:

```go
package main

import "fmt"

func main() {
    // 1. Create a map[string]string for a phone book
    //    Keys: names, Values: phone numbers
    // 2. Add these contacts:
    //    Alice: 555-1234
    //    Bob:   555-5678
    //    Charlie: 555-9012
    // 3. Look up Bob's number and print it
    // 4. Look up "Eve" — she doesn't exist. Print a friendly message.
    // 5. Loop through all contacts and print:
    //    Alice: 555-1234
    //    Bob:   555-5678
    //    Charlie: 555-9012
    // 6. Delete Bob from the phone book
    // 7. Print the final phone book
}
```

## Expected output

```
Bob's number: 555-5678
Eve not found in phone book

All contacts:
Alice: 555-1234
Bob: 555-5678
Charlie: 555-9012

After deleting Bob:
Alice: 555-1234
Charlie: 555-9012
```

## Bonus 1

Add a function `addContact(phoneBook map[string]string, name, number string) error` that:
- Returns an error if the contact already exists
- Adds it otherwise

## Bonus 2

Add a function `findContact(phoneBook map[string]string, name string) (string, error)` that returns the number or an error if not found.

## Run it

```bash
cd ~/Dev/learn-go/DAY05
go run main.go
```
