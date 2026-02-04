# Load Balancing

> Distributes incoming traffic across multiple servers to improve availability and performance.

---

## When to Use

- Multiple app servers behind a single entry point
- High availability (if one server fails, others serve)
- Horizontal scaling

---

## Load Balancing Algorithms

| Algorithm | How it Works | Use Case |
|-----------|--------------|----------|
| **Round Robin** | Rotate through servers sequentially | Equal capacity servers |
| **Weighted Round Robin** | Round robin with weights by capacity | Unequal server capacity |
| **Least Connections** | Send to server with fewest active connections | Long-lived connections |
| **IP Hash** | Hash client IP → same server | Sticky sessions (no shared session store) |
| **Random** | Random selection | Simple, stateless |
| **Least Response Time** | Pick server with lowest latency | Performance optimization |

---

## Health Checks

- **Active:** LB periodically sends requests to servers
- **Passive:** LB observes response failures
- **Graceful shutdown:** Drain connections before removing server

---

## Load Balancer Types

| Type | Layer | Examples |
|------|-------|----------|
| **L4 (Transport)** | TCP/UDP, IP, port | AWS NLB, HAProxy (TCP mode) |
| **L7 (Application)** | HTTP, headers, path | AWS ALB, Nginx, HAProxy (HTTP mode) |

**L7** can route by URL path, host header — useful for microservices.

---

## Common Interview Q&A

**Q: Sticky sessions — pros/cons?**  
A: Pros: Simpler (no shared session store). Cons: Uneven load, harder to scale down. Prefer stateless + Redis for sessions.

**Q: Single point of failure?**  
A: Use multiple LBs (active-passive or active-active) with DNS/anycast. AWS: multiple AZs.

**Q: How to add a new server?**  
A: Add to pool, health check passes, LB starts routing. Zero-downtime with proper deployment.
