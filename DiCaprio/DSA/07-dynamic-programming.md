# Dynamic Programming

> Optimize by storing subproblem results. "Those who cannot remember the past are condemned to repeat it."

---

## When to Use

- Overlapping subproblems
- Optimal substructure (optimal solution uses optimal subsolutions)
- Often: "max/min/count number of ways"

---

## Approach

1. **Define state** — What does dp[i] represent?
2. **Recurrence** — How does dp[i] relate to smaller states?
3. **Base case** — dp[0], dp[1], etc.
4. **Order** — Fill in dependency order (often left to right)

---

## Patterns

### 1. Linear (1D)

- **Climbing stairs:** dp[i] = dp[i-1] + dp[i-2]
- **House robber:** dp[i] = max(dp[i-1], dp[i-2] + arr[i])
- **Max subarray:** Kadane's; dp = max(nums[i], dp + nums[i])

### 2. Grid (2D)

- **Unique paths:** dp[i][j] = dp[i-1][j] + dp[i][j-1]
- **Min path sum:** dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])

### 3. String (2D)

- **LCS:** dp[i][j] = 1 + dp[i-1][j-1] if match else max(dp[i-1][j], dp[i][j-1])
- **Edit distance:** Insert, delete, replace
- **Palindrome:** dp[i][j] = s[i]==s[j] && dp[i+1][j-1]

### 4. Knapsack

- **0/1:** dp[i][w] = max(dp[i-1][w], val[i] + dp[i-1][w-wt[i]])
- **Unbounded:** dp[w] = max(dp[w], val[i] + dp[w-wt[i]])

### 5. Interval / Sequence

- **Matrix chain multiplication**
- **Longest increasing subsequence (LIS):** dp[i] = 1 + max(dp[j]) for j < i, arr[j] < arr[i]

---

## Memoization vs Tabulation

| Memoization | Tabulation |
|-------------|------------|
| Top-down, recursive | Bottom-up, iterative |
| Lazy (only compute needed) | Eager (all states) |
| Stack overflow risk | No recursion |
| Often easier to write | Often more efficient |

---

## Common Problems

| Problem | Pattern |
|---------|---------|
| Climbing Stairs | Linear |
| House Robber | Linear |
| Coin Change | Unbounded knapsack |
| Longest Increasing Subsequence | LIS |
| Longest Common Subsequence | 2D string |
| Edit Distance | 2D string |
| Word Break | 1D + string |
| Max Product Subarray | Kadane variant |

---

## Interview Tips

- Start with recursive solution; add memoization
- Or: Define state, write recurrence, implement
- Space optimization: Often only need prev row/col (rolling array)
