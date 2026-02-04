# Hashing

> O(1) average lookup. Use for frequency, existence checks, grouping.

---

## Key Use Cases

| Use Case | Data Structure | Example |
|----------|----------------|---------|
| **Existence check** | HashSet | Contains duplicate? |
| **Frequency count** | HashMap<K, Integer> | Top K frequent |
| **Index mapping** | HashMap<K, List<Integer>> | Group anagrams |
| **Two Sum / Complement** | HashMap<value, index> | Pair with sum = target |
| **Caching** | HashMap | Memoization, LRU |

---

## Common Patterns

### 1. Two Sum (Complement)

```java
Map<Integer, Integer> map = new HashMap<>();
for (int i = 0; i < nums.length; i++) {
    int complement = target - nums[i];
    if (map.containsKey(complement))
        return new int[]{map.get(complement), i};
    map.put(nums[i], i);
}
```

### 2. Group by Key

```java
Map<String, List<String>> groups = new HashMap<>();
for (String s : strs) {
    String key = sort(s);  // or other grouping logic
    groups.computeIfAbsent(key, k -> new ArrayList<>()).add(s);
}
```

### 3. Frequency Count

```java
Map<Integer, Integer> freq = new HashMap<>();
for (int n : nums) freq.merge(n, 1, Integer::sum);
```

---

## Common Problems

| Problem | Approach |
|---------|----------|
| Two Sum | Complement in hash |
| Group Anagrams | Key = sorted string or char count |
| Top K Frequent | Freq map + bucket sort or heap |
| Longest Consecutive Sequence | Put in set, expand from starts |
| Subarray Sum Equals K | Prefix sum + hash (prefix[j]-prefix[i]=k) |
| Valid Anagram | Freq count or sort |

---

## Interview Tips

- **Collision:** Hash map handles; discuss load factor
- **Order:** HashMap unordered; use LinkedHashMap for insertion order
- **Space:** O(n) for hash; sometimes O(1) with array if key range small (e.g., 26 chars)
