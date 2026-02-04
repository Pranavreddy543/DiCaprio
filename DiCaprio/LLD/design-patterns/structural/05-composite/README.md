# Composite Pattern

> Composes objects into **tree structures** to represent part-whole hierarchies. Clients treat individual objects and compositions uniformly.

---

## When to Use

- Tree structure of objects (file system, UI components, org chart)
- Client should ignore difference between leaf and composite
- "Part-whole" hierarchy

---

## Structure

```
Component (interface)
    + operation()
    |
    +-- Leaf (implements operation)
    |
    +-- Composite
        + children: List<Component>
        + add(), remove(), getChild()
        + operation() → delegates to children
```

---

## Example: File System

```java
interface FileSystemComponent {
    void show(int indent);
}

class File implements FileSystemComponent {
    private String name;
    public File(String name) { this.name = name; }
    public void show(int indent) {
        System.out.println("  ".repeat(indent) + "File: " + name);
    }
}

class Directory implements FileSystemComponent {
    private String name;
    private List<FileSystemComponent> children = new ArrayList<>();

    public Directory(String name) { this.name = name; }
    public void add(FileSystemComponent c) { children.add(c); }
    public void remove(FileSystemComponent c) { children.remove(c); }

    public void show(int indent) {
        System.out.println("  ".repeat(indent) + "Dir: " + name);
        for (FileSystemComponent child : children) {
            child.show(indent + 1);
        }
    }
}

// Usage
Directory root = new Directory("root");
root.add(new File("a.txt"));
Directory sub = new Directory("sub");
sub.add(new File("b.txt"));
root.add(sub);
root.show(0);
```

---

## Common Interview Q&A

**Q: Composite vs Tree structure?**  
A: Composite is a design pattern for tree structures where leaves and composites share same interface. Not every tree uses Composite.

**Q: How to add operations only to leaves?**  
A: Use default impl in Component that does nothing; override in Leaf. Or use Visitor pattern for type-specific ops.

**Q: Real-world examples?**  
A: DOM (Element, Document), JUnit (TestSuite contains Tests), GUI (Panel contains Buttons/Panels).
