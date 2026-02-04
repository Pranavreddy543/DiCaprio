# Distributed Transactions

> Transactions spanning multiple services or databases.

---

## The Problem

- ACID in single DB is straightforward
- Across services: each has own DB; no single coordinator
- Need: All commit or all abort

---

## Two-Phase Commit (2PC)

**Phase 1 (Prepare):**
- Coordinator asks all participants: "Can you commit?"
- Participants prepare (lock resources, write to log)
- Reply Yes/No

**Phase 2 (Commit/Abort):**
- If all Yes: Coordinator says "Commit"; all commit
- If any No: Coordinator says "Abort"; all rollback

**Pros:** Strong consistency  
**Cons:** Blocking (coordinator failure = participants stuck), latency, single point of failure

---

## Saga Pattern

- **Choreography:** Each service emits event; next service reacts
- **Orchestration:** Central orchestrator calls services in sequence

**Compensation:** If step N fails, run compensating transactions for steps 1..N-1

**Example:** Order → Reserve inventory → Charge payment. If charge fails → release inventory, cancel order.

**Pros:** No locking, scales  
**Cons:** Eventual consistency; compensation logic complex

---

## When to Use

| 2PC | Saga |
|-----|------|
| Strong consistency required | Eventual OK |
| Few participants | Many services |
| Short transactions | Long-running |
| Banking, inventory | E-commerce order flow |

---

## Common Interview Q&A

**Q: Why is 2PC blocking?**  
A: If coordinator crashes after Prepare, participants hold locks and wait. Cannot know if commit or abort.

**Q: Saga — what if compensation fails?**  
A: Retry; dead letter queue; manual intervention. Design compensations to be idempotent.
