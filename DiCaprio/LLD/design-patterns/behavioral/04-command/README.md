# Command Pattern

> Encapsulates a **request as an object**, letting you parameterize clients with different requests, queue or log requests, and support undoable operations.

---

## When to Use

- Parameterize operations (callback-style)
- Queue requests (job queue, thread pool)
- Undo/redo
- Log requests for replay/audit

---

## Structure

```
Invoker          Command (interface)
  - command  -->   + execute()
  + invoke()       + undo() [optional]
       |
       +-- calls
            |
       ConcreteCommand
         - receiver: Receiver
         + execute() → receiver.action()
```

---

## Example: Undo/Redo in Editor

```java
interface Command {
    void execute();
    void undo();
}

class Editor {
    private StringBuilder text = new StringBuilder();
    public void insert(String s) { text.append(s); }
    public void delete(int n) { text.setLength(Math.max(0, text.length() - n)); }
    public String getText() { return text.toString(); }
}

class InsertCommand implements Command {
    private Editor editor;
    private String text;
    public InsertCommand(Editor editor, String text) {
        this.editor = editor;
        this.text = text;
    }
    public void execute() { editor.insert(text); }
    public void undo() { editor.delete(text.length()); }
}

class CommandHistory {
    private Stack<Command> history = new Stack<>();

    public void execute(Command cmd) {
        cmd.execute();
        history.push(cmd);
    }
    public void undo() {
        if (!history.isEmpty()) {
            history.pop().undo();
        }
    }
}

// Usage
Editor editor = new Editor();
CommandHistory history = new CommandHistory();
history.execute(new InsertCommand(editor, "Hello "));
history.execute(new InsertCommand(editor, "World"));
history.undo();  // Removes "World"
```

---

## Common Interview Q&A

**Q: Command vs Strategy?**  
A: Command = request + receiver + optional undo. Strategy = algorithm only. Command is "do this"; Strategy is "how to do it".

**Q: Command for async?**  
A: Yes — Commands can be queued, executed by worker threads. Used in message queues, event sourcing.

**Q: Real-world examples?**  
A: Runnable, Lambda (command-like), Undo in editors, Job queues (each job = command).
