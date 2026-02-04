# Binary Search

> O(log n) search on sorted data. Also: search space reduction.

---

## Basic Template (Find target in sorted array)

```java
int left = 0, right = arr.length - 1;
while (left <= right) {
    int mid = left + (right - left) / 2;  // avoid overflow
    if (arr[mid] == target) return mid;
    if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
}
return -1;
```

---

## Variants

### 1. First occurrence (lower bound)

```java
// Find leftmost index where arr[i] >= target
while (left < right) {
    int mid = left + (right - left) / 2;
    if (arr[mid] < target) left = mid + 1;
    else right = mid;  // keep mid, search left
}
return left;
```

### 2. Last occurrence (upper bound)

```java
// Find rightmost index where arr[i] <= target
while (left < right) {
    int mid = left + (right - left + 1) / 2;  // bias right
    if (arr[mid] > target) right = mid - 1;
    else left = mid;
}
return left;
```

### 3. Search in rotated sorted array

- Find pivot (min element) via binary search
- Then binary search in left or right half

### 4. Search space (answer is in range)

- Binary search on answer (e.g., capacity, days)
- For each mid, check if feasible; adjust left/right

---

## Common Problems

| Problem | Approach |
|---------|----------|
| Binary Search | Basic template |
| Search Insert Position | Lower bound |
| First/Last Position | Lower + upper bound |
| Search Rotated Array | Find pivot, then search |
| Min in Rotated Array | Find pivot |
| Koko Eating Bananas | Binary search on speed |
| Capacity to Ship | Binary search on capacity |
| Median of Two Sorted Arrays | Binary search on partition |

---

## Interview Tips

- **Overflow:** Use `left + (right - left) / 2`
- **Loop:** `left <= right` vs `left < right` — affects termination
- **Boundary:** Off-by-one common; test with small examples
