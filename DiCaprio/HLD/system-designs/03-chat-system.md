# Design Chat System (WhatsApp, Slack)

> Real-time messaging between users.

---

## Requirements

**Functional**
- 1:1 and group chat
- Real-time delivery
- Message history
- Online status (presence)
- Read receipts (optional)

**Non-Functional**
- Low latency (< 100ms)
- High availability
- Scale: 50M DAU, 10B messages/day

---

## High-Level Design

```
Clients (Mobile/Web)
    ↕ WebSocket (persistent connection)
API Gateway / WebSocket Servers
    ↕
Message Service  ←→  Message Queue (Kafka)
    ↕
Database (messages)    Cache (online status)
```

---

## Core Components

### 1. Real-Time Connection: WebSocket

- **HTTP:** Client polls → latency, wasteful
- **WebSocket:** Persistent bidirectional connection
- **Fallback:** Long polling for older clients

### 2. Message Flow

**Send:**
```
Client A → WebSocket Server → Message Service
    → Write to DB
    → Publish to Kafka (topic: recipient_id or conversation_id)
```

**Receive:**
```
Kafka Consumer → Find WebSocket connection for recipient
    → Push message over WebSocket to Client B
```

### 3. Storing Messages

**Schema:**
```
messages:
  id, conversation_id, sender_id, content, timestamp
conversations:
  id, type (1:1 or group), participant_ids
```

**Sharding:** By `conversation_id` or `user_id` (for inbox).

### 4. Presence (Online/Offline)

- **Heartbeat:** Client sends ping every 30s
- **Store:** Redis `user_id → last_seen` with TTL
- **On connect:** Publish "user online" to contacts
- **On disconnect:** Publish "user offline"

### 5. Message Ordering

- Per-conversation ordering: Single Kafka partition per conversation
- Sequence numbers or timestamps
- Client handles out-of-order (sort by seq)

---

## Scale Considerations

- **WebSocket servers:** Stateless; connection map in Redis (server_id → user_id)
- **Message queue:** Kafka; partition by conversation for ordering
- **DB:** Shard by conversation_id; separate table for recent (hot) vs archive
- **Media:** Object storage (S3); send URL in message

---

## Interview Talking Points

- WebSocket for real-time; fallback to long polling
- Message queue for fan-out to multiple devices
- Presence via Redis + heartbeat
- Ordering: Kafka partition per conversation
