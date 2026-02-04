# DSA — Interview Cheat Sheet

> Quick reference before coding rounds.

---

## Pattern → Approach

| Pattern | When to Use | Example |
|---------|-------------|---------|
| **Two pointers** | Sorted array, pair finding | Two sum, palindrome |
| **Sliding window** | Subarray/substring, contiguous | Max sum, longest substring |
| **Hash map** | O(1) lookup, frequency | Two sum, anagrams |
| **Fast-slow pointer** | Linked list cycle, middle | Cycle detection |
| **Monotonic stack** | Next/prev greater/smaller | Next greater element |
| **BFS** | Shortest path (unweighted), level-order | Word ladder, tree levels |
| **DFS** | Path, cycle, explore all | Islands, graph traversal |
| **Binary search** | Sorted, or search space | Search, capacity |
| **DP** | Overlapping subproblems, optimal | Climbing stairs, LCS |
| **Backtracking** | All combinations/permutations | Subsets, N-Queens |

---

## Complexity

| Sort | O(n log n) |
| Hash | O(1) avg |
| Heap op | O(log n) |
| BST op | O(log n) |
| DFS/BFS | O(V + E) |

---

## Java Quick Reference

```java
Arrays.sort(arr);
Collections.sort(list);
Arrays.asList(1,2,3);
new ArrayList<>(list);
map.getOrDefault(k, 0);
map.merge(k, 1, Integer::sum);
PriorityQueue<Integer> pq = new PriorityQueue<>();
Deque<Integer> stack = new ArrayDeque<>();
StringBuilder sb = new StringBuilder();
s.toCharArray();
```

---

## Edge Cases

- Empty input
- Single element
- Two elements
- All same
- Duplicates
- Negative numbers
- Integer overflow (use long, mid = left + (right-left)/2)
