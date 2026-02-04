# CAP Theorem

> In a distributed system, you can have at most **2 of 3**: Consistency, Availability, Partition tolerance.

---

## The Three Properties

| Property | Meaning |
|----------|---------|
| **Consistency** | Every read gets the most recent write (or error) |
| **Availability** | Every request receives a response (no timeout) |
| **Partition Tolerance** | System works despite network partitions (nodes can't communicate) |

---

## The Trade-off

**Partition tolerance** is unavoidable in distributed systems (networks fail). So in practice you choose:

| Choice | Description | Examples |
|--------|-------------|----------|
| **CP** | Sacrifice availability during partition | ZooKeeper, etcd, HBase |
| **AP** | Sacrifice consistency during partition | Cassandra, DynamoDB, CouchDB |

---

## Important Nuances

- **CAP is about partition:** During *normal* operation, you can have both C and A
- **"AP" doesn't mean "no consistency":** Eventual consistency; C is relaxed, not absent
- **PACELC:** Extension — if no Partition (P): choose between Latency (L) and Consistency (C)

---

## Common Interview Q&A

**Q: Is MongoDB CP or AP?**  
A: By default, CP (primary-replica; blocks writes during partition). With read from secondary, can be more AP.

**Q: What does "partition" mean?**  
A: Network split; some nodes can't reach others. Not "sharding" (data partition).

**Q: Can you have all three?**  
A: No, in presence of partition. In single-node or perfect network, you can have C + A.
