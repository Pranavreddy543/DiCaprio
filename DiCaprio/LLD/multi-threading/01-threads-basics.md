# Threads & Basics

> Process vs Thread, creating threads, lifecycle.

---

## Process vs Thread

| Process | Thread |
|---------|--------|
| Independent program | Unit of execution within process |
| Own memory space | Shares process memory |
| Heavy (context switch) | Light (same address space) |
| Isolated | Can share data (need sync) |

---

## Creating Threads (Java)

### 1. Extend Thread

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Running");
    }
}
new MyThread().start();
```

### 2. Implement Runnable (Preferred)

```java
Thread t = new Thread(() -> System.out.println("Running"));
t.start();
```

### 3. Implement Callable (Returns value)

```java
Callable<Integer> task = () -> 42;
Future<Integer> future = executor.submit(task);
int result = future.get();
```

---

## Thread Lifecycle

```
NEW → RUNNABLE → RUNNING
         ↑  |         |
         |  |         v
         |  |     BLOCKED/WAITING/TIMED_WAITING
         |  |         |
         |  +---------+
         |
      TERMINATED
```

---

## Key Methods

| Method | Description |
|--------|-------------|
| `start()` | Starts thread; calls `run()` |
| `run()` | Entry point; don't call directly |
| `join()` | Wait for thread to finish |
| `sleep(ms)` | Pause; doesn't release lock |
| `yield()` | Hint to scheduler; may give up CPU |
| `interrupt()` | Request interruption |

---

## Common Interview Q&A

**Q: start() vs run()?**  
A: `start()` creates new thread and invokes `run()`. Calling `run()` directly executes in current thread (no new thread).

**Q: How to pass data to thread?**  
A: Constructor, or pass to Runnable/Callable. Shared data needs synchronization.
