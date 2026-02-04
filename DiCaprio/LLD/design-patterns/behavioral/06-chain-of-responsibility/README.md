# Chain of Responsibility Pattern

> Pass a request along a **chain of handlers**. Each handler decides whether to process the request or pass it to the next handler.

---

## When to Use

- Multiple objects can handle a request; handler unknown a priori
- Want to decouple sender from receivers
- Middleware, validation pipelines, logging

---

## Structure

```
Handler (interface)
  - next: Handler
  + setNext(Handler)
  + handle(Request)
       |
       +-- ConcreteHandlerA, ConcreteHandlerB
            each may handle or pass to next
```

---

## Example: Validation Pipeline

```java
abstract class Validator {
    protected Validator next;

    public Validator setNext(Validator next) {
        this.next = next;
        return next;
    }

    public abstract boolean validate(String input);
}

class NotEmptyValidator extends Validator {
    public boolean validate(String input) {
        if (input == null || input.isEmpty()) {
            System.out.println("Validation failed: empty");
            return false;
        }
        return next == null || next.validate(input);
    }
}

class LengthValidator extends Validator {
    private int min, max;
    public LengthValidator(int min, int max) { this.min = min; this.max = max; }
    public boolean validate(String input) {
        if (input.length() < min || input.length() > max) {
            System.out.println("Validation failed: length");
            return false;
        }
        return next == null || next.validate(input);
    }
}

class EmailValidator extends Validator {
    public boolean validate(String input) {
        if (!input.contains("@")) {
            System.out.println("Validation failed: not email");
            return false;
        }
        return next == null || next.validate(input);
    }
}

// Usage - build chain
Validator chain = new NotEmptyValidator();
chain.setNext(new LengthValidator(5, 50)).setNext(new EmailValidator());
chain.validate("user@example.com");  // true
chain.validate("ab");                 // false (length)
```

---

## Common Interview Q&A

**Q: Chain of Responsibility vs Decorator?**  
A: CoR = one handler processes (or passes). Decorator = all wrappers execute (each adds behavior). CoR stops at first handler; Decorator chains through all.

**Q: Real-world examples?**  
A: Servlet filters, Spring Security filter chain, exception handling (catch blocks), middleware (Express.js).
