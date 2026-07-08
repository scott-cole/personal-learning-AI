# DRILL — Two Pointers

## Task

Solve three problems using the two-pointer technique.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

// TODO: Return indices of the two numbers that add up to target.
// The input slice is sorted in ascending order.
func TwoSum(nums []int, target int) (int, int) {
    return -1, -1
}

// TODO: Return true if s is a palindrome (ignore case and non-alphanumeric).
func IsPalindrome(s string) bool {
    return false
}

// TODO: Move all zeros to the end while preserving relative order of non-zero elements.
func MoveZeroes(nums []int) {
}

func main() {
    // Two Sum
    nums1 := []int{2, 7, 11, 15}
    i, j := TwoSum(nums1, 9)
    fmt.Printf("TwoSum: (%d, %d)\n", i, j)

    // Palindrome
    fmt.Println("Is 'racecar' a palindrome?", IsPalindrome("racecar"))
    fmt.Println("Is 'hello' a palindrome?", IsPalindrome("hello"))

    // Move Zeroes
    nums2 := []int{0, 1, 0, 3, 12}
    MoveZeroes(nums2)
    fmt.Println("After move zeroes:", nums2)
}
```

## Expected Output

```
TwoSum: (0, 1)
Is 'racecar' a palindrome? true
Is 'hello' a palindrome? false
After move zeroes: [1 3 12 0 0]
```

## Hints

- TwoSum: one pointer at start, one at end. Move inward based on sum.
- Palindrome: skip non-alphanumeric with `unicode.IsLetter`/`unicode.IsDigit`.
- MoveZeroes: use a slow pointer to track where the next non-zero goes.

## Run

```bash
go run main.go
```
