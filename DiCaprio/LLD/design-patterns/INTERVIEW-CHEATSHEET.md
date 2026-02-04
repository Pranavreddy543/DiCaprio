# Design Patterns — Interview Cheat Sheet

> Quick reference for last-minute revision. Memorize the **when** and **one-line definition**.

---

## Creational

| Pattern | One-liner | When to Use |
|---------|-----------|-------------|
| **Singleton** | One instance, global access | Logger, DB connection, Config |
| **Factory Method** | Subclass creates object | Multiple product types, defer creation |
| **Abstract Factory** | Family of related products | UI themes, cross-platform kits |
| **Builder** | Step-by-step construction | Many optional params, immutability |
| **Prototype** | Clone instead of create | Expensive object creation |

---

## Structural

| Pattern | One-liner | When to Use |
|---------|-----------|-------------|
| **Adapter** | Make incompatible interfaces work | Legacy integration, third-party |
| **Decorator** | Add behavior by wrapping | Stackable enhancements (logging, caching) |
| **Facade** | Simple interface to complex subsystem | Simplify client API |
| **Proxy** | Control access to object | Lazy init, access control, caching |
| **Composite** | Tree of same-interface objects | File system, UI hierarchy |

---

## Behavioral

| Pattern | One-liner | When to Use |
|---------|-----------|-------------|
| **Observer** | Notify dependents on change | Event systems, pub-sub |
| **Strategy** | Swap algorithm at runtime | Payment methods, sorting |
| **State** | Behavior changes with state | FSM, vending machine |
| **Command** | Encapsulate request as object | Undo/redo, job queues |
| **Template Method** | Skeleton algorithm, hooks for steps | Framework base classes |
| **Chain of Responsibility** | Pass request along handler chain | Middleware, validation |

---

## Pattern Comparison (Interview Favorites)

| Compare | Key Difference |
|---------|----------------|
| **Factory vs Abstract Factory** | Factory = one product; Abstract = family |
| **Adapter vs Facade** | Adapter = change interface; Facade = simplify |
| **Decorator vs Proxy** | Decorator = add behavior; Proxy = control access |
| **Strategy vs State** | Strategy = client chooses; State = auto transitions |
| **Observer vs Pub-Sub** | Observer = direct; Pub-Sub = broker in between |

---

## "Design X" — Which Pattern?

| Problem | Pattern(s) |
|---------|------------|
| Logger (one instance) | Singleton |
| Payment (Card, UPI, Crypto) | Strategy |
| Event notification | Observer |
| Undo in editor | Command |
| Vending machine states | State |
| Third-party API integration | Adapter |
| Add logging to service | Decorator |
| Lazy-load heavy object | Proxy |
| File/folder structure | Composite |
| Cross-platform UI | Abstract Factory |
