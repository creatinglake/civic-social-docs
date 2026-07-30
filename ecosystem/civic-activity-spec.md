---
status: review
last-reviewed: 2026-07-26
owners: [adam]
version: 0.1
---

# Civic Activity Specification v0.1 (Hybrid Model)

> **Naming note.** The protocol-level term is **Civic Activity**, aligned with the vocabulary of ActivityStreams 2.0 and ActivityPub, which the ecosystem bridges to in a later phase, and with the Civic Activity Feed layer that consumes these objects. For v0.1, the wire-format field name is `event_type` and the transport endpoint is `GET /events` — see §14 for the compatibility policy and the v0.2 wire-rename plan.

## Purpose

Define a minimal, interoperable activity model for the Civic.Social ecosystem.

Civic Activities are the **distribution layer** of the system. They communicate what is happening across processes, spaces, and interfaces — one shared stream, refracted into many lenses (inbox, notifications, discovery, space views, embeds) by the Civic Activity Feed.

This spec defines:
- Base activity schema
- Activity types and the extension namespace convention
- Process integration
- Visibility and disclosure rules
- Mapping to ActivityStreams (forward compatibility)

> **Which horizon is this?** This document specifies what can be built and verified **today** — the activity envelope two independent implementations must agree on. Everything beyond it, including the ActivityPub bridge, activity signing, and cross-space propagation, is deferred by design; **[Civic.Social Horizons](../canon/phasing.md)** maps each deferral to the pilot that closes it and to the destination it is headed toward.

---

## Notation and Conformance Language

**Notation.** The key words MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, SHOULD NOT, RECOMMENDED, MAY, and OPTIONAL in this document are to be interpreted as described in RFC 2119 and RFC 8174 when, and only when, they appear in all capitals. The same words in lower case carry their ordinary English meaning and impose no requirement.

---

## 1. Design Principles

- Activity-first architecture — no silent state changes; the activity log is the source of truth
- Simple JSON for v0.1
- Deterministic structure (no ambiguity)
- Compatible with ActivityStreams (upgrade path)
- No hidden logic or implicit fields

---

## 2. Base Activity Schema

All activities MUST conform to the following structure. The block below is an **illustrative envelope**: it shows the shape and the field names, and its values are placeholders describing the expected type (`"uuid"`, `"ISO8601"`), not literal values. §3 is the normative definition of which fields are required and what their values must look like; §2.1 shows a complete activity with real values.

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

### 2.1 Complete Example

The activity below is complete and valid: every field the §3 table marks required is present, with realistic values rather than type placeholders. Implementers can copy it directly and use it as a test fixture for envelope validation.

```json
{
  "id": "018f3a2c-7b41-7c3e-9d21-4a6f8b2e1c05",
  "version": "1.0",
  "event_type": "civic.process.vote_submitted",
  "timestamp": "2026-07-14T15:04:05-04:00",
  "process_id": "0191c4d8-3e52-7a19-8b60-2c7d9f1a4e33",
  "actor": "did:web:id.civic.example:u:9f2c1b7a",
  "jurisdiction": "us-va-floyd",
  "action_url": "https://hub.floyd.example/process/0191c4d8-3e52-7a19-8b60-2c7d9f1a4e33",
  "source": {
    "hub_id": "hub-floyd",
    "hub_url": "https://hub.floyd.example",
    "space_id": "did:web:hub.floyd.example"
  },
  "dedupe_key": "vote:0191c4d8-3e52-7a19-8b60-2c7d9f1a4e33:9f2c1b7a",
  "data": {
    "process": { "type": "civic.vote" },
    "vote": {
      "method": "yes_no",
      "recorded_at": "2026-07-14T15:04:05-04:00"
    }
  },
  "meta": {
    "visibility": "public"
  }
}
```

Two things in this example are worth reading closely. First, `data` carries exactly two keys — the mandatory `process` discriminator and one noun naming the subject of the activity (`vote`); that is the namespacing rule of §5. Second, `data.vote` records *that* a ballot was cast and by what method, but not *what* was chosen: this process is configured for secret ballots, so the participant's selection is withheld from a `public` activity under the disclosure rule in §7. A process configured for on-the-record voting could include the selection here.

---

## 3. Required Fields

| Field | Required | Description |
|------|----------|------------|
| `id` | yes | Unique activity identifier |
| `version` | yes | Schema version of this activity object (`"1.0"` for this spec) |
| `event_type` | yes | Canonical activity type (see §14 for the field-name compatibility policy) |
| `timestamp` | yes | Activity creation time — RFC 3339 date-time with explicit offset (§3.1) |
| `process_id` | yes for process activities | Associated process. Non-process activities (e.g., space lifecycle activities, §4.4) omit it |
| `actor` | yes | Who performed the action: a DID, an opaque user identifier, or a system identifier (e.g. `system:auto-close`) |
| `jurisdiction` | yes | Geographic or community scope |
| `action_url` | yes | Link to view or take action |
| `source` | yes | Emitting space attribution (`hub_id`, `hub_url`; `space_id` recommended) |
| `data` | yes | Activity-specific payload (MAY be `{}`) |
| `meta.visibility` | yes | Visibility class (§7) |
| `dedupe_key` | optional | Idempotency hint: a stable string a consumer MAY use to suppress duplicate deliveries of the same activity (§3.1) |

This table is the single authoritative required-fields list for v0.1. A compliant activity includes every required field above; a compliant emitter never omits `version`, `source`, or `meta.visibility`.

### 3.1 Field Formats

The table says which fields are required; this section says what a valid value looks like. Two implementations that agree on the field list but disagree on these formats will not interoperate, so they are normative.

- **`id`** — a UUID (any version; UUIDv7 is RECOMMENDED because its time-ordering makes activity stores cheap to index) or an absolute URI. The identifier MUST be globally unique, not merely unique within the emitting space: an activity keeps the same `id` wherever it is relayed or re-served, so a consumer MAY use it to deduplicate the same activity received from more than one space.
- **`timestamp`** — an RFC 3339 date-time with an explicit timezone offset, e.g. `2026-07-14T15:04:05-04:00` or `2026-07-14T19:04:05Z`. A timestamp without an offset MUST NOT be emitted; a consumer cannot order a mixed-jurisdiction feed correctly without one.
- **`actor`** — the identity that performed the action, in one of three forms, distinguished by prefix so a consumer can tell them apart without out-of-band knowledge:
  - a **DID**, recognized by the `did:` prefix (e.g. `did:web:id.civic.example:u:9f2c1b7a`). Where the actor has a DID, the emitter MUST use it — *except* where the process's declared `disclosure_policy` is `anonymous` or `pseudonymous`, in which case the emitter SHOULD substitute a process-scoped opaque identifier and MUST NOT publish the mapping back to the DID (Civic Process Specification §2.3, §7.3). A DID in `actor` is attributable and correlatable by design: that is what makes verified participation and portable history possible, and it is why a process that needs participation without attribution has to say so in its disclosure policy rather than relying on visibility.
  - a **system identifier**, recognized by the `system:` prefix (e.g. `system:auto-close`), for transitions performed by the space itself rather than a person.
  - an **opaque identifier** — anything else. An opaque identifier is scoped to the emitting space (identified by `source.space_id`, falling back to `source.hub_url`) and MUST NOT be assumed to denote the same participant across spaces. This is the form a space using a stub identity adapter emits (Civic Space Specification §7.3).
- **`jurisdiction`** — a lowercase, hyphen-separated, hierarchical string ordered broadest to narrowest, e.g. `us-va-floyd` (country, state/region, locality). Deeper or shallower values are permitted (`us`, `us-va`, `us-va-floyd-ward3`) so long as the hierarchy reads left to right. Where an actor has no meaningful jurisdiction — an individual dashboard, a global organization — the emitter MUST use the literal string `"none"` rather than omitting the field or sending an empty string, so that consumers filtering by jurisdiction see an explicit answer rather than a gap.
- **`dedupe_key`** — an OPTIONAL idempotency hint: a stable string a consumer MAY use to suppress duplicate deliveries of the same activity. A consumer SHOULD treat two activities carrying the same `dedupe_key` from the same `source.space_id` as the same activity; the key is scoped to the emitting space and carries no meaning across spaces. It is a hint, not a guarantee, so an emitter MUST NOT rely on consumers deduplicating, and MUST NOT use `dedupe_key` as its only defense against emitting the same activity twice.

---

## 4. Activity Types (v0.1)

> The canonical type identifiers are noun-based (`civic.process.created`) for v0.1. A future revision may introduce verb-based aliases (`Create`, `Update`, `Announce`, etc.) to align more closely with the ActivityStreams 2.0 vocabulary as part of the AS2 bridge work (see §13).

**How to read this registry.** The registry is tiered by how much lifecycle a process opts into. Section 4.1 is the base vocabulary every process emits regardless of lifecycle profile; 4.2 is what participants do inside one. Section 4.3 applies **only** to process types that implement the full deliberative lifecycle — simpler processes never emit those types. Section 4.4 is the vocabulary of spaces themselves. Section 4.5 is the extension convention, and it is why this registry does not have to anticipate everything: the canonical types pin the shared grammar that lets strangers interoperate, while new vocabulary can always be minted without changing this specification.

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

> **Conditionally required; payloads provisional.** Whether a process must emit one of these types depends on whether its declared lifecycle profile actually runs the phase the type reports. The per-profile obligations are tabulated in **Civic Process Specification §14.1, which is authoritative**; this section defines the identifiers and payloads, not who owes them. The distinction is which processes must emit them, not whether they are ratified.
>
> The **type identifiers** in this section are stable: implementers can hardcode them and match on them. The **payload shapes** are provisional — this is the least field-tested part of the registry, and no external implementation has exercised it yet, so `data` contents may be refined in v0.2. Build against the identifiers with confidence; treat unfamiliar keys inside `data` as forward-compatible additions rather than errors.

Defined by the Civic Process Specification for process types that implement the full lifecycle model:

- `civic.process.framed`
- `civic.process.aggregation_completed`
- `civic.process.outcome_recorded`
- `civic.process.outcome_delivered` — an outcome formally delivered to a recipient (e.g., a Representative Space). *Implementation note:* the reference implementation currently emits this as `civic.outcome_delivered`; that spelling is ratified as a deprecated v0.1 alias and will migrate to the canonical namespaced form.
- `civic.process.feedback_received`

### 4.4 Space Lifecycle Activities

- `civic.space.migrated` — the final activity a space emits from its old location upon migration, carrying the new binding so feeds and indexers can re-bind to the space's new location automatically: the forwarding address that makes the portability promise mechanical (see the Civic Space Specification's portability contract and the Discovery Layer's re-binding protocol)

`civic.space.migrated` is not a process activity, so it omits `process_id`. Its payload:

```json
{
  "event_type": "civic.space.migrated",
  "data": {
    "space": {
      "space_id": "did:web:hub.floyd.example",
      "previous_binding": "https://hub.floyd.example",
      "new_binding": "https://floyd.civicspaces.example",
      "effective_at": "2026-08-01T00:00:00Z"
    }
  }
}
```

`space_id` is the space DID, which does **not** change across migration — it is the stable key consumers re-bind on. `previous_binding` and `new_binding` are the old and new serving URLs. `effective_at` is the RFC 3339 time from which the new binding is authoritative; a consumer that receives this activity SHOULD re-resolve the space DID at or after that time rather than trusting `new_binding` blindly, because the DID document is the authoritative record of the binding.

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

Each activity MAY include a `data` object.

### Data Structure Rule

`data` is namespaced by **noun** — the subject the activity is about — not by the activity type. Namespacing at all is what keeps two plugins from colliding on a generic key like `title`; namespacing by noun rather than by type is what lets a consumer read `data.vote` the same way across `vote_submitted`, `vote_updated`, and any future vote-related type, instead of relearning the payload for every verb.

The rule:

- Every `civic.process.*` activity's `data` MUST contain a `process` object carrying `process.type`, the process-type discriminator (see §4.5).
- `data` MAY contain at most one additional top-level key, naming the subject of the activity (e.g. `vote`, `comment`, `proposal`, `outcome`). Activities about the space rather than a process (§4.4) carry that subject key alone, with no `process` object.
- Activity-specific fields MUST NOT be placed at the top level of `data`. Put them inside the subject object, so that `data`'s top level stays a short, predictable set of nouns.

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

Lifecycle transitions MUST also emit activities. There are no silent state changes: any state a consumer can observe via a read endpoint MUST be reconstructible from the activity history.

---

## 7. Visibility & Disclosure Model (v0.1)

Defined in `meta.visibility`. This is the **wire-level visibility class**, and it has exactly two values in v0.1:

- **`public`** — the activity may be served to any caller, authenticated or not, and may be relayed, indexed, and cached without restriction.
- **`restricted`** — the activity may be served only to callers the emitting space has authorized to see it; who those callers are is a space-level access control decision (Civic Space Specification §4.7), not something this spec enumerates.

Two values, not five, because the wire format only has to answer one question — *may this be handed to an arbitrary caller?* — while the finer-grained policy that decides *which* callers lives in the process configuration and the space's authorization seam. Future versions may add credential-scoped visibility, which is what would let the wire format carry the finer distinction directly.

### Serving rule

A feed endpoint MUST omit `restricted` activities from responses to callers not authorized to see them. It MUST do so by returning an empty or filtered list, not by returning an error: an unauthorized caller and a caller for whom no activities exist MUST be indistinguishable in the response. Signalling "there is something here you may not see" is itself a disclosure, and in a civic context — where the existence of a restricted process can reveal that a jurisdiction is deliberating something sensitive — that leak matters.

### Mapping from process-level visibility

The Civic Process Specification configures visibility at the **policy** level, per process, with three values. This spec's `meta.visibility` is the **wire** level. Emitters map policy down onto wire as follows:

| Process `visibility` (policy) | `meta.visibility` (wire) |
|---|---|
| `public` | `public` |
| `participants-only` | `restricted` |
| `jurisdiction-only` | `restricted` |

The mapping is deliberately lossy: `participants-only` and `jurisdiction-only` differ in *who* is authorized, and the space enforces that difference at the serving rule above. They do not differ in whether an arbitrary caller may have the activity — for both, the answer is no — so they collapse to one wire value. A consumer that needs the finer distinction reads it from the process descriptor, not from the activity.

### Disclosure follows process configuration

Visibility classifies *who may see the activity*. **Disclosure** governs *what the payload may contain*, and is configured per process via the process descriptor's identity/disclosure policy (see the Civic Process Specification and the Identity Policy Object in the Civic Identity Specification). The binding rule:

- An activity's payload MUST NOT disclose participant data that the emitting process's disclosure policy protects. For example, a vote process configured for **secret ballots** MUST NOT include the participant's selected option in any activity visible beyond what the policy allows — participation may be public (`vote_submitted` with actor) while the ballot content is withheld or carried only in `restricted` activities.
- A process whose `disclosure_policy` is **`public`** — the on-the-record case, as in roll-call-style votes and public endorsements — MAY include the participant's choice in public activity payloads. That policy MUST appear in the process's published descriptor, so participants know before acting that their choice will be attributable to them.

---

## 8. Activity Transport (v0.1)

Activities can be delivered via:

- HTTP APIs
- Webhooks
- Feed endpoints

The transport endpoint is `GET /events` in v0.1 (see §14); a `GET /activities` alias may be introduced in a later revision. Feed responses are ordered by descending timestamp.

ActivityPub support is optional in v0.1 and is the target of the later federation bridge work.

### 8.1 Feed response format

The rest of this specification describes what an emitter puts on the wire. This subsection describes what a consumer receives, so that a client written against one space works against another without adjustment.

A feed response MUST be served with `Content-Type: application/json` and MUST use this envelope:

```json
{
  "items": [ ],
  "next_cursor": null
}
```

- `items` — an array of activities conforming to §2–3, ordered by descending `timestamp`. An empty array is a valid response and MUST NOT be replaced by an error (see the serving rule in §7).
- `next_cursor` — an opaque string to pass back as the `cursor` parameter to retrieve the next (older) page, or `null` when the caller has reached the end of the feed. Consumers MUST treat the cursor as opaque and MUST NOT parse it; its encoding is an implementation detail and emitters may change it at any time.

The activities are wrapped in an object rather than returned as a bare array so that pagination — and any future response-level metadata — can be added without changing the response's top-level type.

**Query parameters.** All are OPTIONAL.

| Parameter | Meaning |
|---|---|
| `process_id` | Return only activities for this process. |
| `type` | Return only activities of this `event_type`. Exact match on the full type string, and a single value in v0.1: repeating the parameter is not defined here, and a client that relies on it will not work against a conformant space. |
| `since` | Return only activities with a `timestamp` strictly later than this RFC 3339 value. |
| `limit` | Maximum number of activities to return. Default `50`, maximum `200`. A `limit` above the maximum MUST be clamped to the maximum rather than rejected. |
| `cursor` | An opaque `next_cursor` from a previous response. The cursor encodes the filter set of the request that produced it, and the endpoint MUST continue that same filtered sequence. A `since` supplied alongside a `cursor` MUST be ignored rather than re-applied, so that a paging client cannot accidentally skip or repeat activities. |

**Worked example.**

Request:

```
GET /events?process_id=0191c4d8-3e52-7a19-8b60-2c7d9f1a4e33&type=civic.process.vote_submitted&limit=1
Accept: application/json
```

Response:

```json
{
  "items": [
    {
      "id": "018f3a2c-7b41-7c3e-9d21-4a6f8b2e1c05",
      "version": "1.0",
      "event_type": "civic.process.vote_submitted",
      "timestamp": "2026-07-14T15:04:05-04:00",
      "process_id": "0191c4d8-3e52-7a19-8b60-2c7d9f1a4e33",
      "actor": "did:web:id.civic.example:u:9f2c1b7a",
      "jurisdiction": "us-va-floyd",
      "action_url": "https://hub.floyd.example/process/0191c4d8-3e52-7a19-8b60-2c7d9f1a4e33",
      "source": {
        "hub_id": "hub-floyd",
        "hub_url": "https://hub.floyd.example",
        "space_id": "did:web:hub.floyd.example"
      },
      "data": {
        "process": { "type": "civic.vote" },
        "vote": { "method": "yes_no", "recorded_at": "2026-07-14T15:04:05-04:00" }
      },
      "meta": { "visibility": "public" }
    }
  ],
  "next_cursor": "b2Zmc2V0OjE"
}
```

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

A complete AS2 bridge specification — including verb mapping, collection semantics, and the published JSON-LD context document at `https://civicsocial.org/ns/civic` (a prerequisite for both the bridge and the JSON-LD export format in the Civic Space Specification) — is deferred to the federation work.

---

## 10. Space Responsibilities

Every Civic Space (of any scope — hub, dashboard, representative space) MUST:

- Emit activities for all process activity through a **single emission path** (one chokepoint through which every activity flows)
- Validate the activity envelope at that emission path (required fields present, `process_id` populated for process activities, declared types only)
- Provide an activity feed endpoint
- Ensure activities conform to this schema

---

## 11. Minimal Compliance (v0.1)

### 11.1 Emitter requirements

To be compliant, an emitting system MUST:

- Emit valid activities
- Include the `version` field
- Include source attribution (`source.hub_id`, `source.hub_url`; `source.space_id` recommended)
- Include `meta.visibility`
- Use defined activity types (canonical or manifest-declared extensions per §4.5)
- Carry `data.process.type` on every `civic.process.*` activity
- Follow data namespacing rules
- Include all required fields (§3)
- Support at least one transport method

### 11.2 Consumer requirements

A **consumer** is any system that reads activities from a feed — a dashboard, an indexer, a notification service, another space. To be compliant, a consumer MUST:

- **Ignore activity types it does not recognize, without erroring.** An unrecognized `event_type` is a normal condition, not a malformed activity. A consumer MAY skip such an activity for its own processing, display it generically, or store it unprocessed; it MUST NOT reject the response, abort the page, or drop the other activities in `items` because one type was unfamiliar. Skipping settles only what this consumer does with the activity itself, and never licenses dropping it from a stream the consumer relays onward.
- **Ignore unrecognized fields, without erroring.** The same applies inside the envelope and inside `data`: unknown keys are additions, not errors.
- **Preserve unrecognized types and unknown fields when relaying.** A consumer that re-serves, forwards, caches, or exports activities MUST pass them through intact rather than emitting a narrowed copy containing only the fields it understood.
- **Do not widen an activity's audience when relaying.** The pass-through rule above governs *fields and types*, not *audience*. A consumer that re-serves activities MUST apply the serving rule of §7 to its own callers, and MUST NOT relay a `restricted` activity onward to a caller the emitting space has not authorized. Preserving the payload intact and preserving the audience are separate obligations, and neither one licenses breaking the other.

The reason is forward compatibility. The type registry in §4 will grow, and extension types (§4.5) are minted continuously by plugins without any change to this specification — so new activity types must be addable without breaking every consumer already deployed. A consumer that errors on the unfamiliar makes the registry effectively frozen; a consumer that silently narrows what it relays corrupts the stream for everyone downstream of it.

---

## 12. Out of Scope (v0.1)

- Activity signing (planned; see §13 — required before migrated/federated history can be independently verified)
- Federation guarantees
- Ranking algorithms
- **Cross-space deduplication on `dedupe_key`.** The key's semantics are defined in §3.1, and they stop at the emitting space. Recognizing the same activity across two spaces by `dedupe_key`, along with retention windows and any required consumer behavior for it, is deferred to a later version. This defers only `dedupe_key`-based cross-space dedupe: §3.1 already lets a consumer deduplicate across spaces on `id`, which is globally unique.

---

## 13. Future Extensions (v0.2+)

- Wire-format field rename `event_type` → `activity_type` and the `GET /activities` endpoint alias (see §14)
- `source.space_id` becomes required
- ActivityPub native support (the federation bridge) and ActivityStreams 2.0 verb-based type aliases
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

*This section is non-normative. It summarizes the intent of the specification and introduces no requirements; §§2–12 and §14 are the normative text.*

Civic Activities provide the backbone of distribution in the Civic.Social ecosystem.

What v0.1 fixes is the small set of things two independent implementations must agree on to read each other's streams: the activity envelope and its required fields, the canonical type registry and the namespace convention for extending it, the two-value wire visibility class, and the `GET /events` response envelope with its pagination and filter parameters. Everything else is deferred. Activity signing, federation guarantees, cross-space propagation, ranking, and normative deduplication are out of scope for this version (§12), and the ActivityStreams mapping in §9 is a compatibility sketch rather than a bridge specification. The envelope is designed so those can be added without invalidating activities already emitted.
