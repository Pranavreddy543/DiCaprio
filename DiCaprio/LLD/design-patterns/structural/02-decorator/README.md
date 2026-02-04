# Decorator Pattern

> Attaches **additional responsibilities** to an object dynamically. Alternative to subclassing for extending functionality.

---

## When to Use

- Add behavior to individual objects without affecting others
- Behavior can be withdrawn
- Subclassing would lead to explosion of classes (e.g., Coffee: Milk, Sugar, Whip × combinations)

---

## Structure

```
Component (interface)
    |
ConcreteComponent
    |
Decorator extends Component
    |-- wraps Component
    |
ConcreteDecoratorA, ConcreteDecoratorB
```

---

## Example: Coffee with Toppings

```java
interface Coffee {
    double cost();
    String description();
}

class SimpleCoffee implements Coffee {
    public double cost() { return 2.0; }
    public String description() { return "Simple Coffee"; }
}

abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;
    public CoffeeDecorator(Coffee coffee) { this.coffee = coffee; }
}

class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) { super(coffee); }
    public double cost() { return coffee.cost() + 0.5; }
    public String description() { return coffee.description() + ", Milk"; }
}

class WhipDecorator extends CoffeeDecorator {
    public WhipDecorator(Coffee coffee) { super(coffee); }
    public double cost() { return coffee.cost() + 0.7; }
    public String description() { return coffee.description() + ", Whip"; }
}

// Usage - Stack decorators
Coffee coffee = new SimpleCoffee();
coffee = new MilkDecorator(coffee);
coffee = new WhipDecorator(coffee);
System.out.println(coffee.description() + " = $" + coffee.cost());
// "Simple Coffee, Milk, Whip = $3.2"
```

---

## Java I/O Example

```java
// InputStream (Component)
// FileInputStream (ConcreteComponent)
// BufferedInputStream(InputStream) — Decorator
// DataInputStream(InputStream) — Decorator

InputStream is = new BufferedInputStream(
    new FileInputStream("file.txt")
);
```

---

## Common Interview Q&A

**Q: Decorator vs Inheritance?**  
A: Decorator = runtime composition, flexible. Inheritance = compile-time, rigid. Use decorator when combinations explode (Milk+Sugar, Milk+Whip, etc.).

**Q: Decorator vs Strategy?**  
A: Decorator adds behavior by wrapping. Strategy swaps algorithm. Decorator stacks; Strategy replaces.

**Q: Drawback of Decorator?**  
A: Many small classes; debugging can be tricky (stack of wrappers).
