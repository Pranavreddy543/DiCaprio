# Replication

> Copy data across multiple nodes for availability and read scaling.

---

## Sync vs Async Replication

| Sync | Async |
|------|-------|
| Replica acks before commit | Replica acked after |
| Strong consistency | Eventual consistency |
| Higher latency | Lower latency |
| Replica down = write blocks | Replica down = no block, but can lose data |

---

## Replication Topologies

### Primary-Replica (Master-Slave)

- One primary for writes; replicas for reads
- Replication: sync or async
- **Failover:** Promote replica to primary (manual or automatic)

### Multi-Primary

- Multiple nodes accept writes
- **Conflict resolution:** Required (LWW, merge, custom)
- **Use case:** Multi-region, offline-first

### Leaderless (Dynamo-style)

- No single leader; quorum reads/writes
- W + R > N for consistency
- **Example:** Cassandra, DynamoDB

---

## Leader Election

When primary fails, elect new leader.

| Algorithm | Description |
|-----------|-------------|
| **Bully** | Highest ID wins |
| **Raft** | Consensus; leader elected by majority |
| **ZooKeeper** | Ephemeral nodes; smallest wins |

**Tools:** etcd, ZooKeeper, Consul

---

## Split Brain

- Partition causes two primaries
- Both accept writes → divergence
- **Prevention:** Quorum, fencing tokens, odd number of nodes

---

## Common Interview Q&A

**Q: Sync replication — why not always?**  
A: Latency. Every write waits for replica. Geo-distributed = high latency.

**Q: How to handle replica lag?**  
A: Read-from-primary for read-after-write; or accept staleness with TTL.
