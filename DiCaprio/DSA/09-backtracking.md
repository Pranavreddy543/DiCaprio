# Backtracking

> Explore all possibilities; undo (backtrack) when path fails. DFS + prune.

---

## Template

```java
void backtrack(path, choices) {
    if (isComplete(path)) {
        result.add(new ArrayList<>(path));
        return;
    }
    for (choice in choices) {
        if (isValid(choice)) {
            path.add(choice);
            backtrack(path, updatedChoices);
            path.remove(path.size() - 1);  // undo
        }
    }
}
```

---

## Key Problems

### 1. Subsets

- Include or exclude each element
- 2^n subsets

### 2. Permutations

- Swap or use visited array
- n! permutations

### 3. Combinations (n choose k)

- Start index to avoid duplicates
- Choose k from n

### 4. Combination Sum

- Same element can be reused (or not)
- Track remaining target

### 5. N-Queens / Sudoku

- Place; validate; recurse; remove if invalid

---

## Pruning

- Skip invalid choices early
- Sort input for "no duplicate subsets" (skip same value)
- Bounds check (e.g., remaining sum)

---

## Common Problems

| Problem | Approach |
|---------|----------|
| Subsets | Include/exclude |
| Subsets II (with dupes) | Sort + skip same |
| Permutations | Visited array |
| Permutations II | Sort + skip same |
| Combination Sum | Start index, reuse |
| Palindrome Partitioning | Partition at each position |
| N-Queens | Place per row, validate column/diag |
| Word Search | DFS on grid, backtrack |

---

## Interview Tips

- **Copy path:** `result.add(new ArrayList<>(path))` — path is mutable
- **Undo:** Remove after recursive call
- **Pruning:** Reduce search space; mention in interview
