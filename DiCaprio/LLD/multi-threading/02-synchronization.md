# Synchronization

> Protecting shared data from race conditions.

---

## Race Condition

Two threads read-modify-write same variable; result depends on interleaving.

```java
// UNSAFE: count++ is not atomic
count++;  // read, increment, write — can interleave
```

---

## synchronized

- **Method:** Locks on `this` (instance) or class (static)
- **Block:** Locks on specified object
- Only one thread can hold lock; others block

```java
synchronized void increment() {
    count++;
}

// Or block
synchronized (lock) {
    count++;
}
```

**Reentrant:** Same thread can acquire same lock again (e.g., recursive call).

---

## volatile

- **Visibility:** Changes visible to all threads immediately
- **No atomicity:** `volatile count++;` still race — use AtomicInteger
- Use for: flags, double-checked locking

```java
volatile boolean running = true;
// Thread 1: running = false;
// Thread 2: sees it immediately (no cache)
```

---

## ReentrantLock (java.util.concurrent.locks)

Explicit lock with more control than `synchronized`.

```java
ReentrantLock lock = new ReentrantLock();
lock.lock();
try {
    // critical section
} finally {
    lock.unlock();  // MUST unlock in finally
}
```

### Key Features

| Feature | Description |
|---------|-------------|
| **tryLock()** | Non-blocking; returns true if acquired |
| **tryLock(timeout)** | Wait up to timeout; returns true if acquired |
| **lockInterruptibly()** | Blocking lock that responds to interrupt |
| **isFair()** | Fair lock = longest-waiting thread gets lock (slower) |
| **Reentrant** | Same thread can acquire multiple times; must unlock same number of times |

### tryLock — Avoid Deadlock

```java
if (lock.tryLock(1, TimeUnit.SECONDS)) {
    try {
        // do work
    } finally {
        lock.unlock();
    }
} else {
    // couldn't acquire; retry or fail
}
```

### Fair vs Non-Fair (Default)

- **Non-fair:** Higher throughput; may starve waiting threads
- **Fair:** FIFO ordering; no starvation; slower

```java
ReentrantLock fairLock = new ReentrantLock(true);  // fair
```

### Condition (Wait/Notify Alternative)

```java
ReentrantLock lock = new ReentrantLock();
Condition condition = lock.newCondition();

// Wait
lock.lock();
try {
    while (!conditionMet) condition.await();
} finally { lock.unlock(); }

// Signal
lock.lock();
try {
    conditionMet = true;
    condition.signal();  // or signalAll()
} finally { lock.unlock(); }
```

---

## ReadWriteLock (ReentrantReadWriteLock)

- Multiple readers OR single writer
- Use when: Reads >> writes

```java
ReadWriteLock rw = new ReentrantReadWriteLock();
rw.readLock().lock();   // multiple readers allowed
rw.writeLock().lock();  // exclusive; blocks readers too
```

---

## Atomic Classes

- `AtomicInteger`, `AtomicLong`, `AtomicReference`
- Lock-free; uses CAS (Compare-And-Swap)
- `incrementAndGet()`, `compareAndSet()`

```java
AtomicInteger count = new AtomicInteger(0);
count.incrementAndGet();  // thread-safe
```

---

## Common Interview Q&A

**Q: synchronized vs ReentrantLock?**  
A: ReentrantLock: tryLock, fair ordering, multiple conditions. synchronized: simpler, JVM optimized.

**Q: When volatile enough?**  
A: Single read/write (e.g., boolean flag). Not for read-modify-write (need atomic or sync).

**Q: What is CAS?**  
A: Compare-And-Swap: atomic instruction. If current value == expected, set to new; else retry. Used in Atomic* classes.
