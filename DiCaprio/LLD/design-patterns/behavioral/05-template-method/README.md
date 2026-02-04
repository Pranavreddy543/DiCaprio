# Template Method Pattern

> Defines the **skeleton of an algorithm** in a method, deferring some steps to subclasses. Lets subclasses redefine certain steps without changing the algorithm structure.

---

## When to Use

- Common algorithm structure; steps vary
- Framework provides hooks for customization
- Avoid code duplication in similar algorithms

---

## Structure

```
AbstractClass
  + templateMethod() {
        step1();
        step2();  // abstract or hook
        step3();
    }
  - step1() { ... }
  - step2() { abstract }
  - step3() { ... }

ConcreteClass extends AbstractClass
  - step2() { ... }  // override
```

---

## Example: Data Parser

```java
abstract class DataParser {
    // Template method
    public final void parse(String file) {
        openFile(file);
        readData();
        processData();   // Hook - subclasses implement
        closeFile();
    }

    private void openFile(String file) {
        System.out.println("Opening " + file);
    }
    private void readData() {
        System.out.println("Reading data");
    }
    protected abstract void processData();
    private void closeFile() {
        System.out.println("Closing file");
    }
}

class CsvParser extends DataParser {
    protected void processData() {
        System.out.println("Processing CSV");
    }
}

class JsonParser extends DataParser {
    protected void processData() {
        System.out.println("Processing JSON");
    }
}

// Usage
DataParser parser = new CsvParser();
parser.parse("data.csv");
```

---

## Hook Methods

- **Abstract:** Must override
- **Hook:** Optional override; default empty or no-op
- **Final:** Cannot override (fixed steps)

---

## Common Interview Q&A

**Q: Template Method vs Strategy?**  
A: Template Method = inheritance, fixed structure. Strategy = composition, entire algorithm swappable. Template = "fill in the blanks"; Strategy = "replace the whole".

**Q: Hollywood Principle?**  
A: "Don't call us, we'll call you." Framework calls your code (hooks); you don't call framework. Inversion of control.

**Q: Real-world examples?**  
A: JUnit (setUp, tearDown, testMethod), Servlet (init, service, destroy), AbstractList.
