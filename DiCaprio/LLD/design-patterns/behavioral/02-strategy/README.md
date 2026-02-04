# Strategy Pattern

> Defines a **family of algorithms**, encapsulates each one, and makes them **interchangeable**. Strategy lets the algorithm vary independently of clients.

---

## When to Use

- Multiple ways to do something (sorting, payment, compression)
- Want to avoid conditional logic (if/switch)
- Runtime algorithm selection

---

## Structure

```
Context
  - strategy: Strategy
  + setStrategy(Strategy)
  + execute() → strategy.execute()

Strategy (interface)
  + execute()

ConcreteStrategyA, ConcreteStrategyB
```

---

## Example: Payment Processing

```java
interface PaymentStrategy {
    void pay(double amount);
}

class CreditCardPayment implements PaymentStrategy {
    private String cardNumber;
    public CreditCardPayment(String card) { this.cardNumber = card; }
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " via Credit Card " + cardNumber);
    }
}

class PayPalPayment implements PaymentStrategy {
    private String email;
    public PayPalPayment(String email) { this.email = email; }
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " via PayPal " + email);
    }
}

class CryptoPayment implements PaymentStrategy {
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " via Crypto");
    }
}

class ShoppingCart {
    private PaymentStrategy strategy;

    public void setPaymentStrategy(PaymentStrategy strategy) {
        this.strategy = strategy;
    }

    public void checkout(double amount) {
        strategy.pay(amount);
    }
}

// Usage - swap strategy at runtime
ShoppingCart cart = new ShoppingCart();
cart.setPaymentStrategy(new CreditCardPayment("1234-5678"));
cart.checkout(99.99);

cart.setPaymentStrategy(new PayPalPayment("user@email.com"));
cart.checkout(49.99);
```

---

## Strategy vs State

| Strategy | State |
|----------|-------|
| Client chooses strategy | State changes automatically (transitions) |
| Algorithms are independent | States know next state |
| No internal state change | Object's behavior changes with state |

---

## Common Interview Q&A

**Q: Strategy vs Polymorphism?**  
A: Strategy is polymorphism + composition. You inject the algorithm; it's a design pattern application of polymorphism.

**Q: Strategy vs Factory?**  
A: Factory creates objects. Strategy selects algorithm. Often used together: Factory creates appropriate Strategy.

**Q: Real-world examples?**  
A: `Comparator` in Java (different sort strategies), `LayoutManager` in Swing, compression algorithms (gzip, zip).
