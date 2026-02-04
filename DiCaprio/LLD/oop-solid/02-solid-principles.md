# SOLID Principles

> Five principles for maintainable, extensible design.

---

## S — Single Responsibility (SRP)

**A class should have one reason to change.**

- One responsibility per class
- If you add "and" to describe the class, it might violate SRP

```java
// BAD: User handles persistence + validation + email
class User {
    void save() { ... }
    void validate() { ... }
    void sendEmail() { ... }
}

// GOOD: Separate concerns
class User { ... }
class UserRepository { void save(User u) { ... } }
class UserValidator { boolean validate(User u) { ... } }
```

---

## O — Open/Closed (OCP)

**Open for extension; closed for modification.**

- Add new behavior via new classes, not by changing existing
- Use interfaces/abstract classes; inject implementations

```java
// BAD: Modify for each new shape
double area(Shape s) {
    if (s instanceof Circle) return ...;
    if (s instanceof Square) return ...;
}

// GOOD: New shape = new class
interface Shape { double area(); }
class Circle implements Shape { ... }
class Square implements Shape { ... }
```

---

## L — Liskov Substitution (LSP)

**Subtypes must be substitutable for their base types.**

- Subclass should not break superclass contract
- Don't override with weaker preconditions or stronger postconditions

```java
// BAD: Square extends Rectangle but setWidth/setHeight break expectation
class Rectangle { setWidth(w); setHeight(h); }
class Square extends Rectangle {
    setWidth(w) { setHeight(w); }  // Surprise! Violates LSP
}

// GOOD: Don't force Square to extend Rectangle if behavior differs
```

---

## I — Interface Segregation (ISP)

**Many specific interfaces better than one general.**

- Clients shouldn't depend on methods they don't use
- Split fat interface into smaller ones

```java
// BAD: Worker must implement eat() even if robot doesn't eat
interface Worker {
    void work();
    void eat();
}

// GOOD: Segregate
interface Workable { void work(); }
interface Eatable { void eat(); }
class Human implements Workable, Eatable { ... }
class Robot implements Workable { ... }
```

---

## D — Dependency Inversion (DIP)

**Depend on abstractions, not concretions.**

- High-level modules shouldn't depend on low-level; both depend on abstraction
- Inject dependencies (constructor, setter)

```java
// BAD: OrderService depends on concrete StripePayment
class OrderService {
    private StripePayment payment = new StripePayment();
}

// GOOD: Depend on interface
class OrderService {
    private PaymentProcessor payment;
    OrderService(PaymentProcessor p) { this.payment = p; }
}
```

---

## Common Interview Q&A

**Q: Most important SOLID principle?**  
A: DIP and OCP often cited — they enable extensibility. SRP is foundational.

**Q: Real-world example of OCP?**  
A: Plugin systems, strategy pattern — add new strategy without changing context.

**Q: LSP violation example?**  
A: Square extending Rectangle (setWidth changes height). Or throwing in override when base doesn't.
