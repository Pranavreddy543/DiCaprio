# Consistency Models

> How strong are the guarantees when reading data in a distributed system?

---

## Spectrum (Strong → Weak)

| Model | Guarantee | Use Case |
|-------|-----------|----------|
| **Strong** | Read = latest write | Banking, inventory |
| **Causal** | Preserves cause-effect order | Social feed, comments |
| **Eventual** | All replicas converge if no new writes | Counters, likes |
| **Read-your-writes** | User sees own writes | Profile update |
| **Monotonic reads** | User never sees older data on subsequent reads | Session |

---

## Strong Consistency

- Linearizability / Serializability
- Every read sees latest committed write
- **Cost:** Latency (sync replication), availability (blocks during partition)

---

## Eventual Consistency

- If no new writes, all replicas eventually agree
- **Pros:** High availability, low latency
- **Cons:** May read stale data; conflicts possible
- **Conflict resolution:** Last-write-wins (LWW), vector clocks, CRDTs

---

## Causal Consistency

- If A happened-before B, all nodes see A before B
- Weaker than strong; stronger than eventual
- **Example:** Post → Comment → Like; all see in that order

---

## Common Interview Q&A

**Q: When to use eventual consistency?**  
A: When stale read is acceptable: social likes, view counts, recommendations. Not for: payments, inventory.

**Q: How to achieve read-your-writes?**  
A: Route user's reads to same replica that got the write; or use version/timestamp and wait for propagation.
