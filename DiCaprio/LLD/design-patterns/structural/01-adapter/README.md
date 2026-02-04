# Adapter Pattern

> Converts the interface of a class into another interface clients expect. Lets classes work together that couldn't otherwise.

---

## When to Use

- Integrate legacy/third-party code with different interface
- Reuse existing class that doesn't match required interface
- Create reusable class that collaborates with unrelated classes

---

## Types

- **Object Adapter** — Composition (holds adaptee)
- **Class Adapter** — Multiple inheritance (Java: not possible; use composition)

---

## Example: Third-party Payment to Our Interface

```java
// Our expected interface
interface PaymentProcessor {
    void pay(double amount);
}

// Third-party library (incompatible)
class StripeAPI {
    public void chargePayment(int amountInCents) {
        System.out.println("Charged " + amountInCents + " cents via Stripe");
    }
}

// Adapter
class StripeAdapter implements PaymentProcessor {
    private StripeAPI stripe;

    public StripeAdapter(StripeAPI stripe) {
        this.stripe = stripe;
    }

    @Override
    public void pay(double amount) {
        int cents = (int) (amount * 100);
        stripe.chargePayment(cents);
    }
}

// Client uses PaymentProcessor — doesn't know about Stripe
PaymentProcessor processor = new StripeAdapter(new StripeAPI());
processor.pay(29.99);
```

---

## Adapter vs Decorator vs Proxy

| Adapter | Decorator | Proxy |
|---------|-----------|-------|
| Changes interface | Adds behavior | Controls access |
| Wraps incompatible | Wraps same type | Same interface as subject |
| One adaptee | Can stack | One-to-one |

---

## Common Interview Q&A

**Q: Adapter vs Facade?**  
A: Adapter changes one interface to another. Facade provides simplified interface to complex subsystem (many classes).

**Q: Real-world examples?**  
A: JDBC-ODBC bridge, InputStreamReader (adapts InputStream to Reader), SLF4J (adapts various logging libs).
