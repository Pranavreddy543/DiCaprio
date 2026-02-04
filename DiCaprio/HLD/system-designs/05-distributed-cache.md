# Design Distributed Cache (Redis-like)

> In-memory key-value store distributed across multiple nodes.

---

## Requirements

**Functional**
- get(key), set(key, value), delete(key)
- TTL support
- High availability

**Non-Functional**
- Sub-millisecond latency
- Scale: 1M QPS, 10TB data
- Fault tolerance (replication)

---

## Core Design

### 1. Data Partitioning: Consistent Hashing

- **Problem:** Hash(key) % N → adding/removing node causes massive rehashing
- **Consistent hashing:** Ring of hash space; each node owns range. Add/remove node → only adjacent keys move.

```
Ring: 0 ────────────────────────────── 2^32
      │    Node A    │  Node B  │ Node C │
      └──────────────┴──────────┴────────┘
```

- **Virtual nodes:** Multiple points per physical node → even distribution

### 2. Replication

- Each key replicated to N nodes (e.g., 3)
- Read from replica; write to primary + replicas
- Quorum: W + R > N for consistency

### 3. Eviction

- **LRU** per node
- Evict when memory limit reached

### 4. Caching Layer

- Cache is cache — can have stale data
- TTL for expiration
- Cache invalidation on write (optional)

---

## High-Level Design

```
Client → Proxy/LB → Cache Node (consistent hash ring)
                        │
                        ├─ Node A (keys 0-1000)
                        ├─ Node B (keys 1001-2000)
                        └─ Node C (keys 2001-3000)
                        Each with replicas
```

---

## Consistency

- **Strong:** Rarely needed for cache; hurts availability
- **Eventual:** Typical; replicas sync asynchronously
- **Read-your-writes:** Route to same node for session; or version vectors

---

## Interview Talking Points

- Consistent hashing for minimal rebalancing
- Virtual nodes for even distribution
- Replication for fault tolerance
- LRU + TTL for eviction
