# Observer Pattern

> Defines a **one-to-many dependency** — when one object (Subject) changes state, all dependents (Observers) are notified and updated.

---

## When to Use

- Event-driven systems
- When change to one object requires changing others (unknown number)
- Loose coupling between subject and observers

---

## Structure

```
Subject                    Observer (interface)
  - observers: List          + update()
  + attach(Observer)
  + detach(Observer)
  + notify()
         |
         +-- notifies --> ConcreteObserverA, ConcreteObserverB
```

---

## Example: Stock Price Notifier

```java
interface Observer {
    void update(String stock, double price);
}

interface Subject {
    void attach(Observer o);
    void detach(Observer o);
    void notifyObservers();
}

class StockMarket implements Subject {
    private Map<String, Double> prices = new HashMap<>();
    private List<Observer> observers = new ArrayList<>();

    public void setPrice(String stock, double price) {
        prices.put(stock, price);
        notifyObservers();
    }

    public void attach(Observer o) { observers.add(o); }
    public void detach(Observer o) { observers.remove(o); }

    public void notifyObservers() {
        for (String stock : prices.keySet()) {
            double price = prices.get(stock);
            for (Observer o : observers) {
                o.update(stock, price);
            }
        }
    }
}

class EmailAlert implements Observer {
    public void update(String stock, double price) {
        System.out.println("Email: " + stock + " = $" + price);
    }
}

class SMSAlert implements Observer {
    public void update(String stock, double price) {
        System.out.println("SMS: " + stock + " = $" + price);
    }
}

// Usage
StockMarket market = new StockMarket();
market.attach(new EmailAlert());
market.attach(new SMSAlert());
market.setPrice("GOOG", 150.0);  // Both get notified
```

---

## Push vs Pull Model

- **Push:** Subject sends full data in `update(data)`
- **Pull:** Subject sends self; Observer fetches what it needs via getters

---

## Common Interview Q&A

**Q: Observer vs Pub-Sub?**  
A: Observer = direct coupling (Subject knows Observers). Pub-Sub = broker in between (Publisher → Broker → Subscriber); decoupled.

**Q: Memory leak with Observer?**  
A: Yes — if Observer not detached, Subject holds reference. Always detach when Observer is no longer needed.

**Q: Java built-in Observer?**  
A: `java.util.Observable` and `Observer` — deprecated in Java 9. Use `PropertyChangeListener` or reactive libs (RxJava, Reactor).
