# Proxy Pattern

> Provides a **surrogate or placeholder** for another object to control access to it.

---

## When to Use

- Lazy initialization (expensive object)
- Access control (permissions)
- Logging, caching
- Remote proxy (RPC, stub)
- Protection proxy (validate before delegating)

---

## Types of Proxy

| Type | Purpose |
|------|---------|
| **Virtual** | Lazy initialization |
| **Protection** | Access control |
| **Remote** | Local representative for remote object |
| **Caching** | Cache results |

---

## Example: Lazy Image Loading

```java
interface Image {
    void display();
}

class RealImage implements Image {
    private String filename;
    public RealImage(String filename) {
        this.filename = filename;
        loadFromDisk();
    }
    private void loadFromDisk() {
        System.out.println("Loading " + filename);
    }
    public void display() {
        System.out.println("Displaying " + filename);
    }
}

class ProxyImage implements Image {
    private RealImage realImage;
    private String filename;

    public ProxyImage(String filename) {
        this.filename = filename;
    }

    public void display() {
        if (realImage == null) {
            realImage = new RealImage(filename);  // Lazy load
        }
        realImage.display();
    }
}

// Client - RealImage only loaded when display() is called
Image image = new ProxyImage("photo.jpg");
// ... no load yet ...
image.display();  // Now loads and displays
```

---

## Proxy vs Decorator

| Proxy | Decorator |
|-------|-----------|
| Controls access | Adds behavior |
| Same interface, may not add functionality | Adds functionality |
| Manages lifecycle (lazy init) | Wraps for enhancement |

---

## Common Interview Q&A

**Q: Java Dynamic Proxy?**  
A: `java.lang.reflect.Proxy` — creates proxy at runtime for interfaces. Used in Spring AOP, Hibernate lazy loading.

**Q: Proxy vs Decorator in practice?**  
A: Proxy = "gatekeeper" (lazy, access control). Decorator = "enhancer" (adds behavior like logging, caching that modifies output).
