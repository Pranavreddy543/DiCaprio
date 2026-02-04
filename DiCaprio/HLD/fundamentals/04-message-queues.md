# Message Queues

> Asynchronous communication between services. Decouples producers and consumers.

---

## When to Use

- Async processing (emails, notifications)
- Decoupling services
- Load leveling (smooth spikes)
- Event-driven architecture

---

## Key Concepts

| Concept | Description |
|---------|-------------|
| **Producer** | Publishes messages |
| **Consumer** | Processes messages |
| **Broker** | Queue (Kafka, RabbitMQ, SQS) |
| **At-least-once** | May duplicate; consumer must be idempotent |
| **At-most-once** | May lose; no retry |
| **Exactly-once** | Hard; requires transactional outbox or dedup |

---

## Queue vs Pub-Sub

| Queue (Point-to-point) | Pub-Sub (Topic) |
|------------------------|-----------------|
| One consumer per message | Multiple subscribers get same message |
| Competing consumers | Fan-out |
| RabbitMQ queues | Kafka topics, SNS |

---

## Popular Brokers

| Broker | Model | Use Case |
|--------|-------|----------|
| **Kafka** | Log, partition, consumer groups | High throughput, event sourcing, stream processing |
| **RabbitMQ** | Queues, exchanges | Task queues, complex routing |
| **SQS** | Simple queues | AWS, decoupling, simple |
| **Redis Streams** | Log-like | Lightweight, real-time |

---

## Common Interview Q&A

**Q: Message ordering?**  
A: Kafka: per-partition ordering. RabbitMQ: single queue. For global order: single partition (limits throughput).

**Q: Message loss?**  
A: Producer: ack from broker. Broker: replication. Consumer: process then ack. At-least-once + idempotent consumer.

**Q: Dead letter queue (DLQ)?**  
A: Failed messages go to DLQ for retry or manual inspection. Prevents blocking main queue.
