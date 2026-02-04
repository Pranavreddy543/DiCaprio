# Concurrent Collections

> Thread-safe data structures from java.util.concurrent.

---

## ConcurrentHashMap

Thread-safe HashMap; **no full-map lock** — allows concurrent reads and writes.

### How It Works

- **Java 7:** Segment-based locking (16 segments by default); lock per segment
- **Java 8+:** CAS + synchronized on bucket head; finer granularity, no segments
- **Operations:** `put`, `get`, `remove`, `compute`, `merge` — all atomic
- **No null** keys or values (ambiguity with get() returning null)

### Key Methods

```java
ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();

map.put("a", 1);
map.get("a");
map.compute("a", (k, v) -> v == null ? 1 : v + 1);  // atomic increment
map.merge("b", 1, Integer::sum);                     // atomic merge
map.putIfAbsent("c", 10);                             // add only if absent
map.remove("a", 1);                                   // remove if value matches
```

### ConcurrentHashMap vs Hashtable vs synchronized Map

| | ConcurrentHashMap | Hashtable | Collections.synchronizedMap |
|--|-------------------|-----------|-----------------------------|
| Lock granularity | Per bucket | Entire map | Entire map |
| Concurrency | High | Low | Low |
| Null | No | No | Allowed (but get null = ambiguous) |

---

## Concurrent List

### CopyOnWriteArrayList

- **Thread-safe list** for **read-heavy** workloads
- **Write:** Copies entire array; expensive (O(n))
- **Read:** No lock; iterators are snapshot (don't throw ConcurrentModificationException)
- **Use:** Listener lists, config, rarely modified data

```java
List<String> list = new CopyOnWriteArrayList<>();
list.add("a");           // copies array
list.get(0);             // no lock
for (String s : list) {} // safe iterator (snapshot)
```

### Collections.synchronizedList

- Wraps ArrayList; **locks entire list** on every operation
- Iterator needs external sync: `synchronized (list) { for (...) }`
- Use when: Need ArrayList semantics + basic thread-safety; lower concurrency OK

```java
List<String> list = Collections.synchronizedList(new ArrayList<>());
synchronized (list) {
    for (String s : list) { /* ... */ }
}
```

### ConcurrentLinkedQueue (Queue, not List)

- Lock-free linked queue; thread-safe
- No random access; use when FIFO queue needed

---

## Concurrent Array

Java has no "ConcurrentArrayList" for random-access array. Options:

### AtomicIntegerArray / AtomicLongArray / AtomicReferenceArray

- Thread-safe **array of primitives/references**
- Each element updated atomically via CAS
- Use: Counters per index, parallel counters

```java
AtomicIntegerArray arr = new AtomicIntegerArray(10);
arr.set(0, 5);
arr.getAndIncrement(0);      // atomic
arr.compareAndSet(0, 5, 6);  // atomic conditional update
```

### CopyOnWriteArrayList

- Array-backed; use when list semantics + concurrent reads needed.

---

## BlockingQueue

- Queue with blocking `put()` (full) and `take()` (empty)
- **Producer-Consumer** pattern

| Implementation | Bounded | Ordering |
|-----------------|---------|----------|
| ArrayBlockingQueue | Yes | FIFO |
| LinkedBlockingQueue | Optional | FIFO |
| PriorityBlockingQueue | No | Priority |
| SynchronousQueue | 0 capacity | Handoff |

```java
BlockingQueue<Task> queue = new ArrayBlockingQueue<>(100);
queue.put(task);   // blocks if full
Task t = queue.take();  // blocks if empty
```

---

## CopyOnWriteArrayList

- Thread-safe list for read-heavy
- **Write:** Copy entire array; expensive
- **Read:** No lock; iterators don't throw CME
- Use: Listener lists, rarely modified

---

## Concurrent Collections vs synchronized

| synchronized Collection | Concurrent Collection |
|------------------------|----------------------|
| Lock entire structure | Fine-grained / lock-free |
| Blocking | Non-blocking (mostly) |
| Iterator needs external sync | Safe iterator (weak consistency) |

---

## Common Interview Q&A

**Q: ConcurrentHashMap vs Hashtable?**  
A: ConcurrentHashMap: per-bucket locking, high concurrency. Hashtable: locks entire map on every operation.

**Q: Why no null in ConcurrentHashMap?**  
A: Ambiguity — get() returns null: absent or value was null? Can't distinguish atomically.

**Q: When CopyOnWriteArrayList?**  
A: Read-heavy, few writes. Listeners, config. Avoid for write-heavy (copy cost).

**Q: CopyOnWriteArrayList vs synchronizedList?**  
A: CopyOnWrite: lock-free reads, copy on write. SynchronizedList: locks entire list; iterator needs external sync.

**Q: When AtomicIntegerArray?**  
A: Array of counters updated by multiple threads; need per-element atomic updates.
