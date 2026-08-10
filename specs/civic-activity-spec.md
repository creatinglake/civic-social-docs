---
status: review
last-reviewed: 2026-07-26
owners: [adam]
version: 0.1
---

# Civic Activity Specification v0.1 (Hybrid Model)

> **Naming note — and what "Hybrid Model" means.** This specification deliberately joins two layers, and the title names that choice. The **model** is the Civic Activity: a vocabulary aligned with [ActivityStreams 2.0](https://www.w3.org/TR/activitystreams-core/) (AS2) and [ActivityPub](https://www.w3.org/TR/activitypub/), which the ecosystem bridges to in a later phase, and with the Civic Activity Feed layer that consumes these objects. The **wire format** is not ActivityStreams yet: for v0.1 it is the plain, deterministic JSON envelope of §2, with the field name `event_type` and the transport endpoint `GET /events`, matching every shipping implementation. The two are joined by design: the envelope is constructed so that every activity emitted today maps losslessly into an ActivityStreams 2.0 activity once the bridge lands (§9 is the sketch; the published civic JSON-LD context is the missing prerequisite), and §14 states the compatibility policy and the v0.2 wire-rename plan. Emitting AS2 today, before that context exists, would produce activities that *look* fediverse-compatible but carry civic semantics (jurisdiction, process linkage, visibility, disclosure) no AS2 consumer could interpret — worse than clean custom JSON, because it claims an interoperability it cannot deliver. The envelope is transport-agnostic and the ecosystem is protocol-agnostic by intent: the plan is to bridge to both major open social web protocols — ActivityStreams / ActivityPub first, with an AT Protocol bridge intended to follow (§13). Ratifying a stable, simple wire format first is how the ecosystem reaches both without breaking the implementations already emitting activities.

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

## Table of Contents

- Purpose
- Notation and Conformance Language
- **1. Design Principles**
- **2. Base Activity Schema**
  - 2.1 Complete Example
  - 2.2 Restricted Example
- **3. Required Fields**
  - 3.1 Field Formats
- **4. Activity Types (v0.1)**
  - 4.1 Lifecycle Activities
  - 4.2 Participation Activities
  - 4.3 Full-Lifecycle Activities
  - 4.4 Space Lifecycle Activities
  - 4.5 Extension Namespace Convention
- **5. Activity Data Payloads**
- **6. Action → Activity Relationship**
- **7. Visibility & Disclosure Model (v0.1)**
- **8. Activity Transport (v0.1)**
  - 8.1 Feed response format
- **9. ActivityStreams Mapping (Forward Compatibility)**
- **10. Space Responsibilities**
- **11. Minimal Compliance (v0.1)**
  - 11.1 Emitter requirements
  - 11.2 Consumer requirements
- **12. Out of Scope (v0.1)**
- **13. Future Extensions (v0.2+)**
- **14. Implementation Note — Code Terms and Wire Terms**
- **15. Open Questions**
- References
- Summary

---

## Notation and Conformance Language

**Notation.** The key words MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, SHOULD NOT, RECOMMENDED, MAY, and OPTIONAL in this document are to be interpreted as described in [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119) and [RFC 8174](https://www.rfc-editor.org/rfc/rfc8174) when, and only when, they appear in all capitals. The same words in lower case carry their ordinary English meaning and impose no requirement.

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
- `source.space_id` is the emitting space's **stable DID** (see the [Civic Space Specification](./civic-space-spec.md)). It is OPTIONAL for conformance in v0.1 — emitters SHOULD populate it from the start — and becomes REQUIRED in v0.2. Consumers SHOULD key provenance on `space_id` where present, because it survives migration of the space to a new URL or engine; `hub_url` identifies only the *current* serving location.

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

Two things in this example are worth reading closely. First, `data` carries exactly two keys — the mandatory `process` discriminator and one noun naming the subject of the activity (`vote`); that is the namespacing rule of §5. Second, `data.vote` records *that* a ballot was cast and by what method, but not *what* was chosen: this process is configured for secret ballots, so the participant's selection is withheld from a `public` activity under the disclosure rule in §7. The combination mirrors how paper elections work — the poll book is public (who voted is a matter of record, which is what makes turnout auditable), while the ballot is secret (which way they voted is never published). Visibility and disclosure answer different questions, and this activity is `public` *and* secret at once. A process configured for on-the-record voting could include the selection here; a process that wanted participation itself hidden would use restricted visibility, as in §2.2. Note that nothing in the activity itself marks the ballot as secret — the absence of the selection *is* the secrecy. The declaration that mandates it is the process's `disclosure_policy: "secret"`, published in the process descriptor that a consumer fetches at the activity's `action_url`; activities do not restate the policy, because the descriptor is its single source of truth. (Contrast the §5 payload example, where an on-the-record process includes `option_id`.)

### 2.2 Restricted Example

The §2.1 example is `public`. The activity below shows the other wire visibility class: a comment on a consultation whose process-level `visibility` is `participants-only`, which maps to `restricted` on the wire (§7). A feed serves this activity only to callers the emitting space has authorized; deciding *which* callers those are is the space's access-control decision and is not carried on the wire.

```json
{
  "id": "018f4b6e-2d93-7f10-b4c7-8e1a5d3f9b22",
  "version": "1.0",
  "event_type": "civic.process.comment_added",
  "timestamp": "2026-07-21T10:12:33-04:00",
  "process_id": "0191d2ab-64e0-7b55-9c02-3f8e6a1d7c44",
  "actor": "did:web:id.civic.example:u:4c8a2e91",
  "jurisdiction": "us-va-floyd",
  "action_url": "https://hub.floyd.example/process/0191d2ab-64e0-7b55-9c02-3f8e6a1d7c44",
  "source": {
    "hub_id": "hub-floyd",
    "hub_url": "https://hub.floyd.example",
    "space_id": "did:web:hub.floyd.example"
  },
  "data": {
    "process": { "type": "civic.consultation" },
    "comment": {
      "thread_id": "thread-204",
      "posted_at": "2026-07-21T10:12:33-04:00"
    }
  },
  "meta": {
    "visibility": "restricted"
  }
}
```

An unauthorized caller never learns this activity exists: per the serving rule in §7, it is omitted from their feed response rather than signalled by an error.

---

## 3. Required Fields

| Field | Required | Description |
|------|----------|------------|
| `id` | yes | Unique activity identifier |
| `version` | yes | Schema version of this activity object (`"1.0"` for this spec — distinct from this specification's own version number) |
| `event_type` | yes | Canonical activity type (see §14 for the field-name compatibility policy) |
| `timestamp` | yes | Activity creation time — RFC 3339 date-time with explicit offset (§3.1) |
| `process_id` | yes for process activities | Associated process. Non-process activities (e.g., space lifecycle activities, §4.4) omit it |
| `actor` | yes | Who performed the action: a DID, an opaque user identifier, or a system identifier (e.g. `system:auto-close`) |
| `jurisdiction` | yes | Geographic or community scope |
| `action_url` | yes | Link to view or take action |
| `source` | yes | Emitting space attribution (`hub_id`, `hub_url`; `space_id` OPTIONAL in v0.1, SHOULD be populated, REQUIRED in v0.2) |
| `data` | yes | Activity-specific payload (MAY be `{}`) |
| `meta.visibility` | yes | Visibility class (§7) |
| `dedupe_key` | optional | Idempotency hint: a stable string a consumer MAY use to suppress duplicate deliveries of the same activity (§3.1) |

This table is the single authoritative required-fields list for v0.1. A compliant activity includes every required field above; a compliant emitter never omits `version`, `source`, or `meta.visibility`.

### 3.1 Field Formats

The table says which fields are required; this section says what a valid value looks like. Two implementations that agree on the field list but disagree on these formats will not interoperate, so they are normative.

- **`id`** — a UUID ([RFC 9562](https://www.rfc-editor.org/rfc/rfc9562); any version, though UUIDv7 is RECOMMENDED because its time-ordering makes activity stores cheap to index) or an absolute URI. The identifier MUST be globally unique, not merely unique within the emitting space: an activity keeps the same `id` wherever it is relayed or re-served, so a consumer MAY use it to deduplicate the same activity received from more than one space.
- **`timestamp`** — an [RFC 3339](https://www.rfc-editor.org/rfc/rfc3339) date-time with an explicit timezone offset, e.g. `2026-07-14T15:04:05-04:00` or `2026-07-14T19:04:05Z`. A timestamp without an offset MUST NOT be emitted; a consumer cannot order a mixed-jurisdiction feed correctly without one.
- **`actor`** — the identity that performed the action, in one of three forms, distinguished by prefix so a consumer can tell them apart without out-of-band knowledge:
  - a **DID**, recognized by the `did:` prefix (e.g. `did:web:id.civic.example:u:9f2c1b7a`). Where the actor has a DID, the emitter MUST use it — *except* where the process's declared `disclosure_policy` is `anonymous` or `pseudonymous`, in which case the emitter SHOULD substitute a process-scoped opaque identifier and MUST NOT publish the mapping back to the DID ([Civic Process Specification](./civic-process-spec.md) §2.3, §7.3). A DID in `actor` is attributable and correlatable by design: that is what makes verified participation and portable history possible, and it is why a process that needs participation without attribution has to say so in its disclosure policy rather than relying on visibility.
  - a **system identifier**, recognized by the `system:` prefix (e.g. `system:auto-close`), for transitions performed by the space itself rather than a person.
  - an **opaque identifier** — anything else. An opaque identifier is scoped to the emitting space (identified by `source.space_id`, falling back to `source.hub_url`) and MUST NOT be assumed to denote the same participant across spaces. This is the form a space using a stub identity adapter emits ([Civic Space Specification](./civic-space-spec.md) §7.3).
- **`jurisdiction`** — a lowercase, hyphen-separated, hierarchical string ordered broadest to narrowest, e.g. `us-va-floyd` (country, state/region, locality). Deeper or shallower values are permitted (`us`, `us-va`, `us-va-floyd-ward3`) so long as the hierarchy reads left to right. Where an actor has no meaningful jurisdiction — an individual dashboard, a global organization — the emitter MUST use the literal string `"none"` rather than omitting the field or sending an empty string, so that consumers filtering by jurisdiction see an explicit answer rather than a gap. The field is deliberately geographic, and emitters MUST NOT encode organization or community names in it: consumers apply hierarchy semantics to the value (`us-va` contains `us-va-floyd`), and residency credentials key to the same strings. A community that is not a place — a national organization, a union, an issue network — is already identified on every activity it emits by `source.space_id`, and is discoverable through its space manifest and the discovery index; whether activities additionally need a cross-space, non-geographic grouping facility is an open question (§15).
- **`dedupe_key`** — an OPTIONAL idempotency hint: a stable string a consumer MAY use to suppress duplicate deliveries of the same activity. A consumer SHOULD treat two activities carrying the same `dedupe_key` from the same `source.space_id` as the same activity; the key is scoped to the emitting space and carries no meaning across spaces. It is a hint, not a guarantee, so an emitter MUST NOT rely on consumers deduplicating, and MUST NOT use `dedupe_key` as its only defense against emitting the same activity twice.

---

## 4. Activity Types (v0.1)

> The canonical type identifiers are noun-based (`civic.process.created`) for v0.1. A future revision may introduce verb-based aliases (`Create`, `Update`, `Announce`, etc.) to align more closely with the ActivityStreams 2.0 vocabulary as part of the AS2 bridge work (see §13).

**How to read this registry.** The registry is tiered by how much lifecycle a process opts into. Section 4.1 is the shared lifecycle vocabulary; 4.2 is what participants do inside a process. Which types any given process owes is determined by its declared **lifecycle profile** and tabulated in [Civic Process Specification](./civic-process-spec.md) §14.1, which is authoritative; only two emissions are universal across every profile — `civic.process.created`, and a lifecycle activity on the terminal transition (its §7.6). Section 4.3 is the phase vocabulary of the fuller lifecycle profiles: a process emits a §4.3 type exactly when its profile runs the phase the type reports, which reaches beyond the `deliberative` profile (`civic.process.framed`, for instance, is owed by `continuous` and `publish` processes too). Section 4.4 is the vocabulary of spaces themselves. Section 4.5 is the extension convention, and it is why this registry does not have to anticipate everything: the canonical types pin the shared grammar that lets strangers interoperate, while new vocabulary can always be minted without changing this specification.

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

The three named types are not process-specific: they are the activity-layer faces of the cross-cutting interaction primitives `vote`, `comment`, and `propose` from the Civic Process Specification's canonical `interaction_type` vocabulary (its §2.3), and they exist because consumers specialize on them — counting a participant's votes across every voting-shaped process, hanging disclosure rules on ballots, threading comments, linking follow-on activities to a proposal. The remaining canonical interactions (`deliberate`, `allocate`, `signal`) emit `civic.process.action_taken` carrying `data.process.type`, and are candidates for promotion to named types in a future version once implementations demonstrate consumers need shared semantics for them. This set is not closed: a plugin whose interaction warrants its own vocabulary MAY mint an extension type today under the §4.5 convention rather than waiting for promotion — the §4.5 worked example is exactly such a participation activity.

### 4.3 Full-Lifecycle Activities

> **Conditionally required; payloads provisional.** Whether a process must emit one of these types depends on whether its declared lifecycle profile actually runs the phase the type reports. The per-profile obligations are tabulated in **[Civic Process Specification](./civic-process-spec.md) §14.1, which is authoritative**; this section defines the identifiers and payloads, not who owes them. The distinction is which processes must emit them, not whether they are ratified.
>
> The **type identifiers** in this section are stable: implementers can hardcode them and match on them. The **payload shapes** are provisional — this is the least field-tested part of the registry, and no external implementation has exercised it yet, so `data` contents may be refined in v0.2. Build against the identifiers with confidence; treat unfamiliar keys inside `data` as forward-compatible additions rather than errors.

These types belong to the [Civic Process Specification](./civic-process-spec.md)'s full lifecycle model, which defines their payloads and per-profile obligations (its §7.4 and §14.1) — with one exception, noted on the type it concerns:

- `civic.process.framed`
- `civic.process.aggregation_completed`
- `civic.process.outcome_recorded`
- `civic.process.outcome_delivered` — an outcome formally delivered to a recipient (e.g., a Representative Space). *Implementation note:* the reference implementation currently emits this as `civic.outcome_delivered`; that spelling is ratified as a deprecated v0.1 alias and will migrate to the canonical namespaced form. *Definition status:* unlike its four siblings, this type's payload, emission point, and per-profile obligations are **not yet defined** in the Civic Process Specification; ratifying them there is queued for that specification's next revision. Until then, the type identifier is stable and emitters may use it, but its `data` shape should be treated as provisional even by the standards of this section's banner.
- `civic.process.feedback_received`

### 4.4 Space Lifecycle Activities

- `civic.space.migrated` — the final activity a space emits from its old location upon migration, carrying the new binding so feeds and indexers can re-bind to the space's new location automatically: the forwarding address that makes the portability promise mechanical (see the Civic Space Specification's portability contract and the [Discovery Layer](../ecosystem/discovery-layer-spec.md)'s re-binding protocol)

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
- Every activity that belongs to a process — every `civic.process.*` type, and any extension-type activity carrying a `process_id` — MUST carry the process-type discriminator `data.process.type` (e.g. `"data": { "process": { "type": "civic.wordcloud" } }`) so consumers can classify activities without reverse-engineering payload shapes (§5).
- Plugins MUST declare the activity types they emit in their plugin manifest (see the [Civic Plugin Architecture](../ecosystem/civic-plugin-architecture-spec.md)), and hosts SHOULD reject emissions of undeclared types.

**Worked example.** The activity below is a full envelope for an extension type minted by a plugin — a word-cloud plugin running on an entity-scoped Representative Space, so it also illustrates the §2 note that `source.hub_id` and `source.hub_url` carry the emitting space whatever its scope. Two things make this unfamiliar type legible to a stranger: the type name follows the `civic.<domain>.<verb>` convention, and `data` carries the `process` discriminator alongside the subject noun — required, because the activity carries a `process_id` (§5) — so a consumer that has never met a word cloud can still classify, attribute, and relay the activity.

```json
{
  "id": "018f5c81-9a44-7d02-8f36-b27e4c9d1a55",
  "version": "1.0",
  "event_type": "civic.wordcloud.word_submitted",
  "timestamp": "2026-07-22T18:45:10-04:00",
  "process_id": "0191e3bc-75f1-7c66-ad13-4a9f7b2e8d55",
  "actor": "did:web:id.civic.example:u:7d3f9c25",
  "jurisdiction": "us-va-floyd",
  "action_url": "https://supervisors.floyd.example/process/0191e3bc-75f1-7c66-ad13-4a9f7b2e8d55",
  "source": {
    "hub_id": "rep-floyd-bos",
    "hub_url": "https://supervisors.floyd.example",
    "space_id": "did:web:supervisors.floyd.example"
  },
  "data": {
    "process": { "type": "civic.wordcloud" },
    "word": { "text": "broadband" }
  },
  "meta": { "visibility": "public" }
}
```

---

## 5. Activity Data Payloads

Each activity MAY include a `data` object.

### Data Structure Rule

`data` is namespaced by **noun** — the subject the activity is about — not by the activity type. Namespacing at all is what keeps two plugins from colliding on a generic key like `title`; namespacing by noun rather than by type is what lets a consumer read `data.vote` the same way across `vote_submitted`, `vote_updated`, and any future vote-related type, instead of relearning the payload for every verb.

The rule:

- Every activity that belongs to a process — every `civic.process.*` activity, and any extension-type activity carrying a `process_id` — MUST contain in its `data` a `process` object carrying `process.type`, the process-type discriminator (see §4.5). The rule binds on `process_id`, not on the type prefix, so a plugin-minted type like `civic.wordcloud.word_submitted` carries the discriminator too.
- `data` MAY contain at most one additional top-level key, naming the subject of the activity (e.g. `vote`, `comment`, `proposal`, `outcome`). Activities about the space rather than a process (§4.4) carry that subject key alone, with no `process` object.
- Activity-specific fields MUST NOT be placed at the top level of `data`. Put them inside the subject object, so that `data`'s top level stays a short, predictable set of nouns.

Example (an **on-the-record** process — its `disclosure_policy` permits publishing the selection, which is why `option_id` appears here and not in the secret-ballot example of §2.1):

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
- **`restricted`** — the activity may be served only to callers the emitting space has authorized to see it; who those callers are is a space-level access control decision ([Civic Space Specification](./civic-space-spec.md) §4.7), not something this spec enumerates.

Two values, not five, because the wire format only has to answer one question — *may this be handed to an arbitrary caller?* — while the finer-grained policy that decides *which* callers lives in the process configuration and the space's authorization seam. Future versions may add credential-scoped visibility, which is what would let the wire format carry the finer distinction directly.

### Serving rule

A feed endpoint MUST omit `restricted` activities from responses to callers not authorized to see them. It MUST do so by returning an empty or filtered list, not by returning an error: an unauthorized caller and a caller for whom no activities exist MUST be indistinguishable in the response. Signalling "there is something here you may not see" is itself a disclosure, and in a civic context — where the existence of a restricted process can reveal that a jurisdiction is deliberating something sensitive — that leak matters.

### Mapping from process-level visibility

The Civic Process Specification configures visibility at the **policy** level, per process, with three values. This spec's `meta.visibility` is the **wire** level. Emitters map policy down onto wire as follows (this table restates the normative mapping owned by the Civic Process Specification's Publication phase — its §5, Phase 6; the two tables are one rule, and that document governs if they ever diverge):

| Process `visibility` (policy) | `meta.visibility` (wire) |
|---|---|
| `public` | `public` |
| `participants-only` | `restricted` |
| `jurisdiction-only` | `restricted` |

The two restricted policies name different audiences: `participants-only` is *those who acted* in the process, while `jurisdiction-only` is *those who belong* to its jurisdiction, whether or not they took part — and participation is defined by the process's own eligibility rules, which need not involve jurisdiction at all. A thirty-member citizen assembly in a county of fifteen thousand shows the difference: `participants-only` keeps the deliberation record with the thirty in the room; `jurisdiction-only` would open it to every credentialed resident.

The mapping is deliberately lossy: `participants-only` and `jurisdiction-only` differ in *who* is authorized, and the space enforces that difference at the serving rule above. They do not differ in whether an arbitrary caller may have the activity — for both, the answer is no — so they collapse to one wire value. A consumer that needs the finer distinction reads it from the process descriptor, not from the activity.

### Disclosure follows process configuration

Visibility classifies *who may see the activity*. **Disclosure** governs *what the payload may contain*, and is configured per process via the process descriptor's identity/disclosure policy (see the Civic Process Specification and the Identity Policy Object in the [Civic Identity Specification](./civic-identity-spec.md)). The binding rule:

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

Each Civic Activity can be mapped to an [ActivityStreams 2.0](https://www.w3.org/TR/activitystreams-vocabulary/) activity:

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
- `meta.visibility` → AS2 addressing: `public` maps to addressing that includes the well-known `as:Public` collection; `restricted` maps to limited addressing

**Native idioms.** Reuse comes first (the object model's stated design philosophy, Civic Space Specification §4): where the fediverse already has vocabulary for a civic activity, the bridge maps to it rather than wrapping generically. Three canonical types have established idioms:

- `civic.process.comment_added` → `Create` + `Note` with `inReplyTo` — the most widely implemented activity shape on the fediverse
- `civic.process.vote_submitted` → the established poll convention: `Create` + `Note` in reply to a `Question`, addressed only to the poll's author — an existing restricted-audience ballot idiom that aligns naturally with the disclosure rules of §7
- `civic.space.migrated` → the `Move` activity, already used by major fediverse implementations for account migration

Where no core verb fits, a bridged activity can be **dual-typed** — `"type": ["Create", "civic:VoteSubmitted"]` — so the core verb keeps it legible to generic clients while the civic extension term, defined in the civic JSON-LD context, carries the civic semantics. Extension contexts are AS2's own intended extension mechanism; the civic vocabulary extends the fediverse the way the fediverse is designed to be extended.

A complete AS2 bridge specification — including verb mapping, collection semantics, and the published [JSON-LD](https://www.w3.org/TR/json-ld11/) context document at `https://civicsocial.org/ns/civic` (a prerequisite for both the bridge and the JSON-LD export format in the Civic Space Specification) — is deferred to the federation work.

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
- Include source attribution (`source.hub_id`, `source.hub_url`; `source.space_id` SHOULD be populated)
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
- AT Protocol bridge — a civic Lexicon set over the same envelope; intended later target, pending evaluation (see the closing paragraph of this section)
- Activity signatures (signed by the emitting space's DID)
- Credential-scoped visibility
- Cross-space activity propagation
- Activity subscriptions and filtering

**Deferred by dependency, with seams reserved.** Each item above waits on a design artifact that does not yet exist — the civic JSON-LD context for the bridge; a canonicalization profile and the DID-method decision for signing; signing itself for verifiable propagation — and this specification does not write normative text ahead of the decisions it depends on. The v0.1 envelope already reserves each extension's attachment point: `source.space_id` identifies the future signer, globally unique `id` supports cross-relay deduplication, and the ignore-unknown-fields rule (§11.2) lets later additions ship without breaking deployed consumers. Every extension therefore lands additively: no activity emitted under v0.1 is invalidated. The envelope is transport-agnostic by design, and the ecosystem's intent is to bridge to both major open social web protocols: the ActivityStreams 2.0 / ActivityPub bridge is the first planned target, and an [AT Protocol](https://atproto.com/) bridge is an intended later target, pending evaluation. Each bridge requires only its own schema artifact — the civic JSON-LD context for AS2, a civic Lexicon set for AT Protocol — and no changes to this specification. Sequencing and pilot ownership for this work is tracked in [Civic.Social Horizons](../canon/phasing.md), not here.

---

## 14. Implementation Note — Code Terms and Wire Terms

The reference implementation uses the term **event** throughout its code (file names, function names, type names, the `/events` endpoint, the event store, the `emitEvent()` function). This is intentional: "event" is the long-standing term in event-sourced architectures, and renaming it across a codebase offers little engineering benefit.

The protocol-level term is **Civic Activity**. The ratified v0.1 wire format:

- The wire-format type field is **`event_type`** and the transport endpoint is **`GET /events`**. This matches all shipping implementations and is the compatibility baseline external consumers may rely on for the life of v0.1.
- The planned v0.2 revision renames the wire field to `activity_type` and introduces `GET /activities`, coordinated with the AS2 bridge work. v0.2 emitters will dual-emit or alias for a documented deprecation window.

### Conformance phasing

The reference implementation ([`civic-hub`](https://github.com/Mosaic-Foundation/Civic-Social-Mono/tree/main/civic-hub)) currently implements this specification's required envelope, single-emission-path, and feed-endpoint requirements; envelope validation at emission, `source.space_id`, and activity signing are targeted through the Civic Hubs and Civic Activity Feed & Discovery pilots. Specifications in this ecosystem define the target state; reference implementations converge on them through the pilot program.

---

## 15. Open Questions

*This section is non-normative. It records design questions this version deliberately leaves open.*

- **Namespace generalization.** The `civic.*` type prefix, the `civic.json` well-known path ([Civic Space Specification](./civic-space-spec.md) §7.2.0), and the planned JSON-LD context at `civicsocial.org/ns/civic` name the civic profile of primitives that are deliberately domain-general: scoped spaces, typed processes, an activity envelope. Whether and when a domain-neutral namespace is introduced above the civic one — with `civic.*` types aliased into it under the same deprecation machinery as the §14 wire rename — is an open question. One decision cannot wait for it: the JSON-LD context URL should be chosen with this question in mind before it is published, because a published context URL is the identifier in this ecosystem least amenable to later change.

- **Non-geographic grouping.** `jurisdiction` is geographic by design (§3.1), and the emitting community is identified by `source.space_id` — but v0.1 has no way to mark activities as belonging to a grouping that spans spaces: an issue campaign run across twenty hubs, a coalition, a topic. Whether that facility arrives as a `topics` field on the envelope, as a discovery-layer construct over space metadata, or through the extension namespace is undecided. The need is real for issue networks and multi-space campaigns; encoding organization or campaign names into `jurisdiction` is the misuse this question exists to prevent.

---

## References

**Normative.** These documents are required to implement this specification as written.

- **RFC 2119** and **RFC 8174** (BCP 14) — requirement keywords, per Notation and Conformance Language. https://www.rfc-editor.org/rfc/rfc2119 · https://www.rfc-editor.org/rfc/rfc8174
- **RFC 3339** — *Date and Time on the Internet: Timestamps* — the `timestamp` format (§3.1). https://www.rfc-editor.org/rfc/rfc3339
- **RFC 9562** — *Universally Unique IDentifiers (UUIDs)* — the `id` format, including the UUIDv7 variant this specification recommends (§3.1). https://www.rfc-editor.org/rfc/rfc9562
- **RFC 8259** — *The JavaScript Object Notation (JSON) Data Interchange Format* — the v0.1 wire serialization. https://www.rfc-editor.org/rfc/rfc8259

**Informative.** These documents shape the model and the planned bridges; nothing in v0.1 conformance binds on them. The first two become normative with the bridge specification (§13).

- **ActivityStreams 2.0** — W3C Recommendation — the vocabulary this specification's model aligns with, and the target of the §9 mapping. Core: https://www.w3.org/TR/activitystreams-core/ · Vocabulary: https://www.w3.org/TR/activitystreams-vocabulary/
- **ActivityPub** — W3C Recommendation — the first planned federation transport. https://www.w3.org/TR/activitypub/
- **JSON-LD 1.1** — W3C Recommendation — the format of the planned civic context document at `civicsocial.org/ns/civic`. https://www.w3.org/TR/json-ld11/
- **AT Protocol** — protocol documentation published at https://atproto.com/ — the intended later bridge target (§13); a civic Lexicon set would be the bridge's schema artifact.
- **Companion specifications** — the [Civic Space](./civic-space-spec.md), [Civic Process](./civic-process-spec.md), and [Civic Identity](./civic-identity-spec.md) Specifications; the [Civic Plugin Architecture](../ecosystem/civic-plugin-architecture-spec.md); the [Discovery Layer Specification](../ecosystem/discovery-layer-spec.md); and **[Civic.Social Horizons](../canon/phasing.md)**, which places this specification's deferrals on the ecosystem's timeline.

---

## Summary

*This section is non-normative. It summarizes the intent of the specification and introduces no requirements; §§2–12 and §14 are the normative text.*

Civic Activities provide the backbone of distribution in the Civic.Social ecosystem.

What v0.1 fixes is the small set of things two independent implementations must agree on to read each other's streams: the activity envelope and its required fields, the canonical type registry and the namespace convention for extending it, the two-value wire visibility class, and the `GET /events` response envelope with its pagination and filter parameters. Everything else is deferred. Activity signing, federation guarantees, cross-space propagation, ranking, and normative deduplication are out of scope for this version (§12), and the ActivityStreams mapping in §9 is a compatibility sketch rather than a bridge specification. The envelope is designed so those can be added without invalidating activities already emitted.
