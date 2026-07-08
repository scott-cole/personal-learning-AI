# DAY 17 — Tree Recursion

## Standing Between Two Mirrors

Stand between two parallel mirrors. Each mirror reflects the other, creating an infinite regress. The reflection in the first mirror shows the reflection in the second mirror showing the reflection in the first mirror... That's recursion — a function that calls itself, with each call working on a smaller piece of the problem.

Tree problems are naturally recursive: the left child is a smaller tree, the right child is a smaller tree. Recursion lets you solve the whole tree by solving its subtrees.

## Max Depth of a Binary Tree

```go
func maxDepth(root *TreeNode) int {
    if root == nil {
        return 0
    }
    leftDepth := maxDepth(root.Left)
    rightDepth := maxDepth(root.Right)
    if leftDepth > rightDepth {
        return leftDepth + 1
    }
    return rightDepth + 1
}
```

## Balanced Tree Check

A tree is height-balanced if the depth difference between left and right subtrees is at most 1.

```go
func isBalanced(root *TreeNode) bool {
    _, ok := checkBalance(root)
    return ok
}

func checkBalance(root *TreeNode) (int, bool) {
    if root == nil {
        return 0, true
    }
    leftH, leftOk := checkBalance(root.Left)
    rightH, rightOk := checkBalance(root.Right)
    if !leftOk || !rightOk {
        return 0, false
    }
    diff := leftH - rightH
    if diff < 0 {
        diff = -diff
    }
    if diff > 1 {
        return 0, false
    }
    if leftH > rightH {
        return leftH + 1, true
    }
    return rightH + 1, true
}
```

## Invert Tree

```go
func invertTree(root *TreeNode) *TreeNode {
    if root == nil {
        return nil
    }
    root.Left, root.Right = root.Right, root.Left
    invertTree(root.Left)
    invertTree(root.Right)
    return root
}
```

## Serialize and Deserialize

```go
// Serialize (preorder, with markers for nil)
func serialize(root *TreeNode) string {
    var result []string
    var dfs func(*TreeNode)
    dfs = func(node *TreeNode) {
        if node == nil {
            result = append(result, "null")
            return
        }
        result = append(result, strconv.Itoa(node.Value))
        dfs(node.Left)
        dfs(node.Right)
    }
    dfs(root)
    return strings.Join(result, ",")
}

// Deserialize
func deserialize(data string) *TreeNode {
    nodes := strings.Split(data, ",")
    idx := 0
    var build func() *TreeNode
    build = func() *TreeNode {
        if nodes[idx] == "null" {
            idx++
            return nil
        }
        val, _ := strconv.Atoi(nodes[idx])
        idx++
        node := &TreeNode{Value: val}
        node.Left = build()
        node.Right = build()
        return node
    }
    return build()
}
```

## Summary

| Problem | Approach | Complexity |
|---|---|---|
| Max depth | Post-order return depth | O(n) |
| Balanced check | Post-order with early exit | O(n) |
| Invert tree | Pre-order swap children | O(n) |
| Serialize | Pre-order with nil markers | O(n) |
