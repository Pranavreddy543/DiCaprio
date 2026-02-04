# Design News Feed (Twitter, Facebook)

> Show users a personalized feed of posts from people they follow.

---

## Requirements

**Functional**
- Post creation
- Feed: chronological or ranked
- Follow/unfollow
- Like, comment (optional)

**Non-Functional**
- Feed load < 200ms
- Scale: 300M users, 500M posts/day

---

## Pull vs Push (Fan-out)

### Pull (Read-time)

- On feed request: Get followees → Fetch their latest posts → Merge & sort
- **Pros:** Simple, real-time, no precomputation
- **Cons:** Slow for users following many (celebrity problem)

### Push (Write-time)

- On post: Fan-out to all followers' feed cache
- **Pros:** Fast read (precomputed)
- **Cons:** Slow write for celebrities (millions of followers); storage cost

### Hybrid

- **Regular users:** Push (fan-out on write)
- **Celebrities (> 100K followers):** Pull (fetch on read, merge with precomputed)
- Best of both

---

## High-Level Design

```
Post Service → Fan-out Service → Write to Feed Cache (Redis) per follower
                    ↓
              Message Queue (async fan-out)

Feed Service → Read from Feed Cache (user's feed)
                    ↓
              If celebrity in follow list: Pull + merge
```

---

## Data Model

**Posts:**
```
posts: id, user_id, content, created_at
```

**Follow graph:**
```
follows: follower_id, followee_id
```

**Feed cache (per user):**
```
feed:user_123 → Sorted Set [post_id → timestamp]
```

---

## Feed Ranking (Optional)

Instead of pure chronological:
- **Score = f(recency, engagement, relationship)**
- Precompute or compute at read
- ML model for personalization

---

## Scale Considerations

- **Fan-out:** Async via queue; workers write to feed cache
- **Celebrity handling:** Pull for users with > 100K followers
- **Feed cache:** Redis sorted set; TTL (e.g., 7 days of feed)
- **Cold start:** User not logged in for days → rebuild from DB or skip

---

## Interview Talking Points

- Pull vs Push trade-off
- Hybrid for celebrity problem
- Redis sorted set for feed
- Async fan-out via queue
