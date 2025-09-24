# Phase 0: Outline & Research — Real-time chat (1:1, presence, history)

## Decisions
- Realtime transport: WebSocket for bidirectional messaging and presence push (<=1s).
- History store: PostgreSQL for durable message storage (30-day rolling retention).
- Presence state: Redis for online/offline fanout and transient state.
- Limits: 2KB/message max; 10 messages per 5 seconds per user.
- Scope: 1:1 conversations only in MVP.

## Rationale
- WebSocket offers low-latency, server-mediated ordering and delivery acknowledgments.
- PostgreSQL provides reliable persistence, indexing, and ordering guarantees.
- Redis enables efficient presence distribution and ephemeral state management.
- Enforced limits protect system and UX; reduce abuse and keep performance predictable.

## Alternatives Considered
- Server-Sent Events (SSE): simpler but unidirectional; no client-to-server messaging.
- Long polling: higher latency and overhead; inferior UX.
- NoSQL for messages: flexible but unnecessary; relational queries and ordering fit PostgreSQL well.
- Kafka for presence: overkill for MVP; Redis simpler and sufficient.

## Open Questions (NEEDS CLARIFICATION)
- Auth & roles model (admins/moderators?).
- Conversation list sort rules; pin/mute behavior.
- Message edits/deletes policy and visibility.
- Search, attachments, reactions, and notifications scope.
- Tech stack specifics for server language and test framework.
- Scale targets (concurrent users, conversations per user) beyond baseline.

## Performance & Reliability Notes
- Backend budgets: p95 <200ms, p99 <500ms on API endpoints; presence visible <=1s.
- Avoid N+1: use indexed queries and pagination for history.
- Reconnect handling: client resync from last acknowledged message id to prevent duplicates.
- Observability: structured logging for send/receive, rate-limit hits, and errors; basic metrics (send rate, presence updates).

## Security Considerations
- AuthN/Z: only participants access conversation content and presence.
- Input validation: enforce size limits; sanitize content; prevent injection.
- Rate limiting: per-user limits; backoff on abuse.
