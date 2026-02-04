# Design Rate Limiter

> Limit number of requests per user/IP in a time window.

---

## Requirements

**Functional**
- Limit: e.g., 100 requests/minute per user
- Return 429 when exceeded
- Different limits per API tier (free vs premium)

**Non-Functional**
- Low latency (in critical path)
- Distributed (multiple app servers)
- Accurate, fair

---

## Algorithms

### 1. Fixed Window Counter

```
Window: [0-60s] count=50, [60-120s] count=50
Problem: 50 at 59s + 50 at 61s = 100 in 2 seconds (burst)
```

Simple but allows burst at boundaries.

### 2. Sliding Window Log

Store timestamp of each request. Count requests in last window.

```
Requests: [10:00:01, 10:00:30, 10:00:45, 10:01:15]
Window 60s at 10:01:15 → count from 10:00:15 = 3
```

Accurate but memory-heavy (store all timestamps).

### 3. Sliding Window Counter (Hybrid)

```
count = prev_window_count * (1 - elapsed/window) + current_window_count
```

Approximation; O(1) memory; good balance.

### 4. Token Bucket

- Bucket has capacity N tokens
- Refill rate R tokens/sec
- Request consumes 1 token; if empty → reject

Flexible; allows burst up to N.

### 5. Leaky Bucket

- Requests enter bucket; processed at fixed rate (leak)
- Overflow → reject

Smooths traffic; no burst.

---

## Where to Implement

| Location | Pros | Cons |
|----------|------|------|
| **Client** | Reduces load | Bypassable |
| **API Gateway** | Central, before app | Single place |
| **App middleware** | Flexible | Per-service |
| **Reverse proxy** | Nginx `limit_req` | Limited logic |

**Recommended:** API Gateway or dedicated service (Redis-backed).

---

## Redis Implementation (Sliding Window)

```
Key: user_id or IP
Value: sorted set of timestamps

ZADD rate_limit:user123 1633024800.001 "req1"
ZREMRANGEBYSCORE rate_limit:user123 0 (now - window)
ZCARD rate_limit:user123
If count < limit: ZADD, allow
Else: reject 429
```

---

## High-Level Design

```
Client → API Gateway → Rate Limiter (Redis) → App
                ↑
                └─ 429 if exceeded
```

---

## Interview Talking Points

- Fixed vs sliding window (burst problem)
- Token bucket vs leaky bucket
- Redis for distributed state
- 429 + Retry-After header
