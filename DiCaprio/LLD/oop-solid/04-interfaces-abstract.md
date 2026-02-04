# Interfaces & Abstract Classes

> When to use each for abstraction.

---

## Interface

- **Contract only** (before Java 8: no implementation)
- Class can implement **multiple** interfaces
- All methods public (implicitly)
- Java 8+: default and static methods

```java
interface Drawable {
    void draw();
    default void print() { System.out.println("Drawing"); }
}
class Circle implements Drawable {
    public void draw() { ... }
}
```

---

## Abstract Class

- **Partial implementation**; abstract methods must be overridden
- Class can extend **one** abstract class
- Can have constructors, instance fields, any visibility
- Use when: Shared base implementation + force subclasses to implement some

```java
abstract class Animal {
    protected String name;
    public Animal(String name) { this.name = name; }
    abstract void speak();
    void sleep() { System.out.println("Zzz"); }  // shared
}
class Dog extends Animal {
    public Dog(String name) { super(name); }
    void speak() { System.out.println("Woof"); }
}
```

---

## Comparison

| | Interface | Abstract Class |
|---|-----------|----------------|
| **Multiple** | Yes | No (single inheritance) |
| **Implementation** | Default methods only | Full methods + abstract |
| **State** | No instance fields | Yes |
| **Constructor** | No | Yes |
| **Use when** | Contract, capability | Shared base + template |

---

## When to Use

**Interface:** Contract, polymorphism, "can do" (Comparable, Runnable). Multiple inheritance of type.

**Abstract class:** Template method, shared code, "is a kind of" with common behavior.

---

## Common Interview Q&A

**Q: Interface vs Abstract class?**  
A: Interface = contract, multiple. Abstract = partial impl, single inheritance, shared state.

**Q: Can interface have implementation?**  
A: Yes — default methods (Java 8+). For backward compatibility.

**Q: When abstract class over interface?**  
A: When you have significant shared implementation and single inheritance is fine.
