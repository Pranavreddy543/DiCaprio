# Linked List

> Linear structure with O(1) insert/delete at known node. No random access.

---

## Key Patterns

### 1. Dummy Node

Avoid null checks for head; simplifies edge cases.

```java
ListNode dummy = new ListNode(0);
dummy.next = head;
ListNode curr = dummy;
// ... modify curr.next ...
return dummy.next;
```

### 2. Fast-Slow Pointer (Floyd's)

- **Cycle detection:** Fast 2x, slow 1x; meet = cycle
- **Middle element:** Fast at end → slow at middle
- **Cycle start:** After meet, reset slow to head; both 1x until meet

```java
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow == fast) return true;  // cycle
}
```

### 3. Reverse Linked List

```java
ListNode prev = null;
while (head != null) {
    ListNode next = head.next;
    head.next = prev;
    prev = head;
    head = next;
}
return prev;
```

### 4. Merge Two Sorted Lists

Use dummy; compare and attach.

---

## Common Problems

| Problem | Pattern |
|---------|---------|
| Reverse Linked List | Iterative 3-pointer |
| Merge Two Sorted Lists | Dummy + compare |
| Detect Cycle | Fast-slow pointer |
| Remove Nth From End | Fast-slow (fast n ahead) |
| Reorder List | Find middle, reverse half, merge |
| Merge K Sorted Lists | Min heap or divide-conquer |

---

## Doubly Linked List

- Prev + next pointers
- O(1) delete if you have node reference
- Used in: LRU cache, browser history

---

## Interview Tips

- Draw diagram; pointer manipulation is error-prone
- Check: null head, single node, two nodes
- Circular list: handle cycle to avoid infinite loop
