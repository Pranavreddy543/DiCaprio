# Composition vs Inheritance

> "Favor composition over inheritance" — Gang of Four

---

## Inheritance (is-a)

- Dog **is a** Animal
- Subclass gets all of superclass
- **Tight coupling:** Change in parent affects all children
- **Fragile base class:** Parent changes can break children

---

## Composition (has-a)

- Car **has a** Engine
- Inject dependency; use interface
- **Loose coupling:** Swap implementations
- **Flexible:** Compose behaviors

```java
// Inheritance
class Bird extends Animal { ... }
class FlyingBird extends Bird { ... }  // What about Penguin?

// Composition
class Bird {
    private FlyBehavior flyBehavior;  // Inject: can fly or not
}
```

---

## When to Use

| Inheritance | Composition |
|-------------|--------------|
| True is-a relationship | Has-a, or behavior varies |
| Shared base behavior | Need to swap implementations |
| LSP holds | More flexible |
| Framework extension points | Most cases |

---

## Design Principle

**"Prefer composition over inheritance"** because:

1. Inheritance breaks encapsulation (subclass depends on superclass internals)
2. Composition allows runtime behavior change
3. Multiple composition vs single inheritance (Java)
4. Easier to test (mock dependencies)

---

## Common Interview Q&A

**Q: When is inheritance OK?**  
A: Clear is-a, LSP holds, framework design (e.g., extending Exception). Template method pattern.

**Q: Example where composition is better?**  
A: Strategy pattern — inject algorithm. Or: Bird with FlyBehavior instead of FlyingBird subclass.
