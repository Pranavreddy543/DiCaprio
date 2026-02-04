# HLD — Interview Cheat Sheet

> Quick reference for last-minute revision.

---

## Fundamentals

| Topic | Key Points |
|-------|------------|
| **Load Balancing** | Round robin, least connections, IP hash. L4 vs L7. Health checks. |
| **Caching** | Cache-aside (common), LRU eviction. Stampede: lock or probabilistic refresh. |
| **DB Scaling** | Read: replication. Write: sharding. Hash vs range sharding. |
| **Message Queue** | Async, decoupling. Kafka: partitions, consumer groups. At-least-once + idempotent. |

---

## System Design Checklist

1. **Requirements** — Scale (QPS, users), latency, consistency
2. **Estimate** — Storage, bandwidth, # servers
3. **High-level** — Client → LB → App → Cache → DB
4. **Deep dive** — 2–3 components in detail
5. **Bottlenecks** — SPOF, scaling limits

---

## Back-of-Envelope

- 1M DAU ≈ 10–100 QPS (depending on usage)
- Avg request: 10 KB; Response: 100 KB
- Server: ~1K QPS per core (rough)
- DB: ~1K QPS per instance (write); 10K (read with cache)

---

## Design Quick Reference

| System | Key Components |
|--------|-----------------|
| **URL Shortener** | Base62 from ID, 301 vs 302, cache |
| **Rate Limiter** | Sliding window, token bucket, Redis |
| **Chat** | WebSocket, Kafka, presence (Redis) |
| **News Feed** | Push vs Pull, hybrid for celebrities |
| **Distributed Cache** | Consistent hashing, replication, LRU |

---

## CAP & Consistency

| CAP Choice | Examples |
|------------|----------|
| **CP** | ZooKeeper, etcd, HBase |
| **AP** | Cassandra, DynamoDB, CouchDB |

| Consistency | When |
|-------------|------|
| **Strong** | Payments, inventory |
| **Eventual** | Likes, counters, recommendations |

---

## Common Interview Q&A

**Q: How to scale a system?**  
A: Cache (read), replicate (read), shard (write), async (decouple), CDN (static).

**Q: Single point of failure?**  
A: Multiple instances, multiple AZs, health checks, failover.

**Q: How to reduce latency?**  
A: Cache, CDN, reduce round trips, async, connection pooling.

**Q: Consistency vs availability?**  
A: CAP. Banking: consistency. Social: availability. Discuss trade-off.
