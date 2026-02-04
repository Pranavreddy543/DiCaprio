# Creational Design Patterns

> Deal with **object creation** — how to instantiate objects in a flexible, reusable way.

---

## Patterns Overview

| Pattern | Purpose | When to Use |
|---------|---------|-------------|
| **Singleton** | One instance of a class | Logger, DB connection, Config |
| **Factory Method** | Defer object creation to subclasses | Multiple product types |
| **Abstract Factory** | Families of related objects | UI kits, theme systems |
| **Builder** | Step-by-step complex object construction | Immutable objects, many optional params |
| **Prototype** | Clone objects instead of creating new | Expensive object creation |

---

## Interview Focus

- **Singleton**: Thread-safety (double-checked locking, enum), when NOT to use (testing, scalability)
- **Factory vs Abstract Factory**: Factory = one product hierarchy; Abstract Factory = multiple families
- **Builder**: Fluent API, immutability, vs telescoping constructors
