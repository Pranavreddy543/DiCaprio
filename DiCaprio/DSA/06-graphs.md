# Graphs

> Nodes + edges. Adjacency list most common representation.

---

## Representation

```java
// Adjacency list (most common)
List<List<Integer>> adj = new ArrayList<>();
for (int i = 0; i < n; i++) adj.add(new ArrayList<>());
adj.get(u).add(v);  // directed edge u -> v

// For weighted: List<List<int[]>> or Map<Integer, List<Pair>>
```

---

## Traversals

### BFS (Level-order, Shortest path in unweighted)

```java
Queue<Integer> q = new LinkedList<>();
boolean[] visited = new boolean[n];
q.offer(start);
visited[start] = true;
while (!q.isEmpty()) {
    int u = q.poll();
    for (int v : adj.get(u)) {
        if (!visited[v]) {
            visited[v] = true;
            q.offer(v);
        }
    }
}
```

### DFS (Path finding, Cycle detection)

```java
void dfs(int u, boolean[] visited) {
    visited[u] = true;
    for (int v : adj.get(u)) {
        if (!visited[v]) dfs(v, visited);
    }
}
```

---

## Key Algorithms

### 1. Topological Sort (DAG)

- **Kahn's (BFS):** In-degree count; process zero in-degree
- **DFS:** Post-order reverse = topological order

### 2. Shortest Path

| Algorithm | Graph | Complexity |
|-----------|-------|------------|
| BFS | Unweighted | O(V+E) |
| Dijkstra | Non-negative weights | O((V+E) log V) |
| Bellman-Ford | Negative weights | O(VE) |

### 3. Cycle Detection

- **Undirected:** DFS with parent
- **Directed:** DFS with recursion stack (gray/black)

### 4. Union-Find (Disjoint Set)

- Connect components; find root with path compression
- Used in: MST (Kruskal), cycle detection

---

## Common Problems

| Problem | Approach |
|---------|----------|
| Number of Islands | DFS/BFS on grid |
| Clone Graph | BFS + HashMap |
| Course Schedule | Topological sort |
| Word Ladder | BFS (shortest path) |
| Network Delay Time | Dijkstra |
| Redundant Connection | Union-Find |
| Critical Connections | Tarjan (bridges) |

---

## Interview Tips

- Clarify: Directed? Weighted? Connected?
- Visited set to avoid cycles
- Grid as graph: 4 or 8 neighbors
