# Facade Pattern

> Provides a **simplified interface** to a complex subsystem. Doesn't prevent access to subsystem; just offers a convenient way.

---

## When to Use

- Simplify complex library/API
- Decouple client from subsystem components
- Layer your system (facade as entry point)

---

## Example: Home Theater

```java
// Complex subsystem
class DVDPlayer { void on() {} void play(String movie) {} void stop() {} void off() {} }
class Amplifier { void on() {} void setVolume(int v) {} void off() {} }
class Lights { void dim(int level) {} }
class Projector { void on() {} void wideScreenMode() {} void off() {} }

// Facade - simple interface
class HomeTheaterFacade {
    private DVDPlayer dvd;
    private Amplifier amp;
    private Lights lights;
    private Projector projector;

    public HomeTheaterFacade(DVDPlayer dvd, Amplifier amp, Lights lights, Projector projector) {
        this.dvd = dvd;
        this.amp = amp;
        this.lights = lights;
        this.projector = projector;
    }

    public void watchMovie(String movie) {
        lights.dim(10);
        projector.on();
        projector.wideScreenMode();
        amp.on();
        amp.setVolume(5);
        dvd.on();
        dvd.play(movie);
    }

    public void endMovie() {
        dvd.stop();
        dvd.off();
        amp.off();
        projector.off();
        lights.dim(0);
    }
}

// Client - one call instead of 6+
HomeTheaterFacade theater = new HomeTheaterFacade(dvd, amp, lights, projector);
theater.watchMovie("Inception");
```

---

## Facade vs Adapter

| Facade | Adapter |
|--------|---------|
| Simplifies interface | Changes interface |
| Many classes → one | One class → different interface |
| Subsystem stays same | Adapts to client expectation |

---

## Common Interview Q&A

**Q: Real-world examples?**  
A: JDBC (hides Connection, Statement, ResultSet complexity), Spring's JdbcTemplate, REST client libraries.

**Q: Does Facade add functionality?**  
A: No — it orchestrates existing functionality. It's a "simplification" layer.
