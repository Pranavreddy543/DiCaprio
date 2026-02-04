# Sharding (Data Partitioning)

> Split data across multiple databases to scale writes and storage.

---

## When to Shard

- Single DB can't handle write load
- Storage exceeds single node
- After vertical scaling and read replicas

---

## Sharding Strategies

| Strategy | How | Pros | Cons |
|----------|-----|------|------|
| **Range** | Key range per shard (A–M, N–Z) | Range queries easy | Hot spots (e.g., names starting with A) |
| **Hash** | hash(key) % N | Even distribution | Range queries need all shards |
| **Directory** | Lookup table: key → shard | Flexible, can rebalance | Lookup overhead, SPOF |
| **Composite** | e.g., (tenant_id, user_id) | Multi-tenant isolation | Complexity |

---

## Shard Key Selection

- **Critical:** Bad key = hot shards, uneven load
- **Choose:** High cardinality, even distribution, matches query patterns
- **Avoid:** Monotonic (timestamp, auto-increment) → all writes to last shard

---

## Challenges

| Challenge | Solution |
|-----------|----------|
| **Joins across shards** | Denormalize, or application-level join |
| **Adding shards** | Rebalance: move data; dual-write during migration |
| **Hot shard** | Split shard, or better shard key |
| **Global uniqueness** | Central ID service (Snowflake), or UUID |

---

## Common Interview Q&A

**Q: How to add a new shard?**  
A: Create shard; rebalance (move some keys from existing shards). Use consistent hashing to minimize moves. Downtime or dual-write + background migration.

**Q: Cross-shard transactions?**  
A: 2PC (slow, blocks) or Saga (eventual, compensation). Often avoid by design (single shard per transaction).
