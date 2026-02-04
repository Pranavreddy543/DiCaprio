# Stack & Queue

> LIFO vs FIFO. Used in DFS, BFS, parsing, monotonic problems.

---

## Stack

- **Use:** DFS, parenthesis matching, undo, expression evaluation
- **Monotonic stack:** Maintain sorted order; next greater/smaller element

```java
// Next Greater Element - monotonic decreasing stack
Deque<Integer> stack = new ArrayDeque<>();
int[] result = new int[n];
Arrays.fill(result, -1);
for (int i = 0; i < n; i++) {
    while (!stack.isEmpty() && arr[stack.peek()] < arr[i])
        result[stack.pop()] = arr[i];
    stack.push(i);
}
```

---

## Queue

- **Use:** BFS, level-order traversal, sliding window (deque)
- **Priority Queue (Heap):** Top K, merge K sorted, Dijkstra

---

## Deque (Double-ended Queue)

- Push/pop from both ends
- **Sliding window max:** Monotonic deque (store indices; front = max)

---

## Common Problems

| Problem | Approach |
|---------|----------|
| Valid Parentheses | Stack |
| Min Stack | Stack + min tracking |
| Next Greater Element | Monotonic stack |
| Sliding Window Maximum | Monotonic deque |
| Implement Queue using Stacks | Two stacks |
| Level Order Traversal | BFS with queue |

---

## Heap (Priority Queue)

- **Min heap:** Smallest on top; `PriorityQueue` in Java
- **Max heap:** Negate or custom comparator
- **Use:** Top K, merge K lists, median, Dijkstra

```java
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
```

---

## Interview Tips

- Stack for DFS, recursion simulation, matching
- Queue for BFS, level-order
- Monotonic stack: "next greater" / "previous smaller" type problems
