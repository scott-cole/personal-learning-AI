# DRILL — Tree Recursion

## Task

Implement max depth, balanced check, and invert tree.

## Starter Code

Create `main.go`:

```go
package main

import (
    "fmt"
    "math"
)

type TreeNode struct {
    Value int
    Left  *TreeNode
    Right *TreeNode
}

// TODO: Return the maximum depth of the tree.
func MaxDepth(root *TreeNode) int {
    return 0
}

// TODO: Return true if the tree is height-balanced.
func IsBalanced(root *TreeNode) bool {
    return false
}

// TODO: Invert the tree (swap left and right children).
func InvertTree(root *TreeNode) *TreeNode {
    return nil
}

// Helper to print preorder
func Preorder(root *TreeNode) {
    if root == nil {
        return
    }
    fmt.Print(root.Value, " ")
    Preorder(root.Left)
    Preorder(root.Right)
}

func main() {
    // Tree 1:
    //     3
    //    / \
    //   9  20
    //      / \
    //     15  7
    t1 := &TreeNode{Value: 3}
    t1.Left = &TreeNode{Value: 9}
    t1.Right = &TreeNode{Value: 20}
    t1.Right.Left = &TreeNode{Value: 15}
    t1.Right.Right = &TreeNode{Value: 7}

    fmt.Println("Max depth:", MaxDepth(t1))
    fmt.Println("Is balanced:", IsBalanced(t1))

    fmt.Print("Before invert: ")
    Preorder(t1)
    fmt.Println()

    InvertTree(t1)

    fmt.Print("After invert:  ")
    Preorder(t1)
    fmt.Println()

    // Tree 2: degenerate
    t2 := &TreeNode{Value: 1}
    t2.Left = &TreeNode{Value: 2}
    t2.Left.Left = &TreeNode{Value: 3}
    fmt.Println("Max depth:", MaxDepth(t2))
    fmt.Println("Is balanced:", IsBalanced(t2))
}
```

## Expected Output

```
Max depth: 3
Is balanced: true
Before invert: 3 9 20 15 7
After invert:  3 20 7 15 9
Max depth: 3
Is balanced: false
```

## Hints

- MaxDepth: `1 + max(left, right)`.
- IsBalanced: compute height and balance in one pass — return -1 for unbalanced.
- InvertTree: swap children, recurse left and right.

## Run

```bash
go run main.go
```
