# Factory Method Pattern

> Defines an interface for creating an object, but lets **subclasses** decide which class to instantiate.

---

## When to Use

- You don't know exact types at compile time
- Creation logic is complex or varies by subclass
- You want to extend with new product types without changing client code

---

## Structure

```
Creator (abstract)          Product (interface)
    |                              |
    +-- factoryMethod() --> creates Product
    |
ConcreteCreatorA  -->  ConcreteProductA
ConcreteCreatorB  -->  ConcreteProductB
```

---

## Example: Document Creator

```java
// Product
interface Document {
    void open();
    void save();
}

class PdfDocument implements Document {
    public void open() { /* ... */ }
    public void save() { /* ... */ }
}

class WordDocument implements Document {
    public void open() { /* ... */ }
    public void save() { /* ... */ }
}

// Creator
abstract class DocumentCreator {
    abstract Document createDocument();

    public void newDocument() {
        Document doc = createDocument();
        doc.open();
    }
}

class PdfCreator extends DocumentCreator {
    Document createDocument() { return new PdfDocument(); }
}

class WordCreator extends DocumentCreator {
    Document createDocument() { return new WordDocument(); }
}
```

---

## Factory Method vs Simple Factory

| Factory Method | Simple Factory |
|----------------|----------------|
| Subclasses decide | One class has switch/if-else |
| Extensible via inheritance | Extensible via new cases (violates OCP) |
| Follows OCP | Often doesn't |

---

## Common Interview Q&A

**Q: Factory Method vs Abstract Factory?**  
A: Factory Method = one product per creator. Abstract Factory = family of related products (e.g., WinButton + WinScrollbar).

**Q: When would you use Factory Method over constructor?**  
A: When creation logic is complex, involves dependencies, or varies by context (e.g., different DB for test vs prod).
