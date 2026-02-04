# Thread Pools

> Reuse threads instead of creating new ones. Executor framework.

---

## Why Thread Pools?

- **Overhead:** Creating thread is expensive
- **Resource limit:** Too many threads → OOM, context switch
- **Reuse:** Pool of workers; submit tasks

---

## ExecutorService (Java)

```java
ExecutorService executor = Executors.newFixedThreadPool(4);
executor.submit(() -> System.out.println("Task"));
executor.shutdown();  // no new tasks; wait for existing
executor.awaitTermination(10, TimeUnit.SECONDS);
```

---

## Pool Types

| Type | Description | Use Case |
|------|-------------|----------|
| **Fixed** | Fixed N threads | Bounded concurrency |
| **Cached** | Grows; idle threads removed | Short bursty tasks |
| **Single** | One thread | Sequential processing |
| **Scheduled** | Delay, period | Cron-like, retry |
| **Custom** | ThreadPoolExecutor | Full control |

---

## ThreadPoolExecutor Parameters

```java
new ThreadPoolExecutor(
    corePoolSize,      // Min threads to keep
    maximumPoolSize,   // Max threads
    keepAliveTime,     // Idle thread timeout
    unit,
    workQueue,         // BlockingQueue for pending tasks
    threadFactory,
    rejectionHandler   // When queue full
);
```

**Flow:** Core threads → Queue → Extra threads (up to max) → Reject

---

## Rejection Policies

| Policy | Behavior |
|--------|----------|
| AbortPolicy | Throws RejectedExecutionException |
| CallerRunsPolicy | Caller runs task (backpressure) |
| DiscardPolicy | Silently discard |
| DiscardOldestPolicy | Discard oldest, retry new |

---

## Common Interview Q&A

**Q: corePoolSize vs maxPoolSize?**  
A: Core = always kept. Extra created when queue full, up to max. After idle, extra removed (keepAliveTime).

**Q: Bounded vs unbounded queue?**  
A: Unbounded (LinkedBlockingQueue) — can grow; risk OOM. Bounded — full → reject or create more threads (up to max).

**Q: When to use which pool?**  
A: Fixed for predictable load. Cached for bursty. Custom when you need control.
