# Common Concurrency Patterns

> Producer-Consumer, Reader-Writer, and more.

---

## Producer-Consumer

- Producers add to queue; consumers take
- **BlockingQueue** handles blocking when full/empty

```java
BlockingQueue<Item> queue = new ArrayBlockingQueue<>(100);

// Producer
queue.put(item);  // blocks if full

// Consumer
Item item = queue.take();  // blocks if empty
```

**Thread pool:** ExecutorService is producer-consumer (workers consume from queue).

---

## Reader-Writer Lock

- Multiple readers OR single writer
- Use when reads >> writes

```java
ReadWriteLock rw = new ReentrantReadWriteLock();

// Reader
rw.readLock().lock();
try {
    // read
} finally {
    rw.readLock().unlock();
}

// Writer
rw.writeLock().lock();
try {
    // write
} finally {
    rw.writeLock().unlock();
}
```

---

## Semaphore

- Limit concurrent access (N permits)
- `acquire()` — take permit; blocks if none
- `release()` — return permit

```java
Semaphore sem = new Semaphore(3);  // 3 permits
sem.acquire();  // blocks if 0
try {
    // critical section
} finally {
    sem.release();
}
```

**Use:** Rate limiting, connection pool, bounded resource.

---

## CountDownLatch

- One-time barrier; count down to zero
- `await()` blocks until count reaches 0

```java
CountDownLatch latch = new CountDownLatch(N);
// N threads do work, each calls latch.countDown()
latch.await();  // main thread waits for all
```

**Use:** Wait for N tasks to complete.

---

## CyclicBarrier

- Reusable barrier; all threads wait at barrier
- When all arrive, barrier trips; all proceed

```java
CyclicBarrier barrier = new CyclicBarrier(N, () -> System.out.println("All arrived"));
// N threads call barrier.await(); all block until Nth arrives
```

**Use:** Multi-phase computation; parallel merge.

---

## Common Interview Q&A

**Q: Producer-Consumer — what if producer is faster?**  
A: Bounded queue; producer blocks when full (backpressure). Or drop (discard policy).

**Q: Semaphore vs Mutex?**  
A: Mutex = binary semaphore (1 permit). Semaphore can have N permits.

**Q: CountDownLatch vs CyclicBarrier?**  
A: CountDownLatch: one-time, count down. CyclicBarrier: reusable, all wait at same point.
