# Builder Pattern

> Separates the construction of a complex object from its representation, allowing the **same construction process** to create different representations.

---

## When to Use

- Object has many optional parameters (telescoping constructor problem)
- Immutable object with many fields
- Step-by-step construction with validation
- Same construction steps, different representations

---

## Telescoping Constructor Problem

```java
// BAD: Unreadable, error-prone
public User(String name) { ... }
public User(String name, int age) { ... }
public User(String name, int age, String email) { ... }
public User(String name, int age, String email, String phone) { ... }
// Which param is what? Easy to mix up.
```

---

## Builder Solution

```java
public class User {
    private final String name;
    private final int age;
    private final String email;
    private final String phone;

    private User(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
        this.email = builder.email;
        this.phone = builder.phone;
    }

    public static class Builder {
        private final String name;  // Required
        private int age;
        private String email;
        private String phone;

        public Builder(String name) {
            this.name = name;
        }

        public Builder age(int age) {
            this.age = age;
            return this;
        }
        public Builder email(String email) {
            this.email = email;
            return this;
        }
        public Builder phone(String phone) {
            this.phone = phone;
            return this;
        }

        public User build() {
            if (name == null || name.isEmpty())
                throw new IllegalArgumentException("Name required");
            return new User(this);
        }
    }
}

// Usage - Fluent, readable
User user = new User.Builder("John")
    .age(30)
    .email("john@example.com")
    .build();
```

---

## Pros & Cons

**Pros:** Readable, immutable, optional params, validation in `build()`  
**Cons:** More code, verbose for simple objects

---

## Builder vs Factory

| Builder | Factory |
|---------|---------|
| Step-by-step construction | One-shot creation |
| Same process, different config | Different product types |
| Many optional params | Usually fixed params |

---

## Common Interview Q&A

**Q: StringBuilder — is it Builder pattern?**  
A: Not exactly. Builder pattern builds final object in one go. StringBuilder is more like a mutable accumulator. But the "fluent API" style is similar.

**Q: Lombok @Builder — how does it work?**  
A: Compile-time code generation; creates inner Builder class and `builder()` static method.

**Q: When to use Builder over constructor?**  
A: When you have 4+ parameters, many optional, or need validation/immutability.
