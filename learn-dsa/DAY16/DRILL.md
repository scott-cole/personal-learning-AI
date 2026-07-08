# DRILL — Binary Search Tree

## Task

Implement a BST with insert, search, delete, min, and max.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

type BSTNode struct {
    Value int
    Left  *BSTNode
    Right *BSTNode
}

type BST struct {
    Root *BSTNode
}

// TODO: Insert value into the BST.
func (bst *BST) Insert(value int) {
}

// TODO: Return true if value exists in the BST.
func (bst *BST) Search(value int) bool {
    return false
}

// TODO: Delete the node with the given value.
func (bst *BST) Delete(value int) {
}

// TODO: Return the minimum value.
func (bst *BST) Min() (int, bool) {
    return 0, false
}

// TODO: Return the maximum value.
func (bst *BST) Max() (int, bool) {
    return 0, false
}

// Inorder helper for printing
func (bst *BST) Inorder() {
    var inorder func(node *BSTNode)
    inorder = func(node *BSTNode) {
        if node == nil {
            return
        }
        inorder(node.Left)
        fmt.Print(node.Value, " ")
        inorder(node.Right)
    }
    inorder(bst.Root)
    fmt.Println()
}

func main() {
    bst := &BST{}
    values := []int{8, 3, 10, 1, 6, 14, 4, 7, 13}
    for _, v := range values {
        bst.Insert(v)
    }

    bst.Inorder()
    fmt.Println("Min:", must(bst.Min()))
    fmt.Println("Max:", must(bst.Max()))
    fmt.Println("Search 6:", bst.Search(6))
    fmt.Println("Search 99:", bst.Search(99))

    bst.Delete(3)
    bst.Inorder()
}

func must(val int, ok bool) int {
    if !ok {
        panic("empty tree")
    }
    return val
}
```

## Expected Output

```
1 3 4 6 7 8 10 13 14
Min: 1
Max: 14
Search 6: true
Search 99: false
1 4 6 7 8 10 13 14
```

## Hints

- Insert: walk down comparing values, attach when you find a nil child.
- Delete: handle 0, 1, and 2 children separately.
- Inorder of a BST always prints sorted order.

## Run

```bash
go run main.go
```
