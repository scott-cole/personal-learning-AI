# DRILL — Binary Tree Traversals

## Task

Build a binary tree and implement all four traversal methods.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

type TreeNode struct {
    Value int
    Left  *TreeNode
    Right *TreeNode
}

// TODO: Implement preorder traversal (root → left → right).
func Preorder(root *TreeNode) {
}

// TODO: Implement inorder traversal (left → root → right).
func Inorder(root *TreeNode) {
}

// TODO: Implement postorder traversal (left → right → root).
func Postorder(root *TreeNode) {
}

// TODO: Implement level-order traversal (BFS using a queue).
func LevelOrder(root *TreeNode) {
}

func main() {
    // Build:
    //       1
    //      / \
    //     2   3
    //    / \   \
    //   4   5   6
    root := &TreeNode{Value: 1}
    root.Left = &TreeNode{Value: 2}
    root.Right = &TreeNode{Value: 3}
    root.Left.Left = &TreeNode{Value: 4}
    root.Left.Right = &TreeNode{Value: 5}
    root.Right.Right = &TreeNode{Value: 6}

    fmt.Print("Preorder:   ")
    Preorder(root)
    fmt.Println()

    fmt.Print("Inorder:    ")
    Inorder(root)
    fmt.Println()

    fmt.Print("Postorder:  ")
    Postorder(root)
    fmt.Println()

    fmt.Print("LevelOrder: ")
    LevelOrder(root)
    fmt.Println()
}
```

## Expected Output

```
Preorder:   1 2 4 5 3 6
Inorder:    4 2 5 1 3 6
Postorder:  4 5 2 6 3 1
LevelOrder: 1 2 3 4 5 6
```

## Hints

- Depth-first traversals are naturally recursive.
- Level-order needs a queue (slice). Dequeue from front, enqueue children.
- Base case: `if root == nil { return }`.

## Run

```bash
go run main.go
```
