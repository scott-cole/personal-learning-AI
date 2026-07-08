# DAY 15 — Binary Trees

## A Company Org Chart

The CEO sits at the top. Under them are two VPs. Under each VP are two directors. Under each director are two managers. This is a binary tree: each node has at most two children (left and right). The CEO is the "root," the managers at the bottom are "leaves."

Binary trees are the foundation of BSTs, heaps, expression trees, and parse trees. Traversals let you visit every node in a specific order.

## The Node

```go
type TreeNode struct {
    Value int
    Left  *TreeNode
    Right *TreeNode
}
```

## Depth-First Traversals

```go
// Preorder: root → left → right
func preorder(root *TreeNode) {
    if root == nil {
        return
    }
    fmt.Print(root.Value, " ")
    preorder(root.Left)
    preorder(root.Right)
}

// Inorder: left → root → right
func inorder(root *TreeNode) {
    if root == nil {
        return
    }
    inorder(root.Left)
    fmt.Print(root.Value, " ")
    inorder(root.Right)
}

// Postorder: left → right → root
func postorder(root *TreeNode) {
    if root == nil {
        return
    }
    postorder(root.Left)
    postorder(root.Right)
    fmt.Print(root.Value, " ")
}
```

## Level-Order Traversal (BFS)

```go
func levelOrder(root *TreeNode) {
    if root == nil {
        return
    }
    queue := []*TreeNode{root}
    for len(queue) > 0 {
        node := queue[0]
        queue = queue[1:]
        fmt.Print(node.Value, " ")
        if node.Left != nil {
            queue = append(queue, node.Left)
        }
        if node.Right != nil {
            queue = append(queue, node.Right)
        }
    }
}
```

## Building a Tree

```go
//       1
//      / \
//     2   3
//    / \
//   4   5
root := &TreeNode{Value: 1}
root.Left = &TreeNode{Value: 2}
root.Right = &TreeNode{Value: 3}
root.Left.Left = &TreeNode{Value: 4}
root.Left.Right = &TreeNode{Value: 5}
```

## Traversal Orders

| Traversal | Order | Use Case |
|---|---|---|
| Preorder | root, left, right | Copy tree, prefix notation |
| Inorder | left, root, right | BST sorted output |
| Postorder | left, right, root | Delete tree, postfix notation |
| Level-order | top-to-bottom, left-to-right | BFS, shortest path (unweighted) |
