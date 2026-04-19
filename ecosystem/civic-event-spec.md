---
status: stable
last-reviewed: 2026-04-19
owners: [adam]
version: 0.1
---

# Civic Event Specification v0.1 (Hybrid Model)

## Purpose

Define a minimal, interoperable event model for the Civic.Social ecosystem.

Civic Events are the **distribution layer** of the system. They communicate what is happening across processes, hubs, and interfaces.

This spec defines:
- Base event schema
- Event types
- Process integration
- Mapping to ActivityStreams (forward compatibility)

---

## 1. Design Principles

- Event-first architecture
- Simple JSON for v0.1
- Deterministic structure (no ambiguity)
- Compatible with ActivityStreams (upgrade path)
- No hidden logic or implicit fields

---

## 2. Base Event Schema

All events MUST conform to the following structure:

```json
{
  "id": "uuid",
  "version": "1.0",
  "event_type": "civic.process.started",
  "timestamp": "ISO8601",
  "process_id": "uuid",
  "actor": "did:example:123",
  "jurisdiction": "us-va-floyd",
  "action_url": "https://example.org/process/123",
  "source": {
    "hub_id": "hub-123",
    "hub_url": "https://hub.example"
  },
  "dedupe_key": "optional-string",
  "data": {},
  "meta": {
    "visibility": "public"
  }
}
```json
{
  "id": "uuid",
  "event_type": "civic.process.started",
  "timestamp": "ISO8601",
  "process_id": "uuid",
  "actor": "did:example:123",
  "jurisdiction": "us-va-floyd",
  "action_url": "https://example.org/process/123",
  "data": {},
  "meta": {
    "visibility": "public"
  }
}
```

---

## 3. Required Fields

| Field | Description |
|------|------------|
| id | Unique event identifier |
| event_type | Canonical event type |
| timestamp | Event creation time |
| process_id | Associated process |
| actor | DID or system (optional) |
| jurisdiction | Geographic scope |
| action_url | Link to take action |
| data | Event-specific payload |

---

## 4. Event Types (v0.1)

### 4.1 Lifecycle Events

- `civic.process.created`
- `civic.process.updated`
- `civic.process.started`
- `civic.process.ended`
- `civic.process.result_published`

### 4.2 Participation Events

- `civic.process.action_taken`
- `civic.process.vote_submitted`
- `civic.process.comment_added`
- `civic.process.proposal_created`

---

## 5. Event Data Payloads

Each event may include a `data` object.

### Data Structure Rule

The `data` field MUST be namespaced by event_type to avoid collisions and ensure consistency.

Example:

```json
{
  "event_type": "civic.process.vote_submitted",
  "data": {
    "vote": {
      "option_id": "option-1"
    }
  }
}
```

### Example: process_started

```json
{
  "event_type": "civic.process.started",
  "data": {
    "process": {
      "title": "Library Expansion Vote"
    }
  }
}
```

---

## 6. Action → Event Relationship

Every process action MUST emit at least one event.

Example:

| Action | Event |
|--------|------|
| submit_vote | civic.process.vote_submitted |
| submit_comment | civic.process.comment_added |

Lifecycle transitions MUST also emit events.

---

## 7. Visibility Model (v0.1)

Defined in `meta.visibility`:

- public
- restricted

Future versions may include credential-scoped visibility.

---

## 8. Event Transport (v0.1)

Events can be delivered via:

- HTTP APIs
- Webhooks
- Feed endpoints

ActivityPub support is optional in v0.1.

---

## 9. ActivityStreams Mapping (Forward Compatibility)

Each Civic Event can be mapped to ActivityStreams:

```json
{
  "type": "Create",
  "actor": "did:example:123",
  "object": {
    "type": "CivicEvent",
    "id": "event-id",
    "event_type": "civic.process.started"
  },
  "published": "timestamp"
}
```

Mapping Rules:

- `event_type` → object.event_type
- `actor` → actor
- `timestamp` → published
- `process_id` → object.context (future extension)
- `source.hub_url` → attributedTo (future mapping)

---

## 10. Hub Responsibilities

Civic Hubs must:

- Emit events for all process activity
- Provide an event feed endpoint
- Ensure events conform to schema

---

## 11. Minimal Compliance (v0.1)

To be compliant, a system must:

- Emit valid events
- Include version field
- Include source attribution
- Use defined event types
- Follow data namespacing rules
- Include required fields
- Support at least one transport method

---

## 12. Out of Scope (v0.1)

- Event signing
- Federation guarantees
- Ranking algorithms
- Deduplication

---

## 13. Future Extensions (v0.2+)

- ActivityPub native support
- Event signatures
- Credential-scoped visibility
- Cross-hub event propagation
- Event subscriptions and filtering

---

## Summary

Civic Events provide the backbone of distribution in the Civic.Social ecosystem.

They are:
- Simple
- Structured
- Interoperable
- Forward-compatible with ActivityPub

This spec enables rapid development while preserving a clear path to federation and interoperability.
