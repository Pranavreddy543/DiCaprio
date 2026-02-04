# Trees

> Hierarchical structure. Master traversal and recursion.

---

## Traversals

| Order | Visit | Use Case |
|-------|-------|----------|
| **Inorder** | Left, Root, Right | BST → sorted order |
| **Preorder** | Root, Left, Right | Copy tree, prefix |
| **Postorder** | Left, Right, Root | Delete tree, expression eval |
| **Level order** | BFS by level | Level-wise processing |

---

## Recursion Template

```java
void dfs(TreeNode node) {
    if (node == null) return;
    // Pre-order: process here
    dfs(node.left);
    // In-order: process here
    dfs(node.right);
    // Post-order: process here
}
```

---

## Key Patterns

### 1. Max Depth / Height

```java
int height(TreeNode node) {
    if (node == null) return 0;
    return 1 + Math.max(height(node.left), height(node.right));
}
```

### 2. Check Balanced

Height diff ≤ 1 for every node. Return height; use -1 for unbalanced.

### 3. Lowest Common Ancestor (LCA)

- If both in left → LCA in left
- If both in right → LCA in right
- Else current is LCA

### 4. BST Validation

Inorder must be sorted. Or: min < node < max per subtree.

### 5. BST Search / Insert

- Search: Compare, go left or right
- Insert: Find null position, attach

---

## Binary Search Tree (BST)

- Left < Root < Right
- Inorder = sorted
- Search, insert, delete: O(h), h = height

---

## Common Problems

| Problem | Approach |
|---------|----------|
| Max Depth | Recursion |
| Same Tree | Recursion |
| Invert Binary Tree | Swap left/right recursively |
| Lowest Common Ancestor | Recursion (both sides) |
| Validate BST | Inorder or min/max range |
| Kth Smallest in BST | Inorder (stop at k) |
| Serialize/Deserialize | Preorder + null markers |
| Binary Tree Max Path Sum | Postorder; return max path through node |

---

## Interview Tips

- Null check first in recursion
- Return value vs global variable for "max/min so far"
- BST: Inorder for sorted; use property for range queries
