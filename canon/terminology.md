---
status: stable
last-reviewed: 2026-07-24
owners: [adam]
version: 0.2
---

# Civic.Social Terminology Glossary — Canonical Reference (v2.0)

## Core Architecture Terms

**Civic Space**
The core host primitive of the ecosystem: a scoped digital environment that anchors a sovereign identity-and-data foundation, hosts Civic Processes through the plugin contract, emits and consumes Civic Activities through a single emission path, publishes a discovery manifest, and satisfies the portability contract for the data it stewards. Civic Spaces form an **open, extensible set of types distinguished by scope** — anyone can build a new space type on the same protocol. Defined normatively in the Civic Space Specification.

**Space Scope**
The mandatory type parameter of every Civic Space, drawn from the Sovereign Foundation's holder taxonomy:
- **`community`** — held by a community → the **Civic Hub**
- **`individual`** — held by a person → the **Citizen Dashboard**
- **`entity`** — held by an accountable public role (elected official, candidate, institutional body, or other accountable-leadership role) → the **Representative Space**
The set is open: future scopes may be registered without redesigning the primitive. Scope is mandatory because it determines the space's sovereign anchor, membership model, and portability profile — none of which can be inferred from protocol conformance alone; see the Civic Space Specification §1.4 for the design rationale.

**Civic Hub**
A community-scoped Civic Space: a digital civic space operated by a community, jurisdiction, or organization. Hubs host civic processes, enable community participation, and publish civic activities. Each hub is independently operated while interoperating through shared identity and federation protocols. "Civic Hub" names the space *type*; the host contract it conforms to is defined at the Civic Space level.

**Citizen Dashboard**
An individual-scoped Civic Space: a personal civic interface where citizens access tools, view their activity feed, manage space subscriptions, and discover civic opportunities. It holds *personal* data (via the Citizen Node and Personal Data Store), not community data.

**Representative Space**
An entity-scoped Civic Space: the neutral, public-facing surface for elected officials, candidates, and institutional bodies that hold governance authority. Entities control their voice; they do not control the accountability data displayed alongside it.

**Civic Process**
A structured, stateful civic interaction that enables participation (e.g., advisory voting, citizen assemblies, participatory budgeting, petitions, public consultations).

**Civic Plugin**
The general packaging and trust unit of the ecosystem: a manifest, a declared trust tier, and a capability declaration, installable into any conformant host environment. What a plugin *provides* determines its **kind**. The first specified kind is the Civic Process Plugin; future kinds — for example, display/lens plugins such as a calendar view that listens to activities and renders a UI surface without providing any process type — reuse the same manifest, trust tiers, and capability schema. Defined in the Civic Plugin Architecture.

**Civic Process Plugin**
The first specified kind of Civic Plugin: a modular implementation of a Civic Process that can be installed and run across multiple civic spaces (hubs, dashboards, representative spaces, external websites). Plugins are universal by default and integrate with their host only through the identity and activity seams.

**Civic Activity**
A standardized data object representing an action or lifecycle transition within the ecosystem. Activities are the distribution layer — the single shared stream that all feeds, notifications, and federation surfaces are built from. The term is aligned with the vocabulary of ActivityStreams 2.0. (For v0.1, the wire-format field is `event_type` and the transport endpoint is `GET /events`; see the Civic Activity Specification.)

**Civic Activity Feed**
The aggregation and distribution layer for civic activities — one shared stream refracted into many lenses (inbox, notifications, discovery, space view, embed). Feeds are not scoped to any one audience or interface: every space publishes a feed of its own activity (a hub's space view is its feed), and any conformant consumer — a citizen's dashboard, a community hub, an organization, or a third-party page via embed — can assemble and render feeds from the streams it follows.

**Civic Identity**
Decentralized identity infrastructure based on DIDs and Verifiable Credentials enabling trusted participation across the ecosystem. Defined normatively in the Civic Identity Specification.

**Civic Credentialing**
The system for issuing, displaying, and verifying civic credentials (badges). Badges are a publicly-displayed type of credential.

**Civic Process Descriptor**
A machine-readable description of how a process integrates with the ecosystem (actions, required credentials, endpoints).

**Infrastructure Roles**
Ecosystem participants that serve the foundation rather than hosting processes at a scope — they are service providers, **not Civic Spaces**. The set of roles is open; two have defined contracts in the current specifications:
- **Citizen Account Provider** — hosts a citizen's foundation (Citizen Node and Personal Data Store) on their behalf; federated, like an email provider for civic identity. Citizens can migrate between providers.
- **Badge / Credential Issuer** — a third-party organization issuing verifiable credentials into the ecosystem (pledges, endorsements, attestations of office).

Further roles are anticipated as the ecosystem grows — for example:
- **Space Hosting Provider** — operates Civic Spaces on behalf of their holders (e.g., a managed hub for a community without a technical team). The portability contract is what keeps holders sovereign over hosted spaces: a community can always migrate away.
- **Discovery Index Operator** — runs a discovery index that ingests space manifests and serves search and browse. Anyone may operate one; no index has exclusive claim to the ecosystem's map.

## The Four Canonical Specifications

The ecosystem is anchored by four open specifications, which extend underlying open web standards (W3C DIDs and Verifiable Credentials, OpenID4VCI/OpenID4VP/SIOPv2, ActivityPub/ActivityStreams, JSON-LD):

1. **Civic Space Specification** — the scoped host contract every space conforms to (portability, identity integration, plugin hosting, discovery, the Space API Profile)
2. **Civic Process Specification** — the interactive unit (lifecycle profiles, actions, descriptors)
3. **Civic Activity Specification** — the shared activity object and stream (schema, type registry, visibility, the ActivityStreams bridge roadmap)
4. **Civic Identity Specification** — the portable citizen-owned key (Citizen Node, Personal Data Store, credential types, identity policy)

Companion documents: the Civic Plugin Architecture, the Discovery Layer Specification, and the Authorization Model Note.

## Lifecycle Terms

**Process States**
The canonical state vocabulary for Civic Processes: `draft` → `scheduled` → `active` → `closed` → `finalized`. The full five-state sequence is the **deliberative lifecycle profile**; process types declare their lifecycle profile in their descriptor (see the Civic Process Specification's lifecycle profiles).

## Identity Terms

**DID (Decentralized Identifier)**
A globally unique, self-sovereign identifier that does not rely on any centralized registry or authority. Persons, entities, communities, and Civic Spaces themselves hold DIDs.

**Verifiable Credential (VC)**
A cryptographically verifiable digital credential issued by a trusted issuer that can be independently verified by any other party.

**Citizen Node**
The fundamental identity primitive: a DID + credentials + ability to authenticate and act within the Civic.Social ecosystem.

**Personal Data Store (PDS)**
A decentralized data store that holds user social graph and preferences, separate from the identity layer itself. Enables portability and user control.

**Sovereign Foundation**
The layer of participant-owned identity and data beneath every interface: the Citizen Node + Personal Data Store (held by the person), the Entity Node + Entity Data Store (held by the entity), and the Community Node + Community Data Store (held by the community). Portable across the ecosystem; no vendor lock-in.

## Architecture Layers

The Civic.Social architecture is organized into five canonical layers, read bottom → top:

1. **Open Web Standards** — the bedrock, adopted not invented: DIDs, Verifiable Credentials, OpenID4VCI/OpenID4VP/SIOPv2, ActivityPub/ActivityStreams, JSON-LD, OAuth 2.0/OIDC
2. **Civic Specifications** — Civic.Social's open specs, which extend the web standards: Civic Space · Civic Process · Civic Activity · Civic Identity (+ companions)
3. **Sovereign Foundation** — identity and data owned by the participant, labeled by holder: person / entity / community
4. **Components** — reusable building blocks, one engine many lenses: Activity Feed engine, Process runtime, Citizen Console, PDS, Identity Adapter, Access Control. Two components (Activity Feed, Processes) are shippable as standalone embeds on any web page.
5. **Interfaces** — the distinct pieces of software people use: Civic Spaces by scope (Civic Hub, Citizen Dashboard, Representative Space, + your space here) and infrastructure roles (Citizen Account Provider, Badge/Credential Issuer)

Federation is a capability of the Civic Activity Specification and the Discovery Layer (cross-cutting, not a layer). Portability is a contract of the Civic Space Specification over the Sovereign Foundation (a property, not a layer).

## Deprecated Terms (Do Not Use)

| Deprecated Term | Replacement |
|---|---|
| "Civic Capability" | "Civic Process" |
| "Civic Capability Plugin" | "Civic Process Plugin" |
| "Civic Capability Plugin Framework" | "Civic Process Plugin Framework" |
| "Elements" (as architecture term) | "Civic Processes" |
| "Civic Badging" | "Civic Credentialing" |
| "Civic Feed" / "Civic News Feed" / "Event Feed" | "Civic Activity Feed" |
| "Civic Event" (as protocol object / spec name) | "Civic Activity" / "Civic Activity Specification" |
| "Civic Hub Specification" / "Interoperable Civic Hub Specification (ICHS)" / "Civic Hub Compliance Specification" | "Civic Space Specification" (Civic Hub remains the name of the community-scoped space type) |
| "host environment" (as undefined term) | "Civic Space" (or "embed context" for in-page process embeds) |
| "office-scoped" / "public-office-scoped" | "entity-scoped" |
| 6-layer / 7-layer architecture enumerations | The five canonical layers above |

---

**Last updated:** July 24, 2026
**Status:** Canonical terminology reference for all Civic.Social documents
**Contact:** contact@civic.social
