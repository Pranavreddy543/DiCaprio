# Deadlock & Race Conditions

> How they occur and how to prevent them.

---

## Deadlock

Four conditions (all must hold):

1. **Mutual exclusion** — Resource can't be shared
2. **Hold and wait** — Thread holds resource, waits for another
3. **No preemption** — Can't force release
4. **Circular wait** — T1 waits for T2, T2 waits for T1

---

## Example Deadlock

```java
// Thread 1: lock A, then B
synchronized (lockA) {
    synchronized (lockB) { ... }
}

// Thread 2: lock B, then A
synchronized (lockB) {
    synchronized (lockA) { ... }
}
// Circular wait!
```

---

## Prevention

### 1. Lock Ordering (Most Common)

Always acquire locks in same order (e.g., A before B).

```java
// Both threads: lock A first, then B
synchronized (lockA) {
    synchronized (lockB) { ... }
}
```

### 2. tryLock with Timeout

```java
if (lock1.tryLock() && lock2.tryLock()) {
    try { /* ... */ }
    finally { lock2.unlock(); lock1.unlock(); }
}
```

### 3. Single Lock

Use one lock for all resources (simpler, less concurrency).

### 4. Avoid Nested Locks

Restructure to minimize lock nesting.

---

## Race Condition

- Outcome depends on thread scheduling
- Fix: Synchronization (synchronized, atomic, locks)

---

## Liveness Issues

| Issue | Description |
|-------|-------------|
| **Deadlock** | All blocked, waiting for each other |
| **Livelock** | Threads keep retrying, no progress |
| **Starvation** | Thread never gets CPU/lock |
| **Priority inversion** | Low-priority holds lock needed by high-priority |

---

## Common Interview Q&A

**Q: How to detect deadlock?**  
A: `jstack` (thread dump), or `ThreadMXBean.findDeadlockedThreads()`.

**Q: How to avoid deadlock?**  
A: Lock ordering. Or use tryLock with backoff.

**Q: What is livelock?**  
A: Like deadlock but threads are "active" — e.g., both step aside for each other repeatedly. Fix: random backoff.
