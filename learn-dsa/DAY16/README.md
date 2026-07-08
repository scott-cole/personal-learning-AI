# DAY 16 — Binary Search Trees

## A Well-Organised Filing Cabinet

A filing cabinet has drawers labelled A-M and N-Z. You open the "S" drawer, then find the folder "Sm," then "Smi," then "Smith." At each step, you decide left or right and cut the search space in half.

A Binary Search Tree (BST) does the same: for every node, all values in the left subtree are smaller, and all values in the right subtree are larger. This property makes search, insert, and delete O(h) where h is the tree height — ideally O(log n).

## BST Node

```go
type BSTNode struct {
    Value int
    Left  *BSTNode
    Right *BSTNode
}

type BST struct {
    Root *BSTNode
}
```

## Search — O(h)

```go
func (bst *BST) Search(value int) bool {
    curr := bst.Root
    for curr != nil {
        if value == curr.Value {
            return true
        }
        if value < curr.Value {
            curr = curr.Left
        } else {
            curr = curr.Right
        }
    }
    return false
}
```

## Insert — O(h)

```go
func (bst *BST) Insert(value int) {
    node := &BSTNode{Value: value}
    if bst.Root == nil {
        bst.Root = node
        return
    }
    curr := bst.Root
    for {
        if value < curr.Value {
            if curr.Left == nil {
                curr.Left = node
                return
            }
            curr = curr.Left
        } else {
            if curr.Right == nil {
                curr.Right = node
                return
            }
            curr = curr.Right
        }
    }
}
```

## Delete — O(h)

```go
func (bst *BST) Delete(value int) {
    bst.Root = deleteNode(bst.Root, value)
}

func deleteNode(root *BSTNode, value int) *BSTNode {
    if root == nil {
        return nil
    }
    if value < root.Value {
        root.Left = deleteNode(root.Left, value)
    } else if value > root.Value {
        root.Right = deleteNode(root.Right, value)
    } else {
        // Found the node
        if root.Left == nil {
            return root.Right
        }
        if root.Right == nil {
            return root.Left
        }
        // Two children: replace with inorder successor
        minNode := findMin(root.Right)
        root.Value = minNode.Value
        root.Right = deleteNode(root.Right, minNode.Value)
    }
    return root
}

func findMin(root *BSTNode) *BSTNode {
    curr := root
    for curr.Left != nil {
        curr = curr.Left
    }
    return curr
}
```

## Min / Max — O(h)

```go
func (bst *BST) Min() (int, bool) {
    if bst.Root == nil {
        return 0, false
    }
    curr := bst.Root
    for curr.Left != nil {
        curr = curr.Left
    }
    return curr.Value, true
}
```

## Summary

| Operation | Average | Worst (degenerate) |
|---|---|---|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |
| Min/Max | O(log n) | O(n) |

A degenerate tree (always inserting sorted values) becomes a linked list. Balanced trees (AVL, Red-Black) fix this.
