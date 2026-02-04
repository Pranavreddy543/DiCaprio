# Singleton Pattern

> Ensures a class has **only one instance** and provides a global point of access to it.

---

## When to Use

- Logger, Configuration, Database connection pool
- Caching, Thread pool
- When exactly one object is needed to coordinate actions across the system

---

## When NOT to Use

- **Testing** — Hard to mock; prefer dependency injection
- **Distributed systems** — One instance per JVM, not across cluster
- **Stateless services** — No need for single instance

---

## Implementation Approaches

### 1. Eager Initialization (Simple, thread-safe)

```java
public class Logger {
    private static final Logger INSTANCE = new Logger();

    private Logger() {}

    public static Logger getInstance() {
        return INSTANCE;
    }
}
```

**Pros:** Simple, thread-safe  
**Cons:** Instance created at class load (even if never used)

---

### 2. Lazy Initialization (Thread-unsafe — avoid)

```java
public class Logger {
    private static Logger instance;

    private Logger() {}

    public static Logger getInstance() {
        if (instance == null) {
            instance = new Logger();  // Race condition!
        }
        return instance;
    }
}
```

---

### 3. Double-Checked Locking (Lazy + Thread-safe)

```java
public class Logger {
    private static volatile Logger instance;

    private Logger() {}

    public static Logger getInstance() {
        if (instance == null) {
            synchronized (Logger.class) {
                if (instance == null) {
                    instance = new Logger();
                }
            }
        }
        return instance;
    }
}
```

**Why `volatile`?** Prevents instruction reordering; ensures visibility across threads.

---

### 4. Bill Pugh / Static Holder (Recommended)

```java
public class Logger {
    private Logger() {}

    private static class Holder {
        private static final Logger INSTANCE = new Logger();
    }

    public static Logger getInstance() {
        return Holder.INSTANCE;
    }
}
```

**Pros:** Lazy load, thread-safe (JVM guarantees), no explicit synchronization

---

### 5. Enum (Simplest, reflection-safe)

```java
public enum Logger {
    INSTANCE;

    public void log(String msg) {
        System.out.println(msg);
    }
}
```

**Pros:** Serialization-safe, reflection-safe, concise  
**Cons:** Cannot lazy-load; cannot extend a class

---

## Common Interview Q&A

**Q: Why is Singleton considered an anti-pattern?**  
A: Hard to unit test (global state), hides dependencies, can violate Single Responsibility. Prefer DI when possible.

**Q: How to make Singleton thread-safe?**  
A: Eager init, Bill Pugh holder, or double-checked locking with `volatile`.

**Q: Can Singleton be broken by reflection?**  
A: Yes (except enum). Can defend by throwing in constructor if instance exists.

**Q: Singleton in distributed system?**  
A: One per JVM. For cluster-wide singleton, use distributed lock/cache (Redis, Zookeeper).
