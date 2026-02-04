# High Level Design (HLD)

> System design, scalability, and distributed systems — Interview prep for Senior SDE / SDE2.

---

## Contents

| Topic | Description |
|-------|-------------|
| [Fundamentals](./fundamentals/) | Load balancing, Caching, Database scaling, Message queues |
| [System Designs](./system-designs/) | URL shortener, Rate limiter, Chat, News feed |
| [Distributed Systems](./distributed-systems/) | CAP, Consistency, Replication, Sharding |
| [Interview Cheat Sheet](./INTERVIEW-CHEATSHEET.md) | Quick revision before interviews |

---

## How to Approach HLD Interviews

1. **Clarify requirements** — Scale (QPS, users), latency, consistency needs
2. **Estimate** — Back-of-envelope (storage, bandwidth, servers)
3. **High-level design** — Draw boxes: Client → LB → App → Cache → DB
4. **Deep dive** — Pick 2–3 components; discuss trade-offs
5. **Identify bottlenecks** — Single point of failure? How to scale?

---

## Common HLD Problems

- Design URL Shortener (bit.ly)
- Design Rate Limiter
- Design Chat (WhatsApp, Slack)
- Design News Feed (Twitter, Facebook)
- Design YouTube / Netflix
- Design Google Search
- Design Distributed Cache
- Design Pastebin
