---
status: stable
last-reviewed: 2026-07-03
owners: [adam]
version: 0.1
---

# Civic Activity Specification v0.1 (Hybrid Model)

> **Naming note.** This specification was previously titled the "Civic Event Specification." It has been renamed to the **Civic Activity Specification** to align with the vocabulary of ActivityStreams 2.0 and ActivityPub, which the ecosystem bridges to in a later phase, and to match the Civic Activity Feed layer that consumes these objects. The protocol-level term is **Civic Activity**. For v0.1, the wire-format field name remains `event_type` and the transport endpoint remains `GET /events` — see §14 for the ratified compatibility policy and the v0.2 rename plan.

## Purpose

Define a minimal, interoperable activity model for the Civic.Social ecosystem.

Civic Activities are the **distribution layer** of the system. They communicate what is happening across processes, spaces, and interfaces — one shared stream, refracted into many lenses (inbox, notifications, discovery, space views, embeds) by the Civic Activity Feed.

This spec defines:
- Base activity schema
- Activity types and the extension namespace convention
- Process integration
- Visibility and disclosure rules
- Mapping to ActivityStreams (forward compatibility)

---

## 1. Design Principles

- Activity-first architecture — no silent state changes; the activity log is the source of truth
- Simple JSON for v0.1
- Deterministic structure (no ambiguity)
- Compatible with ActivityStreams (upgrade path)
- No hidden logic or implicit fields

---

## 2. Base Activity Schema

All activities MUST conform to the following structure:

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
    "hub_url": "https://hub.example",
    "space_id": "did:web:hub.example"
  },
  "dedupe_key": "optional-string",
  "data": {},
  "meta": {
    "visibility": "public"
  }
}
```

Notes on `source`:

- `source.hub_id` and `source.hub_url` are the **v0.1 serialization of the emitting space's identity** and are retained for backward compatibility with existing implementations. They identify the Civic Space (of any scope) that emitted the activity.
- `source.space_id` is the emitting space's **stable DID** (see the Civic Space Specification). It is OPTIONAL in v0.1 and will become REQUIRED in v0.2. Consumers SHOULD key provenance on `space_id` where present, because it survives migration of the space to a new URL or engine; `hub_url` identifies only the *current* serving location.

---

## 3. Required Fields

| Field | Required | Description |
|------|----------|------------|
| `id` | yes | Unique activity identifier |
| `version` | yes | Schema version of this activity object (`"1.0"` for this spec) |
| `event_type` | yes | Canonical activity type (see §14 for the field-name compatibility policy) |
| `timestamp` | yes | Activity creation time (ISO 8601) |
| `process_id` | yes for process activities | Associated process. Non-process activities (e.g., space lifecycle activities, §4.4) omit it |
| `actor` | yes | Who performed the action: a DID, an opaque user identifier, or a system identifier (e.g. `system:auto-close`) |
| `jurisdiction` | yes | Geographic or community scope |
| `action_url` | yes | Link to view or take action |
| `source` | yes | Emitting space attribution (`hub_id`, `hub_url`; `space_id` recommended) |
| `data` | yes | Activity-specific payload (may be `{}`) |
| `meta.visibility` | yes | Visibility class (§7) |
| `dedupe_key` | optional | Idempotency hint for consumers; deduplication semantics are out of scope in v0.1 (§12) |

This table is the single authoritative required-fields list for v0.1. A compliant activity includes every required field above; a compliant emitter never omits `version`, `source`, or `meta.visibility`.

---

## 4. Activity Types (v0.1)

> The canonical type identifiers are noun-based (`civic.process.created`) for v0.1. A future revision may introduce verb-based aliases (`Create`, `Update`, `Announce`, etc.) to align more closely with the ActivityStreams 2.0 vocabulary as part of the AS2 bridge work (see §13).

### 4.1 Lifecycle Activities

- `civic.process.created`
- `civic.process.updated`
- `civic.process.started`
- `civic.process.ended` — emitted when a process reaches a terminal lifecycle state (`closed` or its profile's equivalent); see the Civic Process Specification's phase→activity mapping
- `civic.process.result_published`

### 4.2 Participation Activities

- `civic.process.action_taken`
- `civic.process.vote_submitted`
- `civic.process.comment_added`
- `civic.process.proposal_created`

### 4.3 Full-Lifecycle Activities

Defined by the Civic Process Specification for process types that implement the full lifecycle model:

- `civic.process.framed`
- `civic.process.aggregation_completed`
- `civic.process.outcome_recorded`
- `civic.process.outcome_delivered` — an outcome formally delivered to a recipient (e.g., a Representative Space). *Implementation note:* the reference implementation currently emits this as `civic.outcome_delivered`; that spelling is ratified as a deprecated v0.1 alias and will migrate to the canonical namespaced form.
- `civic.process.feedback_received`

### 4.4 Space Lifecycle Activities

- `civic.space.migrated` — the final activity a space emits from its old location upon migration, carrying the new binding (see the Civic Space Specification's portability contract and the Discovery Layer's re-binding protocol)

### 4.5 Extension Namespace Convention

Additional activity types MAY be defined by process plugins and space types using the convention:

```
civic.<domain>.<verb>
```

Extension rules:

- Extension types MUST NOT redefine the semantics of the canonical types above.
- Every `civic.process.*` activity MUST carry the process-type discriminator `data.process.type` (e.g. `"data": { "process": { "type": "civic.wordcloud" } }`) so consumers can classify activities without reverse-engineering payload shapes.
- Plugins MUST declare the activity types they emit in their plugin manifest (see the Civic Plugin Architecture), and hosts SHOULD reject emissions of undeclared types.

---

## 5. Activity Data Payloads

Each activity may include a `data` object.

### Data Structure Rule

The `data` field MUST be namespaced by the activity type to avoid collisions and ensure consistency.

Example:

```json
{
  "event_type": "civic.process.vote_submitted",
  "data": {
    "process": { "type": "civic.vote" },
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
      "type": "civic.vote",
      "title": "Library Expansion Vote"
    }
  }
}
```

---

## 6. Action → Activity Relationship

Every process action MUST emit at least one activity.

Example:

| Action | Activity |
|--------|------|
| submit_vote | civic.process.vote_submitted |
| submit_comment | civic.process.comment_added |

Lifecycle transitions MUST also emit activities. There are no silent state changes: any state a consumer can observe via a read endpoint must be reconstructible from the activity history.

---

## 7. Visibility & Disclosure Model (v0.1)

Defined in `meta.visibility`:

- `public`
- `restricted`

Future versions may include credential-scoped visibility.

### Disclosure follows process configuration

Visibility classifies *who may see the activity*. **Disclosure** governs *what the payload may contain*, and is configured per process via the process descriptor's identity/disclosure policy (see the Civic Process Specification and the Identity Policy Object in the Civic Identity Specification). The binding rule:

- An activity's payload MUST NOT disclose participant data that the emitting process's disclosure policy protects. For example, a vote process configured for **secret ballots** MUST NOT include the participant's selected option in any activity visible beyond what the policy allows — participation may be public (`vote_submitted` with actor) while the ballot content is withheld or carried only in `restricted` activities.
- Processes configured for **on-the-record** participation (e.g., roll-call-style votes, public endorsements) MAY include the participant's choice in public activity payloads, and MUST say so in their published descriptor so participants know before acting.

---

## 8. Activity Transport (v0.1)

Activities can be delivered via:

- HTTP APIs
- Webhooks
- Feed endpoints

The transport endpoint is `GET /events` in v0.1 (see §14); a `GET /activities` alias may be introduced in a later revision. Feed responses are ordered by descending timestamp.

ActivityPub support is optional in v0.1 and is the target of the Phase 3 federation bridge.

---

## 9. ActivityStreams Mapping (Forward Compatibility)

Each Civic Activity can be mapped to an ActivityStreams 2.0 activity:

```json
{
  "type": "Create",
  "actor": "did:example:123",
  "object": {
    "type": "CivicActivity",
    "id": "activity-id",
    "event_type": "civic.process.started"
  },
  "published": "timestamp"
}
```

Mapping Rules:

- `event_type` → object type discriminator (and, in the AS2 bridge, may also drive the outer `type` verb)
- `actor` → actor
- `timestamp` → published
- `process_id` → object.context (future extension)
- `source.space_id` / `source.hub_url` → attributedTo (future mapping)

A complete AS2 bridge specification — including verb mapping, collection semantics, and the published JSON-LD context document at `https://civicsocial.org/ns/civic` (a prerequisite for both the bridge and the JSON-LD export format in the Civic Space Specification) — is deferred to the Phase 3 federation work.

---

## 10. Space Responsibilities

Every Civic Space (of any scope — hub, dashboard, representative space) must:

- Emit activities for all process activity through a **single emission path** (one chokepoint through which every activity flows)
- Validate the activity envelope at that emission path (required fields present, `process_id` populated for process activities, declared types only)
- Provide an activity feed endpoint
- Ensure activities conform to this schema

---

## 11. Minimal Compliance (v0.1)

To be compliant, a system must:

- Emit valid activities
- Include the `version` field
- Include source attribution (`source.hub_id`, `source.hub_url`; `source.space_id` recommended)
- Include `meta.visibility`
- Use defined activity types (canonical or manifest-declared extensions per §4.5)
- Carry `data.process.type` on every `civic.process.*` activity
- Follow data namespacing rules
- Include all required fields (§3)
- Support at least one transport method

---

## 12. Out of Scope (v0.1)

- Activity signing (planned; see §13 — required before migrated/federated history can be independently verified)
- Federation guarantees
- Ranking algorithms
- Deduplication semantics (`dedupe_key` is carried but uninterpreted)

---

## 13. Future Extensions (v0.2+)

- Wire-format field rename `event_type` → `activity_type` and the `GET /activities` endpoint alias (see §14)
- `source.space_id` becomes required
- ActivityPub native support (Phase 3 bridge) and ActivityStreams 2.0 verb-based type aliases
- Activity signatures (signed by the emitting space's DID)
- Credential-scoped visibility
- Cross-space activity propagation
- Activity subscriptions and filtering

---

## 14. Implementation Note — Code Terms and Wire Terms

The reference implementation uses the term **event** throughout its code (file names, function names, type names, the `/events` endpoint, the event store, the `emitEvent()` function). This is intentional: "event" is the long-standing term in event-sourced architectures, and renaming it across a codebase offers little engineering benefit.

The protocol-level term is **Civic Activity**. The ratified v0.1 wire format:

- The wire-format type field is **`event_type`** and the transport endpoint is **`GET /events`**. This matches all shipping implementations and is the compatibility baseline external consumers may rely on for the life of v0.1.
- The planned v0.2 revision renames the wire field to `activity_type` and introduces `GET /activities`, coordinated with the AS2 bridge work. v0.2 emitters will dual-emit or alias for a documented deprecation window.

### Conformance phasing

The reference implementation (`civic-hub`) currently implements this specification's required envelope, single-emission-path, and feed-endpoint requirements; envelope validation at emission, `source.space_id`, and activity signing are targeted through the Civic Hubs and Civic Activity Feed & Discovery pilots. Specifications in this ecosystem define the target state; reference implementations converge on them through the pilot program.

---

## Summary

Civic Activities provide the backbone of distribution in the Civic.Social ecosystem.

They are:
- Simple
- Structured
- Interoperable
- Forward-compatible with ActivityPub

This spec enables rapid development while preserving a clear path to federation and interoperability.
