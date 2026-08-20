---
status: draft
last-reviewed: 2026-08-20
owners: [adam]
version: 0.2
---

# Civic Activity Specification v0.2 (ActivityStreams Profile)

> **What this document is.** A **Civic Activity is an [ActivityStreams 2.0](https://www.w3.org/TR/activitystreams-core/) (AS2) activity** that conforms to the profile defined here. This specification does not define a wire format of its own: the wire format is AS2, serialized as JSON-LD, and this document defines (a) the profile — which AS2 properties a Civic Activity uses and how — and (b) the **civic vocabulary** — the small set of terms, published in the [civic JSON-LD context](./civic.jsonld), for the things AS2 has no words for: processes, ballots, outcomes, civic geography. A consumer that speaks AS2 can read a Civic Activity today, ignoring the civic terms it does not know; a consumer that also loads the civic context gets the full civic semantics. [Appendix A](#appendix-a-why-a-profile) explains why this is a profile of AS2 rather than pure AS2, and what each civic term adds.
>
> **Reading order.** This document is self-contained for implementing the activity layer. It references the Civic Process Specification for process-level policy vocabulary (lifecycle profiles, disclosure policy) — you do not need to read that document first, but implementers of process types eventually will. Companion specifications are being aligned to this revision; where an older companion contradicts this document, this document governs for everything on the wire.
>
> **v0.1 note.** Version 0.1 of this specification defined a standalone JSON envelope (`event_type`, `data`, `meta.visibility`, a custom feed format). That envelope is retired, not bridged: the reference implementation converted directly, and no external consumers of the old wire exist. [Appendix B](#appendix-b-mapping-from-the-v01-envelope) maps every v0.1 field to its v0.2 home.
>
> **Status: provisional.** This is a working draft published for community discussion — a deliberately **minimum plausible specification**, defining the least that two independent implementations need to interoperate, no more. It will be revised as the ecosystem and its implementations evolve, and pre-1.0 revisions may include breaking changes. Corrections, challenges, and suggestions are welcome from anyone: open a pull request or an issue on [the repository](https://github.com/Mosaic-Foundation/civic-social-docs), or see [CONTRIBUTING.md](../CONTRIBUTING.md) for other ways to weigh in.

---

## Table of Contents

- Purpose
- Notation and Conformance Language
- **1. Design Principles**
- **2. The Civic Activity Document**
  - 2.1 Profile Requirements
  - 2.2 Property Requirements and Formats
  - 2.3 Complete Example
  - 2.4 Restricted Example
- **3. The Civic Type Vocabulary**
  - 3.1 How Types Are Assigned
  - 3.2 Type Registry
  - 3.3 Space Migration
  - 3.4 Extensions
- **4. Process Linkage**
- **5. Visibility and Disclosure**
  - 5.1 Addressing
  - 5.2 The Serving Rule
  - 5.3 Disclosure Constrains Payloads
  - 5.4 Ballots and the Withheld Selection
- **6. Transport and Collections**
  - 6.1 The Activity Collection
  - 6.2 Query Parameters
  - 6.3 The Conformance Ladder
- **7. Emitter Requirements**
- **8. Consumer Requirements**
- **9. Out of Scope (v0.2)**
- **10. Future Extensions**
- **11. Open Questions**
- **Appendix A: Why a Profile**
- **Appendix B: Mapping from the v0.1 Envelope**
- **References**

---

## Purpose

Civic Activities are the **distribution layer** of the Civic.Social ecosystem: one shared stream of what is happening across processes and spaces, refracted into many lenses (feeds, notifications, discovery, embeds) by any consumer that cares to read it.

This specification fixes the small set of things two independent implementations must agree on to read each other's streams: the document profile ([§2](#2-the-civic-activity-document)), the type vocabulary ([§3](#3-the-civic-type-vocabulary)), process linkage ([§4](#4-process-linkage)), the visibility model ([§5](#5-visibility-and-disclosure)), and the collection transport ([§6](#6-transport-and-collections)). Everything an emitter or consumer must do is stated here; everything deferred is named in [§9](#9-out-of-scope-v02)–[§11](#11-open-questions).

> **Which horizon is this?** [§2](#2-the-civic-activity-document)–[§5](#5-visibility-and-disclosure) and conformance Level 1 of [§6](#6-transport-and-collections) can be built and verified today. Level 2 (federated delivery) is specified as the target state and implemented incrementally — **[Civic.Social Horizons](../canon/phasing.md)** places each deferral on the ecosystem's timeline.

---

## Notation and Conformance Language

**Notation.** The key words MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, SHOULD NOT, RECOMMENDED, MAY, and OPTIONAL in this document are to be interpreted as described in [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119) and [RFC 8174](https://www.rfc-editor.org/rfc/rfc8174) when, and only when, they appear in all capitals. The same words in lower case carry their ordinary English meaning and impose no requirement.

---

## 1. Design Principles

1. **Reuse before invention.** Every property and type that AS2 defines is used as AS2 defines it. The civic vocabulary adds only what AS2 lacks, and [Appendix A](#appendix-a-why-a-profile) justifies each addition.
2. **Activity-first.** No silent state changes: any state a consumer can observe through a read endpoint MUST be reconstructible from the activity stream.
3. **Plain JSON that is valid JSON-LD.** Documents are ordinary JSON with a fixed `@context`; producing and consuming them requires no JSON-LD processor. Implementations MAY apply full JSON-LD processing and get identical semantics — this is the same compact-document convention the fediverse runs on.
4. **Legible to strangers.** A generic AS2 consumer can read, display, and relay every Civic Activity without knowing any civic term. Civic terms deepen meaning; they are never load-bearing for basic handling.
5. **Additive evolution.** New types and terms arrive without breaking deployed consumers ([§8](#8-consumer-requirements)).

---

## 2. The Civic Activity Document

### 2.1 Profile Requirements

A **Civic Activity** is a JSON-LD document that:

1. is a valid AS2 activity ([ActivityStreams 2.0 Core](https://www.w3.org/TR/activitystreams-core/));
2. declares the AS2 context and the civic context, in that order:

```json
"@context": [
  "https://www.w3.org/ns/activitystreams",
  "https://civic.social/ns/civic"
]
```

3. satisfies the property requirements of [§2.2](#22-property-requirements-and-formats); and
4. uses types per [§3](#3-the-civic-type-vocabulary).

> The civic context IRI is **provisional until published** (see Open Question 1, [§11](#11-open-questions)). The context document's current draft ships alongside this specification as [`civic.jsonld`](./civic.jsonld).

### 2.2 Property Requirements and Formats

| Property | Requirement | Profile rules |
|---|---|---|
| `id` | MUST | An IRI, globally unique. For activities minted by a space, `urn:uuid:{uuid}` is RECOMMENDED (UUIDv7 preferred — its time-ordering makes activity stores cheap to index). An activity keeps the same `id` wherever it is relayed, so consumers MAY deduplicate on it across sources. |
| `type` | MUST | Per [§3.1](#31-how-types-are-assigned). |
| `actor` | MUST | An IRI, or an object with an `id`, identifying who acted ([§2.2.1](#221-actors)). |
| `published` | MUST | An [RFC 3339](https://www.rfc-editor.org/rfc/rfc3339) date-time **with an explicit offset** (`2026-07-21T10:12:33-04:00` or `...Z`). Offsets are permitted deliberately: they denote unambiguous instants, and local time of day is civically meaningful. |
| `to` / `cc` | MUST (at least one recipient between them) | The audience, per [§5.1](#51-addressing). |
| `generator` | MUST | The emitting space, as an object: its **stable identifier** (the space DID) as `id`, its space type (e.g. `civic:Hub`) in `type`, and its current serving location as `url`. Consumers SHOULD key provenance on `generator.id`; it survives migration, while `url` names only the current location. |
| `context` | MUST for process activities | The process this activity belongs to ([§4](#4-process-linkage)). Non-process activities (e.g. space migration, [§3.3](#33-space-migration)) omit it. |
| `object` | Per AS2 | Required by AS2 for the verbs this profile uses; carries the subject of the activity, typed per [§3](#3-the-civic-type-vocabulary). |
| `location` | SHOULD, where the activity has civic geography | An AS2 `Place`; civic filtering uses its `civic:code` ([§2.2.2](#222-civic-geography-location-and-civiccode)). Omit entirely where no place applies (an individual dashboard, a global organization) — there is no null sentinel in this profile. |
| `url` | SHOULD | Where a person can view the activity's subject or take the corresponding action. |

Unrecognized properties are permitted everywhere (they are how extension happens) and are governed by [§8](#8-consumer-requirements).

#### 2.2.1 Actors

Three kinds of actor, all IRIs:

- **A participant's DID** (`did:web:id.civic.example:u:9f2c1b7a`) — used whenever the actor has one, **except** where the process's disclosure policy is `anonymous` or `pseudonymous`: the emitter then substitutes a process-scoped actor IRI and MUST NOT publish the mapping back to the DID. A DID actor is attributable and correlatable; this is the v0.1 identity design and its limits, alternatives, and open questions are stated in the Civic Identity Specification (its §7.4 and §13), not glossed here.
- **A space-scoped participant IRI** (`https://hub.floyd.example/users/9f2c1b7a`) — the form a space running a stub identity adapter emits. Scoped to the emitting space; MUST NOT be assumed to denote the same person anywhere else.
- **A space-scoped system IRI** (`https://hub.floyd.example/system/auto-close`) — for transitions performed by the space's own machinery rather than a person. The IRI MUST name the specific component, not a generic "system," so audit logs can distinguish which part of the system acted.

#### 2.2.2 Civic geography: `location` and `civic:code`

Civic geography rides on the standard AS2 `location` property with a `Place` object. The one civic addition is **`civic:code`**: a lowercase, hyphen-separated, hierarchical place code ordered broadest to narrowest — `us`, `us-va`, `us-va-floyd`, `us-va-floyd-ward3` — which is what consumers filter on (`us-va` contains `us-va-floyd`).

```json
"location": {
  "type": "Place",
  "name": "Floyd County, Virginia",
  "civic:code": "us-va-floyd"
}
```

Two rules: emitters MUST NOT encode organization or community names in `civic:code` (consumers apply geographic hierarchy semantics to it; a community that is not a place is already identified by `generator.id` and discoverable through the discovery index), and the code says nothing about *who governs* — administrative authority is a property of the emitting space and its verification credentials, not of the place code. Whether activities need a separate cross-space, non-geographic grouping facility is Open Question 4 ([§11](#11-open-questions)).

### 2.3 Complete Example

A comment submitted to a public consultation. Every MUST-level property is present with realistic values; implementers can use this as a test fixture. Note that a generic fediverse consumer reads this as an ordinary `Create` of a `Note` — the civic terms deepen it without being required for display.

```json
{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
    "https://civic.social/ns/civic"
  ],
  "id": "urn:uuid:018f4b6e-2d93-7f10-b4c7-8e1a5d3f9b22",
  "type": "Create",
  "actor": "did:web:id.civic.example:u:4c8a2e91",
  "published": "2026-07-21T10:12:33-04:00",
  "to": ["https://www.w3.org/ns/activitystreams#Public"],
  "generator": {
    "id": "did:web:hub.floyd.example",
    "type": ["Organization", "civic:Hub"],
    "name": "Floyd Civic Hub",
    "url": "https://hub.floyd.example"
  },
  "location": {
    "type": "Place",
    "name": "Floyd County, Virginia",
    "civic:code": "us-va-floyd"
  },
  "context": {
    "id": "https://hub.floyd.example/process/0191d2ab-64e0-7b55-9c02-3f8e6a1d7c44",
    "type": "civic:Process",
    "civic:processType": "civic:Consultation"
  },
  "object": {
    "id": "urn:uuid:018f4b6e-30a1-7d44-8c15-6e2b9d4f0a77",
    "type": "Note",
    "content": "The proposed route would cut off Alum Ridge Road access for school buses.",
    "inReplyTo": "https://hub.floyd.example/process/0191d2ab-64e0-7b55-9c02-3f8e6a1d7c44/thread/204"
  },
  "url": "https://hub.floyd.example/process/0191d2ab-64e0-7b55-9c02-3f8e6a1d7c44"
}
```

### 2.4 Restricted Example

The same shape with a limited audience: a comment in a participants-only consultation. Instead of `as:Public`, the audience is a collection the emitting space controls; the serving rule ([§5.2](#52-the-serving-rule)) governs who ever sees it.

```json
{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
    "https://civic.social/ns/civic"
  ],
  "id": "urn:uuid:018f4b6e-4f02-7e88-9a31-1c7d8e2b5f90",
  "type": "Create",
  "actor": "did:web:id.civic.example:u:4c8a2e91",
  "published": "2026-07-21T10:12:33-04:00",
  "to": ["https://hub.floyd.example/process/0191d2ab-64e0-7b55-9c02-3f8e6a1d7c44/participants"],
  "generator": {
    "id": "did:web:hub.floyd.example",
    "type": ["Organization", "civic:Hub"],
    "url": "https://hub.floyd.example"
  },
  "context": {
    "id": "https://hub.floyd.example/process/0191d2ab-64e0-7b55-9c02-3f8e6a1d7c44",
    "type": "civic:Process",
    "civic:processType": "civic:Consultation"
  },
  "object": {
    "type": "Note",
    "content": "…",
    "inReplyTo": "https://hub.floyd.example/process/0191d2ab-64e0-7b55-9c02-3f8e6a1d7c44/thread/204"
  },
  "url": "https://hub.floyd.example/process/0191d2ab-64e0-7b55-9c02-3f8e6a1d7c44"
}
```

An unauthorized caller never learns this activity exists: per [§5.2](#52-the-serving-rule) it is omitted from their collection page, not signalled by an error.

---

## 3. The Civic Type Vocabulary

### 3.1 How Types Are Assigned

The profile's rule, in order of preference:

1. **Where an AS2 verb natively expresses the action, use it alone**, with civic meaning carried by the typed `object` and by `context`. A comment is `Create` + `Note`; a ballot is `Create` + `civic:Ballot`; a space migration is `Move`.
2. **Where no AS2 verb fits, use a civic activity type as the single `type` value.** Only three exist: `civic:Frame`, `civic:Start`, `civic:End`.
3. **The activity's own `type` is serialized as a single string** in this profile's canonical form, because single-string activity types are the most widely interoperable form in deployed fediverse software; whether to move to dual-typed activity arrays is Open Question 2 ([§11](#11-open-questions)). Types on *objects and actors* MAY be arrays — the examples dual-type the emitting space as `["Organization", "civic:Hub"]` precisely so generic consumers classify it while civic consumers refine it.

The result is a deliberately small vocabulary: three civic activity types, a handful of civic object classes, and AS2 for everything else.

### 3.2 Type Registry

Each entry: the serialization, a definition, and when it is emitted. Per-lifecycle-profile obligations (which processes owe which emissions) are the Civic Process Specification's to define; two emissions are universal for every process regardless of profile — its creation, and an activity on its terminal transition.

**Process lifecycle**

| Activity | Serialization | Definition |
|---|---|---|
| Process created | `Create` + object `civic:Process` | A new process instance came into being. The object is (a reference to) the process itself; dereferencing its `id` returns the process descriptor. |
| Process updated | `Update` + object `civic:Process` | Mutable process metadata changed (description, timeline, outcome status). |
| Process framed | `civic:Frame` + object `civic:Process` | The process's configuration was finalized: rules, eligibility, timeline, disclosure policy. Emitted once framing completes, before participation opens. |
| Process started | `civic:Start` + object `civic:Process` | The process became active; participation is open. |
| Process ended | `civic:End` + object `civic:Process` | The process reached a terminal state. The object carries `civic:terminalState` (e.g. `closed`, or a profile alias like `archived`) so consumers can detect the end of any process without knowing its lifecycle profile. |
| Result published | `Announce` + object `civic:Result` | The process's structured result was made available per its visibility rules. `Announce` is AS2's calling-attention verb — publication of a result is exactly that. |

**Participation**

| Activity | Serialization | Definition |
|---|---|---|
| Comment added | `Create` + object `Note` (with `inReplyTo`) | A participant contributed deliberative speech to a process. The most native fediverse shape there is. |
| Ballot cast | `Create` + object `civic:Ballot` | A participant cast a ballot in a voting-shaped process. The `civic:Ballot` object carries `civic:method` (e.g. `yes_no`, `approval`, `ranked_choice`) and — only where the disclosure policy permits — the selection ([§5.4](#54-ballots-and-the-withheld-selection)). |
| Proposal created | `Create` + object `civic:Proposal` | A participant submitted a proposal: a referent other activities and processes will link to. |
| Feedback given | `Create` + object `civic:Feedback` | A participant submitted structured feedback about a finalized process (its fairness, accessibility, quality). |
| Generic action | `Create` (or the nearest AS2 verb) + a typed extension object | Any participation with no shape above. The object's type — an extension term ([§3.4](#34-extensions)) — is what makes it classifiable; `context` still links it to its process. |

**Outcomes**

| Activity | Serialization | Definition |
|---|---|---|
| Outcome recorded | `Create` + object `civic:Outcome` | The real-world interpretation of a result was formally recorded: a recommendation adopted, a decision enacted, a report accepted. |
| Outcome delivered | `Announce` + object `civic:Outcome` + `target` | An outcome was formally delivered to a recipient — typically an entity-scoped space (a Representative Space). `target` names the recipient. |

Object classes referenced above (`civic:Process`, `civic:Ballot`, `civic:Proposal`, `civic:Result`, `civic:Outcome`, `civic:Feedback`) and space types (`civic:Hub`, `civic:Dashboard`, `civic:RepresentativeSpace`) are defined in the civic context; their full property shapes belong to the specifications that own them (the process descriptor to the Civic Process Specification, space objects to the Civic Space Specification) and are being aligned to this revision.

### 3.3 Space Migration

A space announcing its move to a new location is pure AS2 — the `Move` verb, the same one fediverse software already uses for account migration:

```json
{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
    "https://civic.social/ns/civic"
  ],
  "id": "urn:uuid:018f5c81-77aa-7b31-9d02-4e8c1f6a2b44",
  "type": "Move",
  "actor": "did:web:hub.floyd.example",
  "published": "2026-08-01T00:00:00Z",
  "to": ["https://www.w3.org/ns/activitystreams#Public"],
  "generator": { "id": "did:web:hub.floyd.example", "type": ["Organization", "civic:Hub"] },
  "object": "did:web:hub.floyd.example",
  "origin": { "type": "Link", "href": "https://hub.floyd.example" },
  "target": { "type": "Link", "href": "https://floyd.civicspaces.example" }
}
```

The space DID (`object`, and `generator.id`) does not change across migration — it is the stable key consumers re-bind on. A consumer receiving this SHOULD re-resolve the space DID rather than trusting `target` blindly, because the DID document is the authoritative record of the binding. This is the final activity emitted from the old location; the Discovery Layer Specification defines the tombstone and re-binding protocol around it.

### 3.4 Extensions

Third parties — plugins, other space types, other ecosystems — extend the vocabulary **in their own namespaces**, never in `civic:` or `as:`:

- An extension defines its terms in its **own JSON-LD context** with IRIs under a domain its author controls, and documents include that context alongside the two in [§2.1](#21-profile-requirements). Example: a word-cloud plugin by `plugins.example` might define `https://plugins.example/ns/wordcloud#Word` and emit `Create` activities whose object is typed with that IRI.
- Extension terms MUST NOT redefine the semantics of AS2 or civic terms.
- Plugins MUST declare the activity and object types they emit in their plugin manifest (see the [Civic Plugin Architecture](../ecosystem/civic-plugin-architecture-spec.md)), and hosts SHOULD reject emissions of undeclared types.

This gives a clean separation between core and third-party vocabulary with no possibility of name collision: names are IRIs, and IRIs carry their author's domain. A civic term can later be *promoted* from an extension into the civic context when consumers demonstrably need shared semantics for it.

---

## 4. Process Linkage

Every activity that belongs to a process MUST carry `context`: either the process's IRI, or (RECOMMENDED) an object with the process's `id`, `type: "civic:Process"`, and `civic:processType`.

Two properties of this design do real work:

1. **The `context` IRI dereferences to the process descriptor.** `GET` on it returns the process document — its rules, eligibility, disclosure policy, actions, and state. The descriptor is the single source of truth for process policy; activities never restate it. (AS2 defines `context` precisely for grouping objects around a common endeavor — this is its intended use.)
2. **`civic:processType` is the inline classifier.** A consumer that has never met a process type — `civic:Consultation`, `civic:Vote`, or a third-party extension type — can still group, filter, and route its activities without fetching the descriptor. Canonical process-type terms are the Civic Process Specification's registry to define; extension process types use their own IRIs per [§3.4](#34-extensions).

---

## 5. Visibility and Disclosure

Visibility (*who may see an activity*) and disclosure (*what an activity says about its participant*) are independent, and the profile keeps them so. A process can be publicly visible while disclosing nothing about how any participant acted.

### 5.1 Addressing

Visibility is expressed through standard AS2 addressing:

- **Public**: the audience (in `to` or `cc`) includes `https://www.w3.org/ns/activitystreams#Public`. The activity may be served to any caller, relayed, indexed, and cached without restriction.
- **Restricted**: the audience does not include `as:Public`. The audience values are IRIs meaningful to the emitting space — typically space-managed collections such as a process's `/participants` collection. Which callers those resolve to is the space's access-control decision, enforced at its authorization seam; the finer policy distinctions (participants-only versus jurisdiction-only, defined by the Civic Process Specification) are configured per process and enforced by the space, not enumerated on the wire.

The emitting space MUST set addressing from the process's declared visibility policy: `public` → includes `as:Public`; anything else → restricted addressing.

### 5.2 The Serving Rule

A collection endpoint ([§6](#6-transport-and-collections)) MUST omit restricted activities from responses to callers not authorized for their audience — by returning a filtered (possibly empty) page, **never** by returning an error. An unauthorized caller and a caller for whom no activities exist MUST be indistinguishable. Signalling "there is something here you may not see" is itself a disclosure, and in civic contexts the mere existence of a restricted process can reveal that a community is deliberating something sensitive.

### 5.3 Disclosure Constrains Payloads

An activity's payload MUST NOT disclose participant data that the process's declared disclosure policy protects. The policy lives in the process descriptor (dereference `context`), is declared before participation begins, and binds every emission for that process. Where the policy is `anonymous` or `pseudonymous`, the constraint reaches the `actor` property itself ([§2.2.1](#221-actors)).

### 5.4 Ballots and the Withheld Selection

The worked case, because it is the one implementers get wrong. A ballot cast in a process whose disclosure policy withholds selections:

```json
{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
    "https://civic.social/ns/civic"
  ],
  "id": "urn:uuid:018f3a2c-7b41-7c3e-9d21-4a6f8b2e1c05",
  "type": "Create",
  "actor": "did:web:id.civic.example:u:9f2c1b7a",
  "published": "2026-07-14T15:04:05-04:00",
  "to": ["https://www.w3.org/ns/activitystreams#Public"],
  "generator": { "id": "did:web:hub.floyd.example", "type": ["Organization", "civic:Hub"], "url": "https://hub.floyd.example" },
  "location": { "type": "Place", "name": "Floyd County, Virginia", "civic:code": "us-va-floyd" },
  "context": {
    "id": "https://hub.floyd.example/process/0191c4d8-3e52-7a19-8b60-2c7d9f1a4e33",
    "type": "civic:Process",
    "civic:processType": "civic:Vote"
  },
  "object": {
    "type": "civic:Ballot",
    "civic:method": "yes_no"
  },
  "url": "https://hub.floyd.example/process/0191c4d8-3e52-7a19-8b60-2c7d9f1a4e33"
}
```

The activity is public — *that* this participant cast a ballot is on the record, the way a poll book is public — but the `civic:Ballot` object carries no selection. **"Withheld selection" means exactly this and nothing more: the participant's choice is not published in activities.** It is a serialization rule, not a security claim: how stored records relate voter to choice is the Civic Process Specification's territory, and what the space operator can know is stated candidly in the Civic Identity Specification (its §7.4). A process configured for on-the-record voting (roll-call votes, public endorsements) includes the selection in the object; that policy is visible in the descriptor before anyone acts. The near-term use of voting in this ecosystem is advisory voting — deliberately low-stakes while these patterns are validated with real communities.

---

## 6. Transport and Collections

### 6.1 The Activity Collection

A space publishes its activities as an AS2 **[`OrderedCollection`](https://www.w3.org/TR/activitystreams-core/#collections)**, newest first, paged with `OrderedCollectionPage`. The reference endpoint path is `GET /events` (the path is arbitrary; consumers discover it from the space manifest's `feeds` array, and, at Level 2, from the actor document's `outbox`).

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://hub.floyd.example/events",
  "type": "OrderedCollection",
  "totalItems": 1240,
  "first": "https://hub.floyd.example/events?page=true"
}
```

A page:

```json
{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
    "https://civic.social/ns/civic"
  ],
  "id": "https://hub.floyd.example/events?page=true",
  "type": "OrderedCollectionPage",
  "partOf": "https://hub.floyd.example/events",
  "orderedItems": [],
  "next": "https://hub.floyd.example/events?page=true&cursor=b2Zmc2V0OjE"
}
```

- `orderedItems` carries activities conforming to [§2](#2-the-civic-activity-document), ordered by `published` descending. An empty array is a valid page and MUST NOT be replaced by an error ([§5.2](#52-the-serving-rule)).
- `next` links the following (older) page and is absent on the last page. Cursor values inside page URLs are opaque; consumers MUST NOT parse them.
- Served with `Content-Type: application/activity+json` (spaces SHOULD also accept and honor `Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"`).

### 6.2 Query Parameters

Profile extras a generic AS2 consumer can ignore; all OPTIONAL:

| Parameter | Meaning |
|---|---|
| `context` | Only activities whose `context` matches this process IRI. |
| `type` | Only activities of this `type` (exact match, single value). |
| `since` | Only activities `published` strictly later than this RFC 3339 value. |
| `limit` | Maximum items per page. Default 50, maximum 200; values above the maximum are clamped, not rejected. |

A page's `next` URL encodes the filter set of the request that produced it and MUST continue that same filtered sequence.

### 6.3 The Conformance Ladder

This specification describes the target state — a space whose activities federate natively over [ActivityPub](https://www.w3.org/TR/activitypub/) — and measures implementations against two levels:

**Level 1 — Publisher.** The space emits conformant Civic Activities through a single emission path, validates them at that path, and serves them from the collection endpoint of [§6.1](#61-the-activity-collection) with the serving rule of [§5.2](#52-the-serving-rule). Interoperation is by pull: any consumer polls the collection. *This is what the reference implementation ships.*

**Level 2 — Federated.** Level 1, plus ActivityPub delivery: the space publishes an actor document (its AP actor is the space, keyed to the space DID, with the [§6.1](#61-the-activity-collection) collection as its `outbox`), supports WebFinger discovery, accepts follows at an `inbox` (verifying inbound HTTP signatures), and delivers new public activities to followers' inboxes with retry. Restricted activities are delivered only to audience members' inboxes, honoring [§5.2](#52-the-serving-rule)'s logic in push form. The exact signature and integrity-proof profile is deliberately not pinned in v0.2 (Open Question 3, [§11](#11-open-questions)); implementations at Level 2 MUST interoperate with contemporary fediverse practice and MUST state which profile they implement.

A conformance claim names its level. Levels are cumulative, and Level 1 output is already valid AS2 — Level 2 adds delivery, never a format change.

---

## 7. Emitter Requirements

A conformant emitting space MUST:

1. Emit every activity through a **single emission path** — one chokepoint through which every activity flows, so no observable state change bypasses the stream.
2. Validate at that path: the [§2.2](#22-property-requirements-and-formats) property requirements, types per [§3](#3-the-civic-type-vocabulary) (declared extension types only), addressing set from the process's visibility policy ([§5.1](#51-addressing)), and payloads within the process's disclosure policy ([§5.3](#53-disclosure-constrains-payloads)).
3. Serve the [§6.1](#61-the-activity-collection) collection with the [§5.2](#52-the-serving-rule) serving rule.
4. Keep `id` stable: the same logical activity is never re-emitted under a new `id` (retries MUST reuse the id), so consumers can rely on `id` for deduplication.

---

## 8. Consumer Requirements

A consumer is anything that reads the stream — a feed, an indexer, a notification service, another space. A conformant consumer MUST:

1. **Tolerate the unknown.** An unrecognized type or property is a normal condition, not an error: skip it, display it generically, or store it unprocessed — never reject the page or drop its other items. This is what lets the vocabulary grow ([§3.4](#34-extensions)) without breaking deployed software.
2. **Preserve on relay.** A consumer that re-serves, forwards, caches, or exports activities passes them through intact — no narrowed copies containing only the properties it understood.
3. **Never widen the audience.** A relaying consumer applies [§5.2](#52-the-serving-rule) to its own callers and MUST NOT serve a restricted activity to a caller outside the activity's audience. Preserving the payload and preserving the audience are separate obligations; neither licenses breaking the other.

---

## 9. Out of Scope (v0.2)

- **Activity signing** (signatures by the emitting space's DID) — required before relayed or migrated history can be independently verified; gated on the export canonicalization profile and the DID-method decision (see Horizons).
- **Cross-space propagation guarantees** — Level 2 delivers to followers; multi-hop relay semantics beyond [§8](#8-consumer-requirements)'s rules are unspecified.
- **Ranking, recommendation, or engagement mechanics** — deliberately and permanently out of scope for the protocol layer.

---

## 10. Future Extensions

- Activity signing, then verifiable relayed/federated history (the dependency chain is mapped in [Civic.Social Horizons](../canon/phasing.md)).
- Pinning the Level 2 signature and integrity-proof profile (Open Question 3).
- Credential-scoped audiences — addressing collections whose membership is defined by verifiable credentials rather than space-local lists.
- **AT Protocol bridge** — an intended later target, pending evaluation: the same civic semantics expressed as a civic Lexicon set over atproto, with the space's repo as publisher. Requires no change to this profile; it is a second serialization of the same vocabulary.

---

## 11. Open Questions

*This section is non-normative.*

1. **The civic namespace home.** This document uses `https://civic.social/ns/civic` provisionally. Whether the civic vocabulary ultimately lives under the initiative's domain or a Mosaic Foundation namespace must be decided **before the context is published**, because a published context IRI is the identifier least amenable to later change — and the same decision governs the AT Protocol Lexicon namespace (domain-anchored: `civic.social` → `social.civic.*`).
2. **Single types versus type arrays.** This profile serializes a single `type` string for maximum compatibility with deployed fediverse software, at the cost of AS2's dual-typing expressiveness (`["Create", "civic:BallotCast"]`). If array handling in the wild improves, or implementations report needing richer typing, this choice should be revisited.
3. **The Level 2 delivery profile.** Which HTTP signature scheme and object-integrity proofs to require (the fediverse is mid-transition between legacy and standardized profiles). Deliberately unpinned in v0.2; must be pinned before two independent Level 2 implementations are expected to interoperate.
4. **Non-geographic grouping.** `civic:code` is geographic by design, and the emitting community is identified by `generator.id` — but there is no way yet to mark activities as belonging to a grouping that spans spaces (an issue campaign across twenty hubs, a coalition). AS2's `audience` and `tag` properties are candidate homes; encoding campaign names into `civic:code` is the misuse this question exists to prevent.

---

## Appendix A: Why a Profile

*Non-normative.*

This specification could not be pure ActivityStreams, and should not be a separate format. The reasoning, stated once so no reviewer has to reconstruct it:

**Why not a standalone civic wire format** (the v0.1 approach): owning a wire format means re-making every design choice AS2 already made — naming style, property shapes, addressing, paging — and every re-make is a place to diverge from an ecosystem that already interoperates. The v0.1 envelope was honest engineering (it matched shipping code, and the civic context did not yet exist), but each of its divergences was a cost with no compensating benefit. Retiring it while the ecosystem had zero external consumers was the cheapest moment that will ever exist.

**Why not pure AS2 with no civic terms**: expressed in bare AS2, a ballot in a county advisory vote and a social media reply are the same object — `Create` + a note-like thing. The civic layer's entire value is that a consumer can distinguish them machine-readably: this object is a ballot, in this process, under this disclosure policy, in this place. Strip the civic terms and nothing is gained in compatibility (AS2 consumers ignore unknown terms by design) while everything is lost in meaning.

**What each civic term adds:** `civic:Process` and `civic:processType` — the unit of structured participation, which AS2's `context` can point at but not describe; `civic:Frame` / `civic:Start` / `civic:End` — lifecycle transitions with no AS2 verb (the only three activity types this profile mints); `civic:Ballot`, `civic:Proposal`, `civic:Result`, `civic:Outcome`, `civic:Feedback` — the object classes of civic participation, each defined in [§3.2](#32-type-registry); `civic:code` — filterable, hierarchical civic geography that a plain `Place` lacks; `civic:terminalState`, `civic:method` — the two properties consumers need inline rather than behind a dereference. Everything else — identity of actors, time, audience, attribution, paging, migration — AS2 already had, and this profile uses it unmodified.

Extension contexts are AS2's own intended extension mechanism; the civic vocabulary extends the fediverse the way the fediverse is designed to be extended.

## Appendix B: Mapping from the v0.1 Envelope

*Non-normative. For readers of v0.1 or code written against it.*

| v0.1 | v0.2 |
|---|---|
| `id` (bare UUID) | `id` (`urn:uuid:` IRI) |
| `version` | the `@context` (versioned by IRI) |
| `event_type` (e.g. `civic.process.vote_submitted`) | `type` + typed `object` per [§3.2](#32-type-registry) (e.g. `Create` + `civic:Ballot`) |
| `timestamp` | `published` |
| `process_id` | `context` (IRI or `civic:Process` object) |
| `actor` (prefix conventions: `did:`, `system:`, opaque) | `actor` (IRIs: DID, space-scoped user IRI, space-scoped system IRI) |
| `jurisdiction` (string; `"none"` sentinel) | `location` (`Place` + `civic:code`); absent where no place applies |
| `action_url` | `url` |
| `source.hub_id` / `source.hub_url` / `source.space_id` | `generator` object (`id` = space DID, `url` = serving location) |
| `data.process.type` | `context` → `civic:processType` |
| `data.<subject>` | typed `object` |
| `meta.visibility: "public"` | audience includes `as:Public` |
| `meta.visibility: "restricted"` | limited addressing |
| `dedupe_key` | removed — `id` is stable across retries ([§7](#7-emitter-requirements), rule 4) |
| `civic.space.migrated` | `Move` ([§3.3](#33-space-migration)) |
| Feed envelope `{items, next_cursor}` | `OrderedCollection` / `OrderedCollectionPage` ([§6.1](#61-the-activity-collection)) |
| Planned `event_type` → `activity_type` rename | cancelled — superseded by AS2 `type` |

The v0.1 envelope is retired without a compatibility window: the reference implementation converted directly, and no external consumers existed. Its full text remains in this repository's git history.

---

## References

**Normative.**

- **ActivityStreams 2.0** — W3C Recommendation. Core: https://www.w3.org/TR/activitystreams-core/ · Vocabulary: https://www.w3.org/TR/activitystreams-vocabulary/
- **ActivityPub** — W3C Recommendation (Level 2 conformance). https://www.w3.org/TR/activitypub/
- **JSON-LD 1.1** — W3C Recommendation. https://www.w3.org/TR/json-ld11/
- **The civic JSON-LD context** — [`civic.jsonld`](./civic.jsonld), published at `https://civic.social/ns/civic` (provisional; Open Question 1).
- **RFC 2119** and **RFC 8174** (BCP 14) — requirement keywords. https://www.rfc-editor.org/rfc/rfc2119 · https://www.rfc-editor.org/rfc/rfc8174
- **RFC 3339** — timestamps (`published`). https://www.rfc-editor.org/rfc/rfc3339
- **RFC 9562** — UUIDs, including UUIDv7 ([§2.2](#22-property-requirements-and-formats)). https://www.rfc-editor.org/rfc/rfc9562

**Informative.**

- **Companion specifications** — the [Civic Space](./civic-space-spec.md), [Civic Process](./civic-process-spec.md), and [Civic Identity](./civic-identity-spec.md) Specifications; the [Civic Plugin Architecture](../ecosystem/civic-plugin-architecture-spec.md); the [Discovery Layer Specification](../ecosystem/discovery-layer-spec.md); and **[Civic.Social Horizons](../canon/phasing.md)**. These are being aligned to this revision; where an older companion contradicts this document, this document governs for the wire.
- **AT Protocol** — https://atproto.com/ — the intended later bridge target ([§10](#10-future-extensions)).

---

*Version 0.2 · Draft · Last updated 2026-08-20 · Civic.Social — civic.social | contact@civic.social*
