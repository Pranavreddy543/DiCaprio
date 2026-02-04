# Arrays & Strings

> Most common data structure. Master these patterns first.

---

## Key Patterns

### 1. Two Pointers

- **Same direction:** Remove duplicates, move zeros
- **Opposite ends:** Two sum (sorted), palindrome check
- **Fast-slow:** Find cycle, middle element

```java
// Two Sum (sorted array) - opposite ends
int left = 0, right = arr.length - 1;
while (left < right) {
    int sum = arr[left] + arr[right];
    if (sum == target) return new int[]{left, right};
    if (sum < target) left++;
    else right--;
}
```

### 2. Sliding Window

- **Fixed size:** Max sum of k consecutive elements
- **Variable size:** Longest subarray with sum ≤ k

```java
// Fixed window - max sum of k elements
int sum = 0;
for (int i = 0; i < k; i++) sum += arr[i];
int maxSum = sum;
for (int i = k; i < arr.length; i++) {
    sum += arr[i] - arr[i - k];
    maxSum = Math.max(maxSum, sum);
}
```

### 3. Prefix Sum

- Precompute cumulative sum for range queries
- `prefix[i] = arr[0] + ... + arr[i]`
- Range sum [i, j] = `prefix[j] - prefix[i-1]`

### 4. In-place Modification

- Use same array; track write index
- Examples: Remove element, move zeros, merge sorted

---

## Common Problems

| Problem | Pattern | Difficulty |
|---------|---------|------------|
| Two Sum | Hash map | Easy |
| Three Sum | Sort + Two pointers | Medium |
| Container With Most Water | Two pointers | Medium |
| Sliding Window Maximum | Monotonic deque | Hard |
| Longest Substring Without Repeating | Sliding window + Hash | Medium |
| Trapping Rain Water | Two pointers / Stack | Hard |

---

## String-Specific

- **Anagram:** Sort both, or count frequency (26/256 array)
- **Palindrome:** Two pointers from ends
- **Substring:** Sliding window, or Rabin-Karp for pattern matching
- **String builder** for concatenation in loops (O(n²) → O(n))

---

## Interview Tips

- Clarify: Sorted? Duplicates? Negative numbers?
- Edge cases: Empty, single element, all same
- Space: Can you use O(1) extra? (In-place)
