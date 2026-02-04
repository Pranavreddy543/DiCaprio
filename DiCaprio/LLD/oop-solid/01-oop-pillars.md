# OOP Pillars

> Four pillars of Object-Oriented Programming.

---

## 1. Encapsulation

**Bundle data and methods; hide internal state.**

- Use `private` for fields; expose via `public` getters/setters
- Protects invariants (e.g., age can't be negative)
- Reduces coupling; caller doesn't depend on internals

```java
class BankAccount {
    private double balance;  // hidden

    public void deposit(double amount) {
        if (amount > 0) balance += amount;
    }
    public double getBalance() { return balance; }
}
```

---

## 2. Inheritance

**Subclass inherits from superclass (is-a relationship).**

- Code reuse; override methods
- **Liskov Substitution:** Subtype must be substitutable for supertype (see SOLID)

```java
class Animal {
    void speak() { System.out.println("..."); }
}
class Dog extends Animal {
    @Override
    void speak() { System.out.println("Woof"); }
}
```

**Prefer composition** when "has-a" is more natural than "is-a".

---

## 3. Polymorphism

**Same interface; different behavior.**

- **Compile-time (overloading):** Same method name, different params
- **Runtime (overriding):** Subclass overrides; call resolved at runtime

```java
Animal a = new Dog();
a.speak();  // "Woof" — runtime polymorphism
```

**Benefit:** Write code against interface; add new types without changing client.

---

## 4. Abstraction

**Hide complexity; expose only what's needed.**

- **Abstract class:** Partial implementation; subclasses complete
- **Interface:** Contract only; no implementation (before Java 8)
- **Data abstraction:** Hide representation (e.g., List hides array vs linked)

```java
interface PaymentProcessor {
    void pay(double amount);
}
// Client depends on abstraction, not Stripe/PayPal
```

---

## Common Interview Q&A

**Q: Encapsulation vs Abstraction?**  
A: Encapsulation = hiding data (private). Abstraction = hiding complexity (interface, abstract class). Related but distinct.

**Q: Overloading vs Overriding?**  
A: Overloading = same name, different params; compile-time. Overriding = same signature in subclass; runtime.

**Q: What is polymorphism?**  
A: One interface, many forms. Call same method; different behavior based on actual object type.
