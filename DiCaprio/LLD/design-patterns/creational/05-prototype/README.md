# Prototype Pattern

> Create new objects by **copying** an existing object (prototype) rather than creating from scratch.

---

## When to Use

- Object creation is expensive (DB calls, complex computation)
- Want to avoid subclassing for object creation
- Need to create copies with slight variations
- When classes are instantiated at runtime (e.g., from config)

---

## Shallow vs Deep Copy

| Shallow | Deep |
|---------|------|
| Copies object + references | Copies object + all nested objects |
| Nested objects shared | Fully independent copy |
| Fast | Slower, more memory |

---

## Example (Java Cloneable)

```java
public class Document implements Cloneable {
    private String title;
    private List<String> pages;

    public Document(String title, List<String> pages) {
        this.title = title;
        this.pages = new ArrayList<>(pages);
    }

    @Override
    public Document clone() {
        try {
            Document doc = (Document) super.clone();
            doc.pages = new ArrayList<>(this.pages);  // Deep copy for list
            return doc;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
}

// Usage
Document original = new Document("Report", Arrays.asList("p1", "p2"));
Document copy = original.clone();
```

---

## Copy Constructor (Alternative)

```java
public class Document {
    private String title;
    private List<String> pages;

    public Document(Document other) {
        this.title = other.title;
        this.pages = new ArrayList<>(other.pages);
    }
}
```

**Pros:** No Cloneable, clearer intent  
**Cons:** Need to maintain for every field

---

## Common Interview Q&A

**Q: Why is Cloneable considered broken in Java?**  
A: Uses `Object.clone()` which does shallow copy; no constructor called; `CloneNotSupportedException` is checked but often meaningless. Prefer copy constructor or factory method.

**Q: When Prototype over Factory?**  
A: When creating from scratch is expensive (e.g., loading from DB) and you need many similar instances. Clone is cheaper than full creation.
