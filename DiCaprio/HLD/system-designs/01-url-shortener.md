# Design URL Shortener (bit.ly)

> Convert long URL to short alias; redirect on access.

---

## Requirements

**Functional**
- Shorten URL: `long.com/very-long-path` → `short.com/abc123`
- Redirect: `short.com/abc123` → 301/302 to long URL
- Optional: Custom alias, analytics

**Non-Functional**
- High availability, low latency
- Scale: 100M URLs/month, 10:1 read:write ratio

---

## Capacity Estimation

- 100M URLs/month ≈ 40 URLs/sec write, 400 redirects/sec read
- 5 years storage: 100M × 12 × 5 = 6B URLs
- Avg URL 500 bytes → ~3 TB (with index)
- Redirects: 400/sec read → cache handles most

---

## High-Level Design

```
Client → LB → App Servers → Cache (Redis) → DB
                ↑                ↑
                |                |
         Shorten API      Redirect (cache hit)
         stores mapping   else DB lookup → cache
```

---

## Core Components

### 1. Short URL Generation

| Approach | Pros | Cons |
|----------|------|------|
| **Hash (MD5/SHA)** | No collision if use counter | Need collision handling |
| **Base62 encode** | Short, unique if from counter | Need unique ID source |
| **Random** | Simple | Collision check, retry |

**Recommended:** Base62(counter). Counter from DB auto-increment or distributed ID (Snowflake).

**Base62:** 0-9, a-z, A-Z = 62 chars. 6 chars → 62^6 ≈ 56B combinations.

### 2. Database Schema

```
url_mappings:
  id (PK, auto-increment)
  short_code (unique index)
  long_url
  created_at
  user_id (optional)
```

### 3. Redirect (301 vs 302)

- **301 Permanent:** Browser caches; fewer server hits; analytics harder
- **302 Temporary:** Server gets request each time; can track clicks
- **Default:** 302 for analytics

### 4. Cache

- Cache `short_code → long_url` in Redis
- TTL: configurable (e.g., 24h for hot URLs)
- Cache-aside: On redirect miss, DB lookup → populate cache

---

## Scale Considerations

- **DB:** Shard by short_code hash or range
- **Cache:** Redis cluster
- **Rate limiting:** Prevent abuse
- **Custom alias:** Check uniqueness; reserve short words

---

## Interview Talking Points

- Base62 encoding from unique ID
- 301 vs 302 trade-off
- Cache for read-heavy workload
- Collision handling if using hash
