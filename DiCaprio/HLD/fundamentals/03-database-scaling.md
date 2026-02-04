# Database Scaling

> Techniques to handle more read/write load and storage.

---

## Vertical vs Horizontal

| Vertical | Horizontal |
|----------|------------|
| Bigger machine (CPU, RAM) | More machines |
| Simpler, has limits | Scales further |
| Single point of failure | Distributed |

---

## Read Scaling: Replication

```
         Write
    Primary (Master) ──────────────────►
         │
         │ Replication
         ▼
    Replica 1   Replica 2   Replica 3
    (Read)      (Read)      (Read)
```

- **Primary-Replica:** Writes to primary; reads from replicas
- **Replication lag:** Reads may be stale. Use primary for read-after-write if needed.
- **Multi-primary:** Complex; conflict resolution needed

---

## Write Scaling: Sharding (Partitioning)

Split data across multiple DBs by a **shard key**.

| Sharding Strategy | How | Pros | Cons |
|-------------------|-----|------|------|
| **Range-based** | Key range per shard (A–M, N–Z) | Simple, range queries | Hot spots |
| **Hash-based** | hash(key) % N → shard | Even distribution | Range queries hard |
| **Directory-based** | Lookup table: key → shard | Flexible | Extra lookup, SPOF risk |

**Shard key** choice is critical. Bad key = hot shards.

---

## Indexing

- **B-tree:** Default for most DBs; good for range queries
- **Hash index:** Exact match only
- **Composite index:** Order matters (a, b) ≠ (b, a)
- **Covering index:** Index includes all needed columns → no table lookup

---

## Common Interview Q&A

**Q: How to add new shard?**  
A: Rebalance: move data from existing shards. Requires downtime or dual-write + migration. Tools: Vitess, Citus.

**Q: Joins across shards?**  
A: Application-level joins (fetch from each shard, merge) or denormalize to avoid.

**Q: Hot partition?**  
A: High traffic on one shard. Fix: Better shard key (e.g., add tenant/date), split hot shard, or use composite key.
