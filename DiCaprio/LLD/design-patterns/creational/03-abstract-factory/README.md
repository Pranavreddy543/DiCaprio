# Abstract Factory Pattern

> Provides an interface for creating **families of related objects** without specifying concrete classes.

---

## When to Use

- System must be independent of how products are created
- You need to use multiple related products together (e.g., Button + Scrollbar of same theme)
- Product families must be used together; mixing is not allowed

---

## Structure

```
AbstractFactory
    + createProductA()
    + createProductB()
         |
    +----+----+
    |         |
ConcreteFactory1   ConcreteFactory2
    |         |
    v         v
ProductA1   ProductA2
ProductB1   ProductB2
```

---

## Example: UI Theme (Cross-platform)

```java
// Abstract products
interface Button { void render(); }
interface Scrollbar { void render(); }

// Concrete products - Windows
class WinButton implements Button {
    public void render() { System.out.println("Windows Button"); }
}
class WinScrollbar implements Scrollbar {
    public void render() { System.out.println("Windows Scrollbar"); }
}

// Concrete products - Mac
class MacButton implements Button {
    public void render() { System.out.println("Mac Button"); }
}
class MacScrollbar implements Scrollbar {
    public void render() { System.out.println("Mac Scrollbar"); }
}

// Abstract Factory
interface GUIFactory {
    Button createButton();
    Scrollbar createScrollbar();
}

class WinFactory implements GUIFactory {
    public Button createButton() { return new WinButton(); }
    public Scrollbar createScrollbar() { return new WinScrollbar(); }
}

class MacFactory implements GUIFactory {
    public Button createButton() { return new MacButton(); }
    public Scrollbar createScrollbar() { return new MacScrollbar(); }
}

// Client
class Application {
    private Button button;
    private Scrollbar scrollbar;

    public Application(GUIFactory factory) {
        button = factory.createButton();
        scrollbar = factory.createScrollbar();
    }
    public void render() {
        button.render();
        scrollbar.render();
    }
}
```

---

## Factory Method vs Abstract Factory

| Factory Method | Abstract Factory |
|----------------|------------------|
| One product | Family of products |
| Single method | Multiple creation methods |
| Subclass creates one object | Factory creates many related objects |

---

## Common Interview Q&A

**Q: How to add a new product (e.g., Checkbox) to Abstract Factory?**  
A: Add `createCheckbox()` to AbstractFactory and implement in all concrete factories. Violates Open/Closed Principle — that's a known drawback.

**Q: Real-world examples?**  
A: JDBC (Connection, Statement, ResultSet), UI kits (Material vs Bootstrap), database drivers.
