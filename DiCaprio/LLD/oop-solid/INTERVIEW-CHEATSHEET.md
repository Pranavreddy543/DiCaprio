# OOP & SOLID — Interview Cheat Sheet

> Quick reference for OOP rounds.

---

## OOP Pillars

| Pillar | One-liner |
|--------|-----------|
| **Encapsulation** | Hide data; expose via methods |
| **Inheritance** | Subclass reuses superclass |
| **Polymorphism** | Same interface, different behavior |
| **Abstraction** | Hide complexity; expose essentials |

---

## SOLID

| Letter | Principle | One-liner |
|--------|-----------|-----------|
| **S** | Single Responsibility | One reason to change |
| **O** | Open/Closed | Extend, don't modify |
| **L** | Liskov Substitution | Subtype substitutable |
| **I** | Interface Segregation | Small, specific interfaces |
| **D** | Dependency Inversion | Depend on abstractions |

---

## Composition vs Inheritance

- **Inheritance:** is-a, tight coupling
- **Composition:** has-a, inject dependency, flexible
- **Prefer composition** when possible

---

## Interface vs Abstract Class

| | Interface | Abstract Class |
|---|-----------|----------------|
| Multiple | Yes | No |
| Implementation | Default only | Full + abstract |
| State | No | Yes |

---

## Common Q&A

**Q: Encapsulation vs Abstraction?** — Encapsulation = hide data. Abstraction = hide complexity.

**Q: Overloading vs Overriding?** — Overloading = same name, diff params. Overriding = same signature in subclass.

**Q: Why favor composition?** — Loose coupling, flexible, testable, no fragile base class.
