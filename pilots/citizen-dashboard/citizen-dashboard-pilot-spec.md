---
status: draft
last-reviewed: 2026-07-03
owners: [adam]
version: 0.1
---

# Citizen Dashboard Pilot

Civic.Social Infrastructure Program

> **Status and governance.** This specification is a **working draft published for community discussion** — not a final standard. The Citizen Dashboard Pilot builds against the four canonical Civic.Social specifications (Civic Space · Civic Process · Civic Activity · Civic Identity), and its central deliverable — the individual-scope compliance profile of the Civic Space Specification — will be developed in the open alongside those documents. Breaking changes between pre-1.0 versions are expected and welcomed.

> **Conformance phasing.** Specifications in this ecosystem define the *target state*; reference implementations converge on them incrementally through the pilot program. Where this document says the dashboard "MUST" do something, that is the conformance bar the pilot builds toward — not a claim about what the current prototype does today. Section 17 states the current implementation reality plainly.

---

## Table of Contents

### [How to Read This Document](#how-to-read-this-document-1)

### Executive Overview
1. [Executive Summary](#1-executive-summary)
2. [Purpose of the Pilot](#2-purpose-of-the-pilot)
3. [Relationship to the Civic.Social Pilot Program](#3-relationship-to-the-civicsocial-pilot-program)
4. [Strategic Importance](#4-strategic-importance)

### Strategic Context
5. [What is the Citizen Dashboard](#5-what-is-the-citizen-dashboard)
6. [The Dashboard as an Individual-Scoped Civic Space](#6-the-dashboard-as-an-individual-scoped-civic-space)
7. [Personal Data, Not Community Data](#7-personal-data-not-community-data)
8. [One Engine, Many Lenses — Assembly from Shared Components](#8-one-engine-many-lenses--assembly-from-shared-components)
9. [The Ecosystem Browser Role](#9-the-ecosystem-browser-role)
10. [Neutrality Commitments](#10-neutrality-commitments)

### Dashboard Architecture
11. [The Individual-Scope Compliance Profile](#11-the-individual-scope-compliance-profile)
12. [Consuming the Ecosystem — Feeds, Manifests, Descriptors](#12-consuming-the-ecosystem--feeds-manifests-descriptors)
13. [Process Participation — Deep-Link and Embedded Flows](#13-process-participation--deep-link-and-embedded-flows)
14. [Subscriptions and the Minimal PDS Profile](#14-subscriptions-and-the-minimal-pds-profile)
15. [The Citizen Console — Identity Surface](#15-the-citizen-console--identity-surface)
16. [Civic Context Widgets and External Data](#16-civic-context-widgets-and-external-data)

### Pilot Implementation
17. [Current Implementation Reality](#17-current-implementation-reality)
18. [Minimum Viable Pilot Scope](#18-minimum-viable-pilot-scope)
19. [Pilot Phases and Timeline](#19-pilot-phases-and-timeline)
20. [Pilot Demonstration Scenarios](#20-pilot-demonstration-scenarios)
21. [Success and Validation Criteria](#21-success-and-validation-criteria)
22. [Expected Deliverables](#22-expected-deliverables)

### Ecosystem and Partnerships
23. [Relationship to Other Civic.Social Pilots](#23-relationship-to-other-civicsocial-pilots)
24. [Estimated Development Effort and Budget](#24-estimated-development-effort-and-budget)

### Pilot Plan
25. [Risks and Mitigations](#25-risks-and-mitigations)
26. [Out of Scope](#26-out-of-scope)
27. [Open Questions for Further Design](#27-open-questions-for-further-design)

### Conclusion
28. [Conclusion](#28-conclusion)

---

<a id="how-to-read-this-document-1"></a>
## How to Read This Document

This document is the canonical specification for the Citizen Dashboard Pilot.

**Funders and program evaluators** should focus on the Executive Overview (sections 1–4), the Success and Validation Criteria (section 21), and the Pilot Plan (sections 24–27). These sections explain why a unified citizen interface matters, how the pilot will be measured, and what it will cost.

**Technical implementers** should focus on the Dashboard Architecture (sections 11–16), the Current Implementation Reality (section 17), and the Expected Deliverables (section 22). These define what the dashboard must consume, what it must store, and where the conformance seams are.

**Ecosystem partners** — space engine builders, civic data providers, and the other Civic.Social pilots — should focus on the Strategic Context (sections 5–10) and the Relationship to Other Civic.Social Pilots (section 23).

This pilot specification refers throughout to four companion documents:

- **[Civic Space Specification](../../ecosystem/civic-space-spec.md)** — the scoped host contract every space conforms to. The dashboard is the individual-scoped space; §1.3–1.6 (scope taxonomy), §7 (Space API Profile), and §9.4 (individual portability profile) are the sections this pilot exercises most directly.
- **[Civic Activity Specification](../../ecosystem/civic-activity-spec.md)** — the activity envelope and stream the dashboard consumes. For v0.1 the wire-format field is `event_type` and the transport endpoint is `GET /events`.
- **[Civic Process Specification](../../ecosystem/civic-process-spec.md)** — the process descriptors and action contracts the dashboard participates against.
- **[Discovery Layer Specification](../../ecosystem/discovery-layer-spec.md)** — the manifest, indexer, and re-binding protocol the dashboard's discovery surface is built on.

The pilot does not redefine what those documents already cover. It consumes them, validates them from the individual's side of the ecosystem, and surfaces what they need to make stronger.

---

<a id="1-executive-summary"></a>
## 1. Executive Summary

The Citizen Dashboard Pilot will design, build, and validate the **reference Citizen Dashboard**: one unified civic view for the individual, across hubs, spaces, and issues. The dashboard is where a citizen sees the civic life of every community they belong to, discovers spaces and processes they did not know existed, and participates — voting here, commenting there — with a single identity, without leaving one interface.

Architecturally, the Citizen Dashboard is the **individual-scoped Civic Space** (Civic Space Specification §1.4): a space anchored to the citizen's own Sovereign Foundation — the Citizen Node and Personal Data Store — rather than to a community's or an entity's. It is the first non-hub space type built against the generalized Civic Space contract, which makes this pilot the practical test of a specific architectural claim: that the Space primitive actually generalizes beyond the community-scoped Civic Hub. If the same identity, activity, discovery, and portability contracts serve a personal interface as naturally as they serve a community hub, the generalization holds. If they do not, this pilot is where the friction becomes visible — early, cheaply, and in the open.

In the five-layer Civic.Social architecture (Open Web Standards → Civic Specifications → Sovereign Foundation → Components → Interfaces), the dashboard lives at the **Interfaces** layer and is deliberately assembled from the shared **Components** beneath it: the Activity Feed engine refracted into inbox, notification, and discovery lenses; the Process runtime for participating in processes hosted by other spaces; the Citizen Console rendering the Citizen Node; the PDS surface rendering what the citizen follows and cares about; the Identity Adapter; and Access Control. One engine, many lenses — the dashboard invents no new infrastructure; it proves the infrastructure composes.

The pilot productizes an existing UI prototype against real APIs: reading `GET /events` feeds, discovery manifests, and process descriptors from live spaces (the reference Civic Hub and a Representative Space), and acting through `POST /process/:id/action` with a single portable identity. It also names, and resolves, the ecosystem's most conspicuous unbuilt dependency: the Personal Data Store. Subscriptions and preferences belong in the PDS, and no pilot currently builds one. This pilot proposes a **minimal PDS profile** — subscriptions and preferences only — as an explicit coordination deliverable co-owned with the Civic Identity Pilot (section 14).

The primary public artifacts are the reference dashboard application itself and the **individual-scope compliance profile** of the Civic Space Specification — the first worked answer to what Space conformance means for a space that consumes far more than it hosts.

---

<a id="2-purpose-of-the-pilot"></a>
## 2. Purpose of the Pilot

The Citizen Dashboard Pilot exists to prove three things.

**First, that the Interface layer works.** The Civic.Social architecture claims that identity, activity, process, and discovery infrastructure can be composed into interfaces people actually use. The dashboard is the citizen-facing interface of the ecosystem — the place where the infrastructure either becomes a coherent experience or is exposed as a pile of endpoints. The pilot builds that experience against live spaces and measures whether one person can conduct real cross-space civic life from it.

**Second, that the Civic Space primitive generalizes.** The Civic Space Specification v0.2 generalized the host contract from the Civic Hub to a scoped primitive with an open type set. The dashboard is the first space built at a scope other than `community`. Every place where the Space contract fits the individual scope cleanly is evidence the generalization was right; every place it has to be bent is a finding the pilot feeds back into the specification.

**Third, that individual portability is real.** The Civic Space Specification's individual portability profile (§9.4) promises that a citizen's foundation is continuously portable: switch interfaces, keep your civic life. The pilot makes that testable — export the PDS from one dashboard implementation, import it into another, and verify nothing about the citizen's subscriptions, preferences, or participation pointers is lost.

The pilot is explicitly *not* a civic data product. Election information, representative profiles, and legislative tracking are valuable context surfaces (section 16), but they are additive widgets, not the pilot's thesis. The thesis is the protocol-conformant personal space.

---

<a id="3-relationship-to-the-civicsocial-pilot-program"></a>
## 3. Relationship to the Civic.Social Pilot Program

The Citizen Dashboard Pilot is one component of the broader Civic.Social Infrastructure Program, and it is the most *downstream* of the pilots — it consumes what the others produce.

**Civic Identity Pilot** — provides authentication, the Citizen Node, and the identity wallet. The dashboard's Citizen Console renders what this pilot builds, and the minimal PDS profile is co-owned between the two pilots (section 14). This is the dashboard's deepest dependency.

**Civic Activity Feed & Discovery Pilot** — provides the feed aggregation engine and the reference indexer. The dashboard is the primary consumer of both: its activity view is a lens over the feed engine, and its discovery surface queries the indexer.

**Civic Hubs Pilot** — provides the publishers. Hubs emit the activities, publish the manifests, and host the processes the dashboard surfaces.

**Civic Process Plugin Pilot** — defines the process descriptors and action contracts the dashboard's participation flows are written against, and the plugin surfaces ("a module inside a Citizen Dashboard") that embedded participation depends on.

No pilot is a hard prerequisite for another; each dependency in this document names its interim fallback. But the preferred sequencing puts the dashboard after — or overlapping the tail of — the Identity and Activity Feed pilots, because a dashboard against stubbed identity and hand-rolled feed polling proves less than a dashboard against the real layers.

---

<a id="4-strategic-importance"></a>
## 4. Strategic Importance

The fragmentation problem the Civic.Social program addresses has a citizen-shaped symptom: even where good civic tools exist, each one is a separate destination with a separate account and a separate notion of who you are. A resident of one county who follows their board of supervisors, a regional housing coalition, and their state delegate's office is asked to maintain three logins, check three sites, and rebuild their preferences three times. Most people, reasonably, do none of it.

Every other layer of the ecosystem exists to fix this, but only an interface makes the fix *visible*. Shared identity means nothing to a citizen until one login works everywhere. A shared activity stream means nothing until the updates from every space they care about arrive in one place. Discovery means nothing until a person finds a participation opportunity they were not looking for. The dashboard is where the ecosystem's value proposition becomes experienceable — and therefore fundable, adoptable, and demonstrable.

There is also a defensive reason this pilot matters. If the ecosystem does not define the personal civic interface as an open, protocol-conformant, replaceable application, someone will eventually define it as a platform — a single proprietary app that owns the citizen relationship and reduces every hub and space to a content source. The dashboard pilot establishes the opposite pattern while the pattern is still cheap to establish: the personal interface is a *space the citizen holds*, portable across implementations, with its data in the citizen's own foundation. The individual-scope compliance profile is the artifact that makes "replaceable dashboard" a testable property rather than a slogan.

---

<a id="5-what-is-the-citizen-dashboard"></a>
## 5. What is the Citizen Dashboard

The **Citizen Dashboard** is an individual-scoped Civic Space: a personal civic interface where a citizen sees cross-space civic activity, manages what they follow, discovers spaces and processes, understands their civic context, and takes civic action — all with one identity.

The framing that has guided its design from the earliest architecture work: the dashboard is **the citizen's ecosystem browser**. A web browser does not host websites; it renders them, remembers the ones you visit, and gives you one place from which to reach everything. The dashboard stands in the same relation to the civic ecosystem. It does not host communities, moderate discussions, or run governance — those belong to community-scoped hubs. It renders the ecosystem *from one person's point of view*: their subscriptions, their jurisdictions, their pending participation opportunities, their history.

This is also why the dashboard is deliberately not a discussion environment. Deliberation, with all the moderation weight it carries, lives in hubs; the dashboard carries information, alerts, and lightweight actions, and hands the citizen off (or embeds the process surface) when deeper participation begins. The separation keeps the dashboard neutral, light, and personal.

Concretely, the pilot dashboard presents three categories of content, following the prototype's three-column shape:

- **My spaces** — the Civic Spaces the citizen follows or belongs to (hubs, representative spaces), sourced from PDS subscriptions.
- **Activity** — the cross-space Civic Activity feed, aggregated across everything followed, refracted into inbox, notification, and discovery lenses.
- **Context and actions** — civic context widgets (jurisdictions, representatives, elections) and direct participation entry points (vote, comment, contact, RSVP).

---

<a id="6-the-dashboard-as-an-individual-scoped-civic-space"></a>
## 6. The Dashboard as an Individual-Scoped Civic Space

The Civic Space Specification defines a space by six obligations (§1.3): anchor a sovereign foundation, host processes through the plugin contract, emit and consume activities through a single path, publish a discovery manifest, mediate participation through the identity adapter and a single authorization seam, and satisfy the portability contract. Every space declares exactly one scope; the scope determines the sovereign anchor, the relationship model, and the portability profile (§1.4).

The dashboard's declarations:

| Contract element | Individual-scope binding |
|---|---|
| Scope | `individual` |
| Space type | `civic.dashboard` |
| Sovereign anchor | **Citizen Node** (the citizen's DID + credentials) |
| Data store | **Personal Data Store** (subscriptions, preferences, participation pointers) |
| Relationship model | Owner + followers — no membership rolls (Civic Space Specification §4.5) |
| Portability profile | Individual profile (§9.4): continuous, pointer-based |

Two features of the specification make the individual scope legitimate rather than a stretch. First, §1.3 states that a space which "primarily *surfaces* processes hosted elsewhere" can be fully conformant — hosting a rich process catalog is not required. That clause describes the dashboard exactly. Second, §1.4 makes the type set open by construction: a new space type registers its scope, anchor, relationship model, and portability profile, and inherits everything else unchanged, requiring **no changes** to the specification. The dashboard is the first exercise of that registration path, and completing it cleanly — or documenting precisely where it could not be completed cleanly — is itself a pilot deliverable (section 11).

The dashboard is also where a boundary in the taxonomy earns its keep: the **Citizen Account Provider is not a space**. The provider is an infrastructure role (§1.5) that hosts the citizen's foundation — Citizen Node and PDS — on their behalf, federated like an email provider. The dashboard is the *interface over* that foundation, not the custodian of it. A citizen can switch dashboard implementations without switching providers, and switch providers without losing what any dashboard shows them. Keeping the two concepts separate is what makes both migrations possible, and the pilot's demonstration scenarios exercise the first of them directly.

---

<a id="7-personal-data-not-community-data"></a>
## 7. Personal Data, Not Community Data

The single most important data rule in this pilot, inherited from the individual portability profile (Civic Space Specification §9.4): the dashboard holds **personal** data, never copies of community content.

What the dashboard's foundation stores:

- **Subscriptions** — which spaces the citizen follows, keyed by space DID (URLs are resolvable attributes, not identity).
- **Preferences** — notification settings, jurisdiction context, display and accessibility options.
- **Participation pointers** — references to processes the citizen has engaged with: process id, hosting space DID, action taken, timestamp, `action_url`. Pointers, not transcripts.

What it never stores: the substance of participation. When a citizen votes in a hub's advisory vote or comments in a representative space's consultation, the ballot and the comment belong to the process that hosted them, governed by that process's disclosure policy and that space's portability profile. The citizen's own export contains *pointers into other spaces, not their content*. This is not a storage optimization; it is the property that keeps the individual profile clean. A dashboard export can be continuous, small, and privacy-safe precisely because it carries references rather than a shadow copy of every community it touches — and no dashboard implementation can quietly become a data aggregator holding other communities' records.

Two consequences follow. Rendering the citizen's participation history requires *dereferencing* pointers against the hosting spaces' read endpoints, which means history display degrades gracefully (pointer metadata remains) when a source space is unreachable — a behavior the pilot must design deliberately rather than discover accidentally. And the snapshot-migration machinery that dominates the community portability profile applies only weakly here: the individual foundation is *always* portable, by construction, which is why the pilot's portability test (section 21) is a live export/import rather than a scheduled migration window.

---

<a id="8-one-engine-many-lenses--assembly-from-shared-components"></a>
## 8. One Engine, Many Lenses — Assembly from Shared Components

The dashboard is an assembly, not an invention. Each of its surfaces is a shared Component from the fourth architecture layer, configured for the individual scope:

- **Activity Feed engine** — the same feed machinery every space uses, here consuming *many* feeds (one per subscription) and merging them into one stream. The dashboard refracts that stream into three lenses: an **inbox** lens (everything from followed spaces, chronological), a **notifications** lens (items that reference the citizen or a process they participate in), and a **discovery** lens (activity from beyond the subscription set, supplied by an indexer). One engine, three views — no separate feed systems.
- **Process runtime** — the participation machinery that lets the citizen act on a process *hosted by another space* without leaving the dashboard, per the process descriptor's declared endpoints (section 13).
- **Citizen Console** — the rendering of the Citizen Node: who I am, my DIDs, my credentials, what I am eligible to do (section 15).
- **PDS surface** — the rendering of the Personal Data Store: what I follow, what I care about, my preferences, my participation pointers (section 14).
- **Identity Adapter** — the replaceable seam through which authentication flows: stub assurance in early phases, DID authentication later, with no rebuild in between (Civic Space Specification §7.3).
- **Access Control** — the single authorization seam. Thin at individual scope (one owner, no members), but present, so the policy engine can evolve without rewriting interfaces.

The assembly discipline is the point. If the dashboard needed a bespoke feed system, a bespoke participation flow, or a bespoke identity model, the Components layer would be a diagram rather than an architecture. Building the dashboard *only* from shared components is how the pilot validates that layer — and everything it hardens (multi-feed consumption, lens configuration, cross-space action) is inherited for free by the next space type built on the same parts.

---

<a id="9-the-ecosystem-browser-role"></a>
## 9. The Ecosystem Browser Role

Four capabilities together constitute the browser role. Each maps to a specific protocol surface — the dashboard adds no private channels.

**See.** The cross-space activity view: for every subscribed space, resolve the space DID to its serving URL, read its manifest, poll its `GET /events` feed, merge and render. One place where the civic life of every followed community is visible.

**Discover.** Query one or more discovery indexers (Discovery Layer Specification) for spaces and processes by jurisdiction, type, and category; browse and search beyond the subscription set. The discovery model is deliberately pluralistic — multiple indexers can exist, and the dashboard treats the indexer as a configurable choice, not a hardwired authority.

**Follow.** Subscribe to a space with one action. The subscription is written to the PDS — not to the dashboard's own database and not to the followed space — which is what makes it portable across dashboard implementations and legible to any application the citizen authorizes.

**Act.** Participate in a process hosted anywhere in the ecosystem, with one identity, via the process descriptor's action contract (section 13). The act is recorded where it belongs (the hosting space emits the activity); the dashboard keeps only its pointer.

Everything the browser role needs already exists in the four canonical specifications. That is the quiet claim this pilot tests: a complete personal civic interface can be built from public contracts alone, which means anyone else can build a competing one — and the citizen can leave ours for theirs without losing anything.

---

<a id="10-neutrality-commitments"></a>
## 10. Neutrality Commitments

An interface that aggregates everything a citizen sees about civic life is a potential gatekeeper, and the failure mode is well documented in consumer software: engagement-ranked feeds, opaque recommendations, and a private subscription graph that makes leaving expensive. The reference dashboard makes the opposite commitments, and makes them testable:

1. **No engagement ranking.** The default activity view is chronological, with geographic filtering — the Discovery Layer's MVP ordering. No engagement-optimizing algorithm is introduced in this pilot, and any future ranking (Discovery Layer Phases 2–3) must be user-selectable and transparent, never a silent default.
2. **No hidden curation.** Every item in the inbox lens is traceable to a subscription the citizen made and an activity a space emitted. The dashboard does not suppress, boost, or interleave sponsored content.
3. **No captive graph.** Subscriptions live in the PDS, exportable at any time, importable by any conformant implementation. The dashboard holds no relationship data hostage.
4. **No indexer monopoly.** Discovery is served by configurable indexers; the reference index is a default, not a chokepoint.
5. **Open reference implementation.** The dashboard ships as open source, so the neutrality claims are inspectable rather than promised.

These commitments are recorded here as pilot requirements, and the success criteria (section 21) include verifying them against the shipped implementation.

---

<a id="11-the-individual-scope-compliance-profile"></a>
## 11. The Individual-Scope Compliance Profile

The Space API Profile (Civic Space Specification §7) was written from the community scope outward, and it mostly transfers — but not uniformly, because the dashboard **consumes more than it hosts**. Defining exactly how it transfers is a named deliverable of this pilot: the **individual-scope compliance profile**, published as a companion to the Civic Space Specification.

The pilot's working position, to be validated and refined:

| Space API element (§7) | Individual scope | Rationale |
|---|---|---|
| `GET /.well-known/civic.json` | **Conditional** | A publicly addressable dashboard SHOULD publish a manifest (scope `individual`, type `civic.dashboard`). A private, client-side, or provider-hosted personal dashboard need not be publicly discoverable at all — privacy is the default for personal spaces. The profile must define what conformance means for a space with no public serving URL (section 27). |
| `POST /process` | **Optional** | The dashboard primarily surfaces processes hosted elsewhere (§1.3 allows this explicitly). If a dashboard later hosts personal-scope processes, the endpoint applies as written. |
| `GET /process/:id` | **Optional** | Applies only to processes the dashboard itself hosts. |
| `POST /process/:id/action` | **Optional as server; required as client.** | The dashboard's defining behavior is *calling* this endpoint on other spaces, correctly: actor from authenticated context, eligibility respected, errors surfaced. The profile specifies client-side conformance — a novel notion the community profile never needed. |
| `GET /events` | **Optional as server; required as client.** | The dashboard consumes many feeds. If it emits activities at all (e.g., for personally hosted processes), they flow through a single emission path per the Civic Activity Specification; personal subscription changes are PDS writes, not public activities, by default. |
| Identity adapter (§7.3) | **Required** | Identical to community scope: replaceable adapter, declared assurance level, DID-ready. |
| Single authorization seam (§4.7) | **Required** | Thin (owner + followers) but present. |
| Portability contract (§9) | **Required — individual profile (§9.4)** | Continuous pointer-based export; identity/credential export at the wallet layer, never through the space export. |

The general shape: the individual scope inverts the API profile's emphasis from *serving* to *consuming*, and the compliance profile's contribution is to make consumption conformance-testable — which feeds the dashboard must poll, how it must key provenance (on `source.space_id` where present, per the Civic Activity Specification), how it must honor `meta.visibility`, and how it must follow the migration re-binding protocol when a followed space moves (Discovery Layer §7.4).

---

<a id="12-consuming-the-ecosystem--feeds-manifests-descriptors"></a>
## 12. Consuming the Ecosystem — Feeds, Manifests, Descriptors

The dashboard reads three kinds of public documents, and nothing else, to build its entire view of the ecosystem:

**Discovery manifests** (`GET /.well-known/civic.json`). For each subscribed or discovered space: name, space identity (`space.id` DID, scope, type), jurisdictions, feed URLs, enabled processes. The dashboard accepts both the current form and the legacy top-level `type: "hub"` form for the life of v0.1.

**Activity feeds** (`GET /events`). The v0.1 wire format as ratified: JSON envelope with `event_type`, `timestamp`, `process_id`, `actor`, `jurisdiction`, `action_url`, `source`, `data`, `meta.visibility`. The dashboard merges feeds across subscriptions, orders by timestamp, classifies by `event_type` and `data.process.type`, keys provenance on `source.space_id` where present (falling back to `hub_url`), and derives display fields per the Discovery Layer's presentation guidance. It treats `action_url` as the canonical bridge from *seeing* to *acting*.

**Process descriptors** (`GET /process/:id`). When an activity references a process the citizen can act on, the dashboard fetches the descriptor — type, title, status, lifecycle profile, actions with input contracts, credential requirements, endpoints — and renders the appropriate participation affordance (section 13).

In pilot phase 1, feed consumption is direct polling of each subscribed space — honest, simple, and sufficient for a handful of subscriptions. As the Activity Feed & Discovery pilot's aggregation engine matures, the dashboard swaps direct polling for engine-mediated consumption behind the same internal interface, so the lenses never know the difference. Both modes are legitimate; the interface between them is designed so the swap is a configuration change, not a rewrite.

---

<a id="13-process-participation--deep-link-and-embedded-flows"></a>
## 13. Process Participation — Deep-Link and Embedded Flows

Participation is the dashboard's highest-stakes flow: it is where a personal interface takes an action *inside another space's process*, and where the ecosystem's one-identity promise is either kept or broken.

The dashboard supports two participation modes, selected per process from its descriptor:

**Deep-link participation.** The default and the floor. The dashboard follows the descriptor's `endpoints.view` (or the activity's `action_url`) into the hosting space's own interface, carrying the citizen's identity context per the Civic Identity pilot's session/handoff model. The hosting space runs the interaction; the dashboard records a participation pointer when the corresponding activity appears in the space's feed. Deep-linking works for every process type, including Tier 3 external-service plugins like Polis whose interfaces cannot be embedded.

**Embedded participation.** Where the process descriptor declares an embeddable surface (per the Civic Process Plugin framework's "module inside a Citizen Dashboard" surface), the dashboard renders the participation panel inline — an advisory vote's options and submit button, a consultation's comment box — and submits via `POST /process/:id/action` against the hosting space, with the actor taken from the authenticated context, never from the request body. The activity the hosting space emits is the receipt; the dashboard's pointer references it.

In both modes the invariants are the same: the hosting space owns the process state and emits the activities; the citizen acts as one identity everywhere; the dashboard holds pointers, not content. The pilot deliberately exercises both modes across two live spaces — an embedded `civic.vote` participation in the reference hub and a comment flow in the representative space — because the two-mode contract is exactly the seam the Civic Process Plugin pilot needs validated from the host side.

---

<a id="14-subscriptions-and-the-minimal-pds-profile"></a>
## 14. Subscriptions and the Minimal PDS Profile

Here the pilot must be blunt about a dependency: **subscriptions and preferences belong in the Personal Data Store, and no pilot currently builds one.** The Civic Identity Pilot defines the PDS architecturally — its scope, its layer separation, its exclusions — and explicitly defers implementation to a subsequent phase. The Citizen Dashboard cannot defer it: a dashboard whose subscriptions live in its own database is, by this program's own definitions, just another platform.

The proposed resolution, which this pilot carries as an explicit coordination deliverable:

**A minimal PDS profile, co-owned by the Citizen Dashboard and Civic Identity pilots.** Scope: **subscriptions and preferences only** (plus the participation pointers of section 7, which are structurally subscriptions' siblings). The profile defines:

- the record shapes: subscription (space DID, subscribed-at, notification preferences), preference (namespaced key/value with declared schema), participation pointer (process id, space DID, action, timestamp, `action_url`);
- a minimal read/write API the dashboard consumes and the identity layer serves, with writes authorized by the citizen;
- the export/import format — deterministic, versioned, and small — that makes the portability test of section 21 executable;
- what it deliberately excludes, inheriting the Identity pilot's boundaries: no content, no feeds, no application state.

Co-ownership is the mechanism, not a courtesy: the Identity pilot owns where the PDS lives and how access is authorized; the Dashboard pilot owns the record shapes it needs and is the first real consumer that keeps the profile honest. Neither pilot can ship the profile alone, and the joint deliverable is named in both pilots' plans.

**Interim scaffolding, clearly marked.** Until the minimal PDS service exists, the reference dashboard persists subscriptions and preferences in browser local storage behind the *same internal PDS interface* it will later point at the real service. This interim is **non-conformant scaffolding and is labeled as such** — in the code, in the UI's about screen, and in every demonstration. It exists so dashboard development is not serialized behind the identity work; it is not a portability story, and the pilot does not claim conformance until the PDS-backed path passes the export/import test. The internal interface is the enforcement mechanism: swapping local storage for the PDS service must touch one adapter, not the application.

---

<a id="15-the-citizen-console--identity-surface"></a>
## 15. The Citizen Console — Identity Surface

The Citizen Console is the dashboard's rendering of the Citizen Node: the "who I am" surface. It shows the citizen their identifier(s), their credentials, and their capabilities — what they are currently able to do in the ecosystem and at what assurance level.

The console is built **stub-assurance first, DID-ready**. In early phases the identity adapter authenticates with the reference implementation's session model, and the console displays exactly what that model can honestly claim: an opaque identifier, a declared (low) assurance level, and the participation capabilities that assurance supports. As the Civic Identity pilot delivers DID authentication and verifiable credentials, the same console renders real DIDs, real credentials (resident, organizational, and so on), and credential-gated eligibility — through the same replaceable adapter seam, with no rebuild of the dashboard (Civic Space Specification §3.1, §7.3).

Two design rules keep the console honest. First, **assurance is displayed, not implied**: a stub-authenticated citizen sees that their identity is provisional, so the pilot never demos a stronger identity story than it has. Second, **the console renders; it does not custody**: keys and credentials live in the wallet and with the Citizen Account Provider (an infrastructure role, not a space), and identity export happens at that layer — never through the dashboard's own export, per §9.4.

---

<a id="16-civic-context-widgets-and-external-data"></a>
## 16. Civic Context Widgets and External Data

The prototype and the earliest dashboard designs include civic context surfaces: My Civic Map (jurisdictions), Your Representatives, Upcoming Elections, Sample Ballot, plus contact-your-representative tooling — with candidate integrations including Ballotpedia, VoteSmart, Democracy Works/TurboVote, OpenStates, and the FEC API.

This pilot's posture: context widgets are **retained as interface surfaces but demoted from the conformance story**. They make the dashboard genuinely useful and demo well, and the guiding principle stands — aggregate existing civic data, never recreate it. But they exercise third-party REST APIs, not Civic.Social protocol, and the pilot's budget guards against them consuming it. Concretely:

- Phase scope ships context widgets against static or cached data with **one** live external integration chosen for effort/value (election information is the leading candidate);
- widget data is app-layer cache, never PDS content (the citizen's *jurisdiction preference* is PDS; the ballot data it fetches is not);
- the representative-transparency surface links toward the Representative Space where one exists — the protocol-native home for that data — rather than duplicating it;
- full multi-provider integration is future work (section 26).

---

<a id="17-current-implementation-reality"></a>
## 17. Current Implementation Reality

Stated plainly, because the pilot's credibility depends on it: **the current citizen-dashboard code is a UI prototype running entirely on mock data.** It is a React/Vite single-page application (`citizen-dashboard/` in the monorepo) with the three-column layout, feed rendering, hub pages, sample ballot, and contact-representatives surfaces — all fed from a single static `mockData.js`. It has **no API client, no identity integration, and no persistence**. It has never read a real `GET /events` feed, never fetched a manifest or process descriptor, and never submitted an action.

What the prototype is worth: the information architecture is validated, the three-column model (spaces / activity / context+actions) matches the pilot design, and the component inventory maps cleanly onto the shared-components assembly of section 8. What it is not: any part of the conformance story.

The pilot is therefore a **productization**, and its work is exactly the delta: an API client layer speaking the live contracts (manifests, `GET /events`, descriptors, `POST /process/:id/action` against the reference Civic Hub and Representative Space, both of which run today); a replaceable identity adapter; a PDS-interface persistence layer (interim local storage, then the minimal PDS); and the migration of every rendered surface from mock data to live data — with mock data ripped out rather than left as a silent fallback. Zero mock data in conformant surfaces is a stated success criterion (section 21).

---

<a id="18-minimum-viable-pilot-scope"></a>
## 18. Minimum Viable Pilot Scope

The minimum scope that proves the pilot's three claims (section 2):

1. **Live-data dashboard.** The reference dashboard renders subscriptions, a merged cross-space activity view, and process detail from at least **two live publishing spaces** — the reference Civic Hub (community scope) and a Representative Space (entity scope) — via manifests, `GET /events`, and process descriptors. No mock data on conformant surfaces.
2. **One identity, two spaces, two actions.** An authenticated citizen votes in a process hosted by one space and comments in a process hosted by the other, from the dashboard alone — at least one action embedded, at least one deep-linked.
3. **Subscriptions via minimal PDS.** Follow/unfollow writes to the minimal PDS profile (through the interim scaffold first, the PDS service before pilot completion), and the PDS export/import round-trip preserves subscriptions, preferences, and participation pointers across dashboard instances.
4. **Discovery UX.** Browse/search of spaces and processes against a real indexer (the reference index from the Activity Feed & Discovery pilot, or a minimal pilot-local indexer as fallback), with one-action follow.
5. **Citizen Console.** Identity surface at declared stub assurance, rendering identifier, assurance level, and capabilities, on the DID-ready adapter seam.
6. **Individual-scope compliance profile.** The published companion document per section 11, validated against the shipped dashboard.

Everything beyond this — richer context widgets, multiple external data integrations, notification digests, additional lenses — is stretch, not scope.

---

<a id="19-pilot-phases-and-timeline"></a>
## 19. Pilot Phases and Timeline

The pilot runs approximately **10–14 weeks** across four phases. Phases overlap deliberately; each ends in something demonstrable.

**Phase 0 — Design and contracts (weeks 1–3).** Product design pass over the prototype's information architecture against the pilot design (lenses, participation flows, console). Draft the individual-scope compliance profile positions (section 11) and the minimal PDS profile record shapes (section 14) with the Civic Identity pilot — the co-owned drafts are a phase gate, not an afterthought.

**Phase 1 — Live read path (weeks 3–6).** API client layer; manifest + feed + descriptor consumption from the reference hub and representative space; merged chronological activity view with inbox lens; mock data removed from converted surfaces. *Demo: real cross-space civic activity, live, in one view.*

**Phase 2 — Identity and action path (weeks 6–10).** Identity adapter and Citizen Console at stub assurance; deep-link participation with identity handoff; embedded participation via `POST /process/:id/action`; participation pointers recorded. *Demo: one citizen, two spaces, a vote and a comment, without leaving the dashboard.*

**Phase 3 — PDS, discovery, and conformance (weeks 9–14).** Subscription management moved from interim scaffold to the minimal PDS service (joint deliverable with the Identity pilot); export/import round-trip; discovery lens and search against the indexer; notifications lens with digest-style preferences; neutrality review; publication of the individual-scope compliance profile v0.1 and the pilot validation report. *Demo: the full demonstration scenarios of section 20, including the implementation-switch test.*

If the minimal PDS service slips (the pilot's top risk), Phase 3 completes every criterion except the PDS-backed portability test on the clearly-labeled scaffold, and the pilot's conformance claim is explicitly deferred — not fudged — until the joint deliverable lands.

---

<a id="20-pilot-demonstration-scenarios"></a>
## 20. Pilot Demonstration Scenarios

**Scenario 1 — One civic life, one view.** A Floyd County resident signs into the dashboard. Their inbox lens shows, interleaved chronologically: a new advisory vote from the Floyd Civic Hub, a consultation opened in their state delegate's Representative Space, and a result published in a regional hub they follow. Every item is a real activity from a real space's `GET /events` feed, and each carries its source attribution visibly.

**Scenario 2 — Act without leaving.** From the same session, the resident opens the advisory vote and casts a ballot in an embedded panel (action submitted to the hub, activity emitted by the hub), then opens the consultation and submits a comment via the representative space's flow. Both appear in the hosting spaces' feeds attributed to the same identity; both appear in the resident's participation history as pointers.

**Scenario 3 — Discover and follow.** The resident searches discovery for housing-related spaces in Virginia, finds a coalition hub they had never heard of, reads its manifest-derived profile, and follows it. The subscription is written to their PDS; the hub's activity begins appearing in their inbox. No account was created anywhere.

**Scenario 4 — Switch dashboards, keep your life.** The resident exports their PDS data, opens a *second, independent* dashboard instance, imports, and sees their full subscription list, preferences, and participation history render immediately — zero re-follows, zero reconfiguration. The dashboard was replaceable; the citizen's civic life was not disturbed.

**Scenario 5 — The honest console.** Throughout, the Citizen Console shows exactly what the identity layer can claim: in the pilot, a stub-assurance identity with its capabilities; in the joint demo with the Civic Identity pilot, a DID with verifiable credentials — same console, same seam, no rebuild.

---

<a id="21-success-and-validation-criteria"></a>
## 21. Success and Validation Criteria

The pilot succeeds when all of the following are demonstrated and recorded in the validation report:

1. **Cross-space participation, dashboard-only.** One citizen follows two live spaces, votes in a process hosted by one and comments in a process hosted by the other, entirely from the dashboard; both activities appear in the hosting spaces' `GET /events` feeds attributed to the same identity, and both participation pointers appear in the citizen's history.
2. **Portability round-trip.** PDS export from one dashboard instance imports into an independent instance with 100% of subscriptions, preferences, and participation pointers preserved, and the second instance renders a working feed from the imported subscriptions on first load.
3. **Zero mock data.** Every item on the dashboard's conformant surfaces is traceable to a live manifest, feed entry, or process descriptor; the mock data module is deleted, not dormant.
4. **Pointer discipline.** Inspection of the PDS export confirms it contains pointers (process id, space DID, action, timestamp, URL) and no copies of community content — the §9.4 property, verified mechanically.
5. **Discovery to participation.** A citizen finds a previously unknown space via the discovery lens, follows it, and participates in one of its processes within the same session.
6. **Neutrality verified.** Review confirms the shipped defaults: chronological ordering, no engagement ranking, no hidden curation, configurable indexer, open-source release.
7. **Compliance profile published.** The individual-scope compliance profile v0.1 is published with a per-endpoint applicability table, reviewed against Civic Space Specification §7, with every deviation the implementation forced documented as spec feedback.
8. **Minimal PDS profile ratified.** The joint profile is published and implemented by both its owners: the identity layer serves it, the dashboard consumes it, and criterion 2 runs against it (not against the scaffold).
9. **Adapter seam demonstrated.** The identity adapter swap (stub → DID-backed, in the joint demo with the Civic Identity pilot) touches the adapter and console rendering only — no changes to feed, participation, or PDS code.

---

<a id="22-expected-deliverables"></a>
## 22. Expected Deliverables

1. **The reference Citizen Dashboard application** — open source; an individual-scoped Civic Space, conformant to the Space API Profile per the individual-scope compliance profile; assembled from the shared components (feed engine lenses, process runtime, Citizen Console, PDS surface, identity adapter, access control).
2. **The individual-scope compliance profile of the Civic Space Specification** (v0.1) — the per-endpoint applicability and client-side conformance definition of section 11; the pilot's primary specification artifact.
3. **The minimal PDS profile** (v0.1) — subscriptions + preferences + participation pointers; record shapes, access API, export/import format; **co-owned deliverable with the Civic Identity Pilot**, implemented on both sides.
4. **Subscription management via the minimal PDS** — follow/unfollow, notification preferences, export/import, with the interim local-storage scaffold retired.
5. **Cross-space activity view** — multi-feed consumption, provenance keyed on space DID, inbox/notifications/discovery lenses, migration re-binding honored.
6. **Process participation flows** — deep-link and embedded modes against live spaces, with the client-side action contract documented for the Civic Process Plugin pilot.
7. **Discovery UX** — search and browse against a real indexer, jurisdiction-first, one-action follow.
8. **Citizen Console** — stub-assurance identity surface on the DID-ready adapter seam.
9. **Pilot validation report** — the section 21 criteria with evidence, plus consolidated spec feedback: everywhere the Space, Activity, Process, or Discovery specifications had to be interpreted or bent for the individual scope.

---

<a id="23-relationship-to-other-civicsocial-pilots"></a>
## 23. Relationship to Other Civic.Social Pilots

The dashboard's dependency posture, with fallbacks named:

| Pilot | The dashboard consumes | Interim fallback |
|---|---|---|
| **Civic Identity** | Authentication, Citizen Node rendering, and the co-owned minimal PDS | Stub-assurance adapter (declared, per Space Spec §7.3); local-storage PDS scaffold, labeled non-conformant |
| **Civic Activity Feed & Discovery** | Feed aggregation engine; reference indexer | Direct per-space `GET /events` polling; minimal pilot-local indexer |
| **Civic Hubs** | Live publishers: manifests, feeds, hosted processes | The reference Civic Hub implementation, already running |
| **Civic Process Plugin** | Process descriptors, action contracts, embeddable dashboard surfaces | Deep-link participation for any process lacking an embeddable surface |

The flow runs the other way too. The dashboard is the first *client-side* conformance consumer the ecosystem has had: it validates the Hubs pilot's publications by consuming them cold, exercises the plugin framework's dashboard surface from the host side, gives the feed engine its first many-feeds consumer, and gives the Identity pilot a real application pulling the PDS into existence. The Citizen Dashboard is where the other pilots' outputs are proven to compose.

---

<a id="24-estimated-development-effort-and-budget"></a>
## 24. Estimated Development Effort and Budget

Estimated effort, aligned with the phase plan (section 19):

- Product design and interaction architecture: 3–4 weeks
- Live read path (API client, feeds, manifests, descriptors): 3–4 weeks
- Identity, console, and participation flows: 4–5 weeks
- PDS integration, discovery UX, conformance and validation: 3–4 weeks
- Coordination with the Civic Identity pilot on the minimal PDS profile: continuous, budgeted explicitly

Team shape: one product designer (part-time after Phase 0), one to two frontend/integration engineers, fractional coordination with the identity and feed pilots' engineers, and specification-writing time for the compliance profile.

Estimated budget range, consistent with program-level estimates for this pilot:

- **$75,000** — low estimate: MVP scope on the interim scaffold, single external data integration deferred
- **$120,000** — typical pilot: full MVP scope including the PDS-backed portability test and published compliance profile
- **$200,000** — expanded pilot: adds a second independent dashboard implementation of the compliance profile (the strongest possible portability demonstration), richer context widgets, and one live external data integration

---

<a id="25-risks-and-mitigations"></a>
## 25. Risks and Mitigations

**R1 — The PDS is unbuilt (highest risk).** The dashboard's portability story depends on a component no pilot has shipped. *Mitigation:* the minimal PDS profile is deliberately tiny (subscriptions + preferences + pointers), co-owned and scheduled in both pilots' plans; the interim scaffold sits behind the same internal interface so the swap is an adapter change; if the service slips, the pilot defers its conformance claim explicitly rather than shipping a fake portability demo.

**R2 — The dashboard becomes a gatekeeper.** A successful personal interface concentrates attention, and attention concentrations get monetized into ranking and lock-in. *Mitigation:* the neutrality commitments of section 10 are shipped defaults verified by criterion 6, subscriptions are structurally outside the dashboard's control (PDS), discovery indexers are pluggable, and the compliance profile makes competing implementations cheap to build — the durable anti-gatekeeper mechanism is replaceability, not policy.

**R3 — Notification fatigue.** A cross-space aggregator can bury a citizen in every hub's every activity, and fatigue is disengagement. *Mitigation:* lens separation (inbox ≠ notifications), per-subscription notification preferences in the PDS, digest-style defaults over interrupt-style, and treating "which activities deserve attention" as an explicit, user-controlled setting rather than a growth lever. The pilot measures scenario walkthroughs for overwhelm and records findings for the feed pilot.

**R4 — Dependency slip in upstream pilots.** Identity, feed engine, or indexer deliverables may land late. *Mitigation:* every dependency has a named interim (section 23); the dashboard's phases are sequenced so read-path work proceeds against the already-running hub and representative space.

**R5 — Context-widget scope creep.** External civic data integration is seductive and unbounded. *Mitigation:* section 16's posture — one live integration maximum in pilot scope, cached data elsewhere, and the conformance story explicitly excludes widgets.

**R6 — Single-implementation bias in the compliance profile.** A profile written against one dashboard risks encoding that dashboard's accidents as requirements. *Mitigation:* the profile is published for community review alongside the Civic Space Specification's open process; the expanded budget option funds a second independent implementation precisely to shake accidents out.

**R7 — Stub identity overstays.** Interim assurance has a way of becoming permanent. *Mitigation:* assurance is displayed, not hidden (section 15); criterion 9 requires the adapter swap to be demonstrated with the Civic Identity pilot before the program calls the interface layer done.

---

<a id="26-out-of-scope"></a>
## 26. Out of Scope

- **Native mobile applications.** The pilot ships a responsive web application; native clients are future work once the compliance profile stabilizes.
- **Algorithmic recommendations and personalization.** No relevance ranking, no recommendation engine — per the neutrality commitments and the Discovery Layer's phased model. Future ranking work must be user-selectable and transparent.
- **Multi-account and delegation.** One citizen, one foundation, one dashboard session. Household, caretaker, and organizational-operator patterns are deferred.
- **Full external civic-data integration suite.** Ballotpedia/VoteSmart/OpenStates/FEC-class integrations beyond the single pilot integration (section 16).
- **Hosting community content or deliberation.** The dashboard is not a discussion environment; moderation-bearing surfaces remain in hubs by design.
- **Citizen Account Provider implementation.** The provider is an infrastructure role governed by the Civic Identity Specification; this pilot consumes its services and builds none of it.
- **Federation push delivery.** Pull-based feed consumption satisfies v0.1 (Civic Space Specification §5.1); webhooks and ActivityPub delivery arrive with the Phase 3 federation work.

---

<a id="27-open-questions-for-further-design"></a>
## 27. Open Questions for Further Design

1. **Must a personal space be addressable?** The Space contract assumes a network-addressable application with a public manifest; a citizen's dashboard may be a client-side app with no public URL at all. What does discovery-manifest conformance mean at individual scope — is the manifest optional, private, or served by the Citizen Account Provider on the citizen's behalf?
2. **Where does the dashboard run?** Static SPA against public APIs, provider-hosted application, self-hosted — the compliance profile should stay deployment-agnostic, but the identity handoff and PDS access patterns differ meaningfully across the three. Which deployment is the reference posture?
3. **Does the dashboard hold its own space DID?** Section 3.5 of the Space Specification allows the space DID to be the sovereign anchor "or a DID controlled by it." Should the dashboard reuse the citizen's DID, or mint a subordinate space DID — and what does the distinction change for provenance and for multi-dashboard citizens?
4. **PDS write authorization.** Which applications may write to a citizen's PDS, under what consent framework, and how are conflicting writes from two authorized dashboards reconciled? (Shared with the Civic Identity pilot's open questions.)
5. **Pointer dereference when sources disappear.** How much display metadata should a participation pointer carry so history remains meaningful when a hosting space is offline or gone — and does caching that metadata edge back toward content copying?
6. **Notification semantics across scopes.** Should spaces be able to mark activities as notification-worthy (an envelope hint), or is attention allocation purely the citizen's/lens's concern? A hint field trades neutrality for usability; the feed pilot shares this question.
7. **Embedded-surface trust.** When the dashboard embeds another space's process panel, what is the sandboxing and capability story on the *dashboard's* side of the boundary? The plugin architecture answers this for hosts running plugin code; the dashboard-as-embedding-host case should be confirmed against the same three-tier model.
8. **When does the dashboard emit?** Personal subscription changes are PDS writes by default — but are there individual-scope activities (e.g., a citizen publishing a public endorsement) that should flow through a dashboard emission path, and if so, under which visibility rules?

---

<a id="28-conclusion"></a>
## 28. Conclusion

The Citizen Dashboard Pilot delivers the piece of the Civic.Social program that people can see: one unified civic view for the individual, across hubs, spaces, and issues, built entirely from public contracts. Along the way it performs three services for the ecosystem that no other pilot can. It proves the Interface layer by assembling the shared components into a real application. It proves the Civic Space primitive generalizes by being the first space built at a scope other than community — and it publishes the individual-scope compliance profile so the next space type inherits a worked example. And it forces the Personal Data Store from architecture into existence, in deliberately minimal form, co-owned with the Civic Identity pilot.

The deeper commitment the pilot encodes is the one the ecosystem browser metaphor carries: the personal civic interface should be a space the citizen holds, not a platform that holds the citizen. Subscriptions in the citizen's own data store, participation recorded as pointers into the communities that hosted it, chronological feeds with no hidden hand, and a compliance profile that makes every part of the dashboard replaceable — including the reference implementation itself. If the pilot succeeds, its strongest result is not the application; it is that anyone can build a better one and no citizen loses anything by switching.

---

*Citizen Dashboard Pilot — Civic.Social Infrastructure Program*
*civic.social | contact@civic.social*
