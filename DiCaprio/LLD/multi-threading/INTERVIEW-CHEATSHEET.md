# Multi-threading — Interview Cheat Sheet

> Quick reference for concurrency rounds.

---

## Key Concepts

| Concept | One-liner |
|---------|-----------|
| **Race condition** | Outcome depends on interleaving; fix with sync |
| **Deadlock** | Circular wait; fix with lock ordering |
| **synchronized** | One thread at a time; reentrant |
| **volatile** | Visibility across threads; not atomicity |
| **AtomicInteger** | Lock-free increment; uses CAS |

---

## Lock Ordering (Deadlock Prevention)

Always acquire locks in same order (e.g., A before B).

---

## Java Concurrency Summary

| Class | Use |
|-------|-----|
| synchronized | Method/block lock |
| volatile | Visibility |
| AtomicInteger | Lock-free counter |
| **ReentrantLock** | Explicit lock; tryLock, fair, Condition |
| ReadWriteLock | Multiple readers, single writer |
| BlockingQueue | Producer-consumer |
| **ConcurrentHashMap** | Thread-safe map; per-bucket lock |
| **CopyOnWriteArrayList** | Thread-safe list; read-heavy |
| **AtomicIntegerArray** | Thread-safe array of ints |
| ExecutorService | Thread pool |
| CountDownLatch | Wait for N events |
| Semaphore | Limit N concurrent |

---

## ReentrantLock Quick Ref

- `lock()` / `unlock()` — always unlock in finally
- `tryLock()` — non-blocking; returns boolean
- `tryLock(timeout)` — wait with timeout; avoids deadlock
- `new ReentrantLock(true)` — fair lock (FIFO)
- `lock.newCondition()` — await/signal (replaces wait/notify)

---

## Concurrent Collections Quick Ref

| Collection | Use Case |
|------------|----------|
| ConcurrentHashMap | Thread-safe map; high concurrency |
| CopyOnWriteArrayList | Read-heavy list; few writes |
| Collections.synchronizedList | Basic thread-safe list; full lock |
| AtomicIntegerArray | Array of atomic counters |
| BlockingQueue | Producer-consumer |

---

## Common Q&A

**Q: start() vs run()?** — start() creates thread; run() executes in current.

**Q: synchronized vs ReentrantLock?** — ReentrantLock: tryLock, fair, Condition, explicit. synchronized: simpler, JVM optimized.

**Q: volatile enough for counter?** — No. Use AtomicInteger or synchronized.

**Q: How to avoid deadlock?** — Lock ordering; tryLock with timeout.

**Q: ConcurrentHashMap internal?** — Java 8+: CAS + synchronized per bucket. No full-map lock.
