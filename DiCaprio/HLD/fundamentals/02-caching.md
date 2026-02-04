# Caching

> Store frequently accessed data in fast storage to reduce latency and load on primary data store.

---

## Cache Strategies

| Strategy | When to Use | How |
|----------|-------------|-----|
| **Cache-Aside** | General purpose | App checks cache → miss → fetch from DB → populate cache |
| **Read-Through** | Read-heavy | Cache sits in front of DB; cache fetches on miss |
| **Write-Through** | Strong consistency | Write to cache + DB together |
| **Write-Behind** | Write-heavy | Write to cache; async flush to DB |
| **Write-Around** | Rarely re-read writes | Write only to DB; cache on read |

**Cache-Aside** is most common. App owns cache logic.

---

## Cache Eviction Policies

| Policy | How | Use Case |
|--------|-----|----------|
| **LRU** (Least Recently Used) | Evict least recently accessed | General purpose |
| **LFU** (Least Frequently Used) | Evict least frequently accessed | Popular items stay |
| **TTL** (Time To Live) | Expire after time | Stale data acceptable |
| **FIFO** | Evict oldest | Simple |

---

## Cache Invalidation

- **TTL** — Simple; may serve stale data
- **Event-based** — On write, invalidate/update cache
- **Versioning** — Cache key includes version; old keys expire naturally

---

## Cache Placement

| Layer | Example | Purpose |
|-------|---------|---------|
| **Client** | Browser cache | Reduce network |
| **CDN** | CloudFront, Cloudflare | Static assets, edge caching |
| **App** | In-memory (Guava, Caffeine) | Per-server, fastest |
| **Distributed** | Redis, Memcached | Shared across servers |

---

## Common Interview Q&A

**Q: Cache stampede / thundering herd?**  
A: Many requests miss cache simultaneously → all hit DB. Fix: Lock (single flight), probabilistic early expiration, or background refresh.

**Q: Cache consistency?**  
A: Hard. Options: Short TTL, write-through (stronger), or accept eventual consistency. Discuss trade-off.

**Q: Redis vs Memcached?**  
A: Redis: persistence, data structures, pub-sub. Memcached: simpler, multi-threaded, slightly faster for pure key-value.
