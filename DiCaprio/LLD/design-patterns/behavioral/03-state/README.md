# State Pattern

> Allows an object to **alter its behavior when its internal state changes**. Object appears to change its class.

---

## When to Use

- Object behavior depends on state; state transitions are complex
- Replace many conditionals with state-specific classes
- Finite State Machine (FSM) implementation

---

## Structure

```
Context
  - state: State
  + request() → state.handle()

State (interface)
  + handle(Context)

ConcreteStateA, ConcreteStateB
  + handle() — may transition context to another state
```

---

## Example: Vending Machine

```java
interface VendingState {
    void insertCoin(VendingMachine machine);
    void ejectCoin(VendingMachine machine);
    void selectProduct(VendingMachine machine);
    void dispense(VendingMachine machine);
}

class VendingMachine {
    private VendingState state;
    private int coins;

    public VendingMachine() {
        state = new NoCoinState();
    }
    public void setState(VendingState state) { this.state = state; }
    public void addCoin() { coins++; }
    public int getCoins() { return coins; }
    public void releaseProduct() { coins = 0; }

    public void insertCoin() { state.insertCoin(this); }
    public void ejectCoin() { state.ejectCoin(this); }
    public void selectProduct() { state.selectProduct(this); }
}

class NoCoinState implements VendingState {
    public void insertCoin(VendingMachine m) {
        m.addCoin();
        m.setState(new HasCoinState());
    }
    public void ejectCoin(VendingMachine m) { /* do nothing */ }
    public void selectProduct(VendingMachine m) { /* do nothing */ }
    public void dispense(VendingMachine m) { /* do nothing */ }
}

class HasCoinState implements VendingState {
    public void insertCoin(VendingMachine m) { m.addCoin(); }
    public void ejectCoin(VendingMachine m) {
        m.setState(new NoCoinState());
        m.releaseProduct();
    }
    public void selectProduct(VendingMachine m) {
        m.setState(new DispensingState());
    }
    public void dispense(VendingMachine m) { /* ... */ }
}

class DispensingState implements VendingState {
    public void insertCoin(VendingMachine m) { /* ... */ }
    public void ejectCoin(VendingMachine m) { /* ... */ }
    public void selectProduct(VendingMachine m) { /* ... */ }
    public void dispense(VendingMachine m) {
        m.releaseProduct();
        m.setState(new NoCoinState());
    }
}
```

---

## State vs Strategy

| State | Strategy |
|-------|----------|
| States know each other; transitions | Strategies independent |
| Context delegates to current state | Client chooses strategy |
| Behavior changes with internal state | Algorithm swapped externally |

---

## Common Interview Q&A

**Q: State vs if/else?**  
A: State scales better. Many states + transitions = messy conditionals. State pattern = one class per state, clear transitions.

**Q: Real-world examples?**  
A: TCP connection states, order lifecycle (Pending→Paid→Shipped), workflow engines, game character states.
