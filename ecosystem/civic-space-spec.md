---
status: review
last-reviewed: 2026-07-03
owners: [adam]
version: 0.2
---

# Civic Space Specification

## Draft v0.2

> **Scope note.** The **Civic Space** is the scoped host primitive that every space type in the ecosystem conforms to. The Civic Hub is the canonical *community-scoped* space type; portability, identity integration, plugin hosting, and discovery are defined at the Space level. This document also includes the concrete **Space API Profile**, so the interoperability contract and the implementable API live in one document.

## Executive Summary

The Civic Space Specification proposes a standards-based architecture for autonomous civic communities, individuals, and accountable public entities to operate online without dependence on a single software platform or identity provider. The specification defines how civic spaces, civic processes, identity systems, and data structures interoperate while preserving both local governance autonomy and individual identity sovereignty.

A **Civic Space** is a scoped host environment: a network-addressable application that anchors a sovereign identity-and-data foundation appropriate to its scope, hosts Civic Processes through the plugin contract, emits and consumes Civic Activities through a single emission path, publishes a discovery manifest, and satisfies the portability contract for the data it stewards. Space types form an **open, extensible set distinguished by scope** — community-scoped Civic Hubs, individual-scoped Citizen Dashboards, entity-scoped Representative Spaces, and future space types anyone can build on the same protocol.

This specification introduces a layered architecture combining decentralized identity (DIDs and Verifiable Credentials), canonical civic data objects, modular civic process plugins, and open federation protocols. Together, these components enable communities to deliberate, propose, vote, organize, and coordinate while maintaining portability of their governance history and social relationships.

A core goal of the specification is **portability of civic spaces across engines**. A community must be able to migrate its full community data, relationships, governance history, and operational configuration from one space engine to another without losing structure, participation records, or social graph continuity — and a citizen's own space (their dashboard, identity, and relationships) must travel with them across providers. This ensures that no software vendor can lock a community or a citizen into a single platform and that the ecosystem can support multiple interoperable implementations.

Another key objective is to support **multiple interoperable space engines**. Existing community platform projects — such as Bonfire Networks, Social Roots, Roundabout, and other civic or community software — could implement this specification so that communities using different platforms can still interoperate and migrate between them. By enabling multiple compatible implementations, the ecosystem avoids vendor lock-in while encouraging innovation among providers.

This draft (v0.2) focuses on defining the foundational architecture, the canonical data models, and the minimal API profile needed for interoperability. It is intended as a working document to solicit feedback from civic technologists, governance researchers, standards bodies, and software developers.

# Specification Status

This document represents **Draft Version 0.2** of the Civic Space Specification.

The purpose of this draft is to:

- Present a coherent technical architecture for interoperable civic spaces of every scope
- Demonstrate how decentralized identity, civic processes, and community software can interoperate
- Invite feedback from civic technologists, governance researchers, open-standards communities, and software builders
- Identify gaps, edge cases, and implementation challenges before formal standardization

This document should be considered **exploratory and collaborative** rather than finalized.

Version 0.2 focuses on defining:

- The Civic Space primitive and its scope taxonomy
- Core architectural positioning (the five-layer ecosystem model)
- Canonical civic objects
- Identity integration requirements
- Federation and activity models (by reference to the Civic Activity Specification)
- The Space API Profile (the minimal implementable interface)
- Portability and migration guarantees, as a conformance ladder

Future versions may refine or expand:

- Civic process schemas
- Federation protocols
- Governance interoperability
- Identity provider portability
- Reputation and moderation portability

The long-term goal is to support an **ecosystem of interoperable civic platforms** rather than a single centralized application.

**A note on conformance phasing.** Specifications in this ecosystem define the *target state*. Reference implementations converge on them incrementally through the pilot program; each pilot brings the running software up to a further slice of this contract. Non-conformance of an early implementation is a sequencing fact, not a specification failure — the conformance ladder in Section 8 and the compliance profiles in Section 7 exist precisely to make that convergence measurable.

Feedback from implementers, civic organizations, and standards bodies is strongly encouraged.

---

# Table of Contents

1. Specification Status
2. Purpose & Scope
   - 1.1 Purpose
   - 1.2 Scope
   - 1.3 Definition of Civic Space
   - 1.4 Space Scopes and Space Types
   - 1.5 Infrastructure Roles (Not Spaces)
   - 1.6 The Civic Hub (Community-Scoped Space)
3. Architectural Overview
   - 2.1 Design Principles
   - 2.2 Position in the Layered Architecture
4. Identity Layer Specification
   - 3.0 Identity Architecture Model
   - 3.1 Core Requirements
   - 3.2 Authentication
   - 3.3 Credential Types
   - 3.4 Identity Portability
   - 3.5 Space Identity (Space DIDs)
5. Canonical Civic Object Model
   - 4.1 Canonical Civic Primitives
   - 4.2 Person
   - 4.3 Organization
   - 4.4 CivicSpace
   - 4.5 Membership
   - 4.6 Subscription / Following
   - 4.7 Access Control Model
   - 4.8 Post / DeliberationThread
   - 4.9 Proposal
   - 4.10 Vote
   - 4.11 Delegation
   - 4.12 CivicProcess
   - 4.13 Badge / Credential Object
   - 4.14 Immutability & Versioning
   - 4.15 Object Integrity Requirements
6. Federation & Interoperability Layer
7. Plugin Architecture Requirements
8. Space API Profile (v0.1)
9. Portability & Migration Specification
10. Public vs Private Space Requirements
11. Specification Stewardship & Versioning
12. Open Design Questions

---

# 1. Purpose & Scope

## 1.1 Purpose

The purpose of this specification is to establish a standards-based foundation for autonomous Civic Spaces — scoped digital environments that retain sovereignty over their governance, data, and identity integration.

This specification exists to ensure that:

- Civic communities govern their own spaces (local, jurisdictional, or issue-based).
- Individuals and accountable public entities have spaces at their own scope on the same protocol.
- No software vendor can trap a community, an individual, or an entity through technical lock-in.
- Individuals can use a decentralized Civic ID to access any compliant space.
- Communities can migrate between space engines without loss of structure, history, or legitimacy.
- Independent space engines can interoperate through open protocols.

Autonomy is the primary design objective.

Autonomy applies at two levels:

1. **Space Autonomy** — Each Civic Space governs its own moderation policies, civic processes, plugin configuration, and data stewardship, at its scope.
2. **Individual Identity Autonomy** — Individuals authenticate using decentralized identity standards (DIDs + Verifiable Credentials) rather than server-bound accounts.

This specification defines the minimum technical requirements necessary to preserve both forms of autonomy while enabling network-level interoperability.

---

## 1.2 Scope

This specification defines:

- The Civic Space primitive and its open scope taxonomy
- Identity integration requirements (DID / VC support)
- Canonical civic object structures
- Federation and activity interoperability requirements (by reference to the Civic Activity Specification)
- Plugin architecture constraints (by reference to the Civic Plugin Architecture)
- The Space API Profile — the minimal implementable interface
- Portability and migration requirements, as a conformance ladder
- Minimum export/import guarantees

This specification does NOT:

- Mandate a specific space engine implementation
- Mandate a specific user interface
- Define governance rules for individual communities
- Require centralization of moderation or authority

This is a protocol-layer and data-layer specification — not a product definition.

---

## 1.3 Definition of Civic Space

A **Civic Space** is a scoped host environment in the Civic.Social ecosystem: a network-addressable application that

- (a) is anchored to a **sovereign identity-and-data foundation** appropriate to its scope (see 1.4),
- (b) **hosts Civic Processes** through the plugin contract defined by the Civic Process Specification and the Civic Plugin Architecture,
- (c) **emits and consumes Civic Activities** through a single emission path, per the Civic Activity Specification,
- (d) **publishes a discovery manifest** (see Section 8 and the Discovery Layer Specification),
- (e) **mediates all participation** through the identity adapter and a single authorization seam, and
- (f) **satisfies the portability contract** (Section 9) for the data it stewards.

The defining test is (b) at a scope: *if it hosts processes at a scope, it is a space; if it serves the foundation, it is an infrastructure role* (see 1.5).

A Civic Space MAY enable civic processes through plugins, but a rich process catalog is not required for compliance — a space that hosts a single process type (or, for some scopes, primarily *surfaces* processes hosted elsewhere) can be fully conformant.

---

## 1.4 Space Scopes and Space Types

Every Civic Space declares exactly one **scope**, drawn from the Sovereign Foundation's holder taxonomy. The scope determines which sovereign anchor the space binds to, which membership model applies, and which portability profile (Section 9.4) governs its export.

| Scope | Sovereign anchor | Data store | Canonical space type |
|---|---|---|---|
| `community` | Community Node (collective DID + credentials) | Community Data Store | **Civic Hub** |
| `individual` | Citizen Node (DID + VCs) | Personal Data Store | **Citizen Dashboard** |
| `entity` | Entity Node (DID + role credentials) | Entity Data Store | **Representative Space** (sub-types: public official, candidate, institutional body) |
| *(open)* | *(a Node of the new holder kind)* | *(its data store)* | *+ your space here* |

**The set of space types is open.** A new space type registers:

1. a scope identifier and the sovereign Node / Data Store kind it anchors to,
2. its membership / relationship model (see 4.5–4.6),
3. its portability profile (Section 9.4), and
4. optionally, host-specific plugin surfaces (per the Civic Plugin Architecture's opt-in host-requirements mechanism).

Everything else — identity integration, activity emission, process hosting, discovery, the authorization seam — is inherited from this specification unchanged. A conformant new space type requires **no changes** to this specification.

**Why scope is mandatory (design rationale).** Conformance establishes that a space speaks the protocol; scope establishes **who holds it** — and three contracts are undecidable without that answer. First, the sovereign anchor: every space binds to a holder's Node and Data Store, and the binding cannot be inferred from behavior. Second, the membership and relationship model (4.5–4.6). Third, and most consequentially, the portability profile (9.4): a community export is a full tenant migration; an individual export contains pointers into other spaces rather than copies (copying would exfiltrate other participants' contributions through a personal export); an entity export must preserve third-party accountability data with provenance intact (so a record cannot be laundered by migration). An exporter facing an unscoped space could not know which obligation applies, and either wrong guess causes real harm. Scope also protects the open type set: because consumers will encounter space types they have never seen, behavior must key off a coarse classifier rather than the type itself — a feed, indexer, or migration tool that has never met a new community-scoped type still handles it correctly the moment it reads `scope: community`. (This is the same mechanism that lets ActivityPub consumers handle unfamiliar actors via the actor type.) The cost is a single required field. Note the distinction between software and instance: a space *engine* may be scope-generic, but a deployed *instance* has an actual holder and actual data, and therefore declares exactly one scope.

**Scope stability and type transitions.** A space's scope is stable for its lifetime. A space's *type within a scope* MAY transition where the type's own specification defines it (for example, a candidate's entity-scoped space transitioning after an election). Type-transition semantics are an open design area (Section 12) and are defined by space-type specifications, not here.

**Entity scope naming.** The entity scope covers elected officials, candidates, and institutional governance bodies, and generalizes to any role in which a person or collective body holds accountable leadership over a constituency. Earlier documents used "office-scoped" or "public-office-scoped"; **`entity`** is the canonical scope name.

---

## 1.5 Infrastructure Roles (Not Spaces)

Two ecosystem participants serve the foundation rather than hosting processes at a scope. They are **service providers governed by the Civic Identity Specification**, not Civic Spaces:

- **Citizen Account Provider** — hosts a citizen's foundation (Citizen Node and Personal Data Store) on the citizen's behalf; federated, like an email provider for civic identity. Citizens can migrate between providers without losing credentials or relationships.
- **Badge / Credential Issuer** — a third-party organization issuing verifiable credentials into the ecosystem (pledges, endorsements, attestations of office, participation records).

Keeping these out of the Space taxonomy keeps the primitive crisp and keeps their obligations where they belong: in the identity layer's provider and issuer requirements.

---

## 1.6 The Civic Hub (Community-Scoped Space)

A Civic Hub is the community-scoped Civic Space: a digitally hosted community space that

- Represents a jurisdiction, institution, or civic community
- Hosts deliberation, proposals, voting, assemblies, or other civic processes
- Integrates decentralized identity for authentication
- Can export its full governance and social history in structured form
- Can interoperate with other spaces via open standards

A Civic Hub may be:

- Jurisdictional (town, county, state, federal)
- Institutional (school board, nonprofit, coalition)
- Issue-based (climate coalition, housing task force)
- Private but credential-gated

All compliant hubs must satisfy the autonomy and portability guarantees defined in this document.

---

# 2. Architectural Overview

## 2.1 Design Principles

- Individuals control their own digital identity through decentralized identity standards (DIDs and Verifiable Credentials), rather than identities being owned by any single platform.
- Communities, individuals, and entities retain control over their own data and history, at their scope.
- Open standards over proprietary APIs.
- Integration without dependency.
- Local autonomy with network-level interoperability.

## 2.2 Position in the Layered Architecture

The Civic.Social ecosystem is organized into **five canonical layers** (see the terminology glossary): Open Web Standards → Civic Specifications → Sovereign Foundation → Components → Interfaces.

This specification is one of the four core documents at the **Civic Specifications** layer (Civic Space · Civic Process · Civic Activity · Civic Identity). Civic Spaces themselves live at the **Interfaces** layer, are assembled from reusable **Components** (activity feed engine, process runtime, identity adapter, access control), and operate on the participant-owned **Sovereign Foundation**.

Two concerns that earlier drafts modeled as standalone layers are re-homed in this revision:

- **Federation** is a capability of the Civic Activity Specification and the Discovery Layer (cross-cutting transport, not a layer).
- **Portability** is a contract of this specification over the Sovereign Foundation (a property every space must satisfy, not a layer).

Within any single space engine, implementations SHOULD keep the following concerns separable and replaceable: identity adapter, engine core, civic object storage, plugin runtime, activity emission, discovery, and export. The component factoring is the reference implementation's architecture, not a conformance requirement — an independent implementer may satisfy this contract monolithically.

---

# 3. Identity Layer Specification

> Normative identity requirements are being consolidated into the **Civic Identity Specification**. This section states the space-side obligations and retains the identity architecture model; where the two documents overlap, the Civic Identity Specification governs.

## 3.0 Identity Architecture Model

This specification adopts a **Managed Sovereign Identity Model**.

The objective is to preserve individual identity sovereignty without requiring every user to self-custody cryptographic keys.

Key principles:

- Identity is based on Decentralized Identifiers (DIDs).
- Verifiable Credentials (VCs) are portable across spaces.
- Identity must not be permanently bound to any single space engine.

However:

- Key custody MAY be managed by a trusted civic identity provider (managed wallet model).
- Self-custody MUST remain an available option.
- Users MUST be able to export their identity keys and credentials.

This enables:

- Practical usability (no mass key-loss problem)
- Reduced onboarding friction
- Eventual migration toward full self-sovereign custody
- Portability across space engines

### Identity and Social Graph Model

This specification adopts a **Managed Sovereign Identity Model with Transitional Central Stewardship**.

The ecosystem will begin with a single Civic Identity service implementation operated by a neutral, nonprofit steward ("Civic Identity Steward"). This steward:

- Operates the reference Civic Identity service.
- Maintains DID infrastructure and credential registries.
- Maintains user-associated social graph metadata.
- Provides managed key custody by default.
- Exposes open, standards-based APIs.

This initial central stewardship model is intended to:

- Reduce complexity during early network formation.
- Ensure coherent migration and interoperability guarantees.
- Build trust under nonprofit governance rather than venture-backed control.

However, the architecture MUST be designed from inception to allow:

- Export of identity keys and credentials.
- Export of full social graph data.
- Migration to an alternative compliant Civic Identity provider.
- Emergence of interoperable competing providers.

Long-term design objective:

- The Civic Identity layer becomes a standards-based ecosystem of interoperable providers.
- Users can migrate between identity providers without loss of credentials or relationships.
- No single identity operator can permanently centralize control.

This transitional model balances:

- Usability (managed custody)
- Network coherence (single initial provider)
- Long-term decentralization (provider portability)

### Identity and Relationship Storage

- Identity (DID + credentials) is sovereign and portable.
- Social graph relationships are associated with the user's Civic Identity account (the Personal Data Store), not owned by any individual space engine.

This means:

- Relationship data (follows, memberships, delegations, endorsements, reputation signals) is logically tied to the user's identity record within the Civic Identity service layer.
- Space engines may cache or index relationship data for performance, but they must not treat relationship data as proprietary or exclusive.
- Users must be able to export their full social graph in structured form.
- If a space engine is replaced, the user's relationships remain intact and re-bind to the new space instance.

Operationally, the Civic Identity service layer MUST:

- Maintain DID records
- Maintain credential references
- Maintain user-associated social graph metadata
- Expose portable, standards-based APIs for relationship retrieval
- Provide full identity + graph export in structured format

This preserves:

- Space autonomy at the space layer
- Individual portability at the identity layer
- Migration without relational amnesia

---

## 3.1 Core Requirements

A compliant Civic Space (target state) must:

- Support Decentralized Identifiers (DIDs)
- Support Verifiable Credentials (VCs)
- Support selective disclosure
- Support credential revocation checks
- Not bind user identity to a server-based account model

The Space API Profile (Section 8) permits **stub identity at a declared assurance level** during early phases: a space may accept opaque user identifiers through the identity adapter seam, provided the adapter is replaceable and the space declares its assurance level in its identity policy. Upgrading from stub identity to full Civic Identity must not require rebuilding the space.

## 3.2 Authentication

Named per protocol function:

- **OpenID4VCI** — credential issuance
- **OpenID4VP** — credential presentation
- **SIOPv2** — self-issued authentication
- Wallet-based authentication supported; FIDO-based authenticators compatible via the identity provider

## 3.3 Credential Types (Initial Registry)

- Resident credential (jurisdiction-scoped)
- Organizational credential
- Moderator credential
- Elected official credential
- Civic process facilitator credential

The full credential registry and schema publication requirements are defined in the Civic Identity Specification.

## 3.4 Identity Portability

Users must retain:

- Their DID
- Their credentials
- Their attestations

Space engines may store role mappings locally but must not control identity issuance. Role and authority bindings SHOULD be keyed to the participant's identifier (DID or portable id), not to provider-specific attributes such as email addresses, so that identity-provider migration does not strand the authorization layer.

## 3.5 Space Identity (Space DIDs)

Every Civic Space MUST hold a **stable decentralized identifier — the space DID** — distinct from its serving URL. The space DID is the sovereign anchor of its scope (Community Node, Citizen Node, or Entity Node) or a DID controlled by it.

- The space's serving URL is a **current binding**, resolvable via the space DID's document (service endpoint).
- Activities emitted by the space carry the space DID in `source.space_id` (Civic Activity Specification §2); `source.hub_id`/`hub_url` remain the v0.1 compatibility serialization.
- Cross-space references, subscriptions, and provenance SHOULD be keyed to the space DID, because it survives migration to a new URL or engine (Section 9.9).

DID method guidance: `did:web` is acceptable for spaces expected to remain on a stable domain; spaces that anticipate domain or provider migration SHOULD prefer a method with verifiable history and controller continuity (e.g., `did:tdw`). Method selection criteria are consolidated in the Civic Identity Specification.

---

# 4. Canonical Civic Object Model

The Civic Object Model defines the canonical data primitives used across interoperable Civic Spaces.

The design philosophy is:

1. **Reuse existing standards wherever possible** (especially Schema.org and ActivityStreams).
2. **Extend existing vocabularies only where civic functionality requires it.**
3. **Align with emerging civic process ontologies**, including the work of the civic technology research community such as the ontology efforts associated with MetaGov.

The goal is not to invent a new web ontology but to define a **minimal civic extension layer** that allows civic platforms to interoperate while remaining compatible with the broader semantic web.

All civic objects SHOULD be represented using **JSON-LD** and SHOULD reference existing vocabularies when applicable.

Recommended contexts:

- Schema.org (general web entities)
- ActivityStreams (social and activity events)
- Civic extension vocabulary (for governance-specific objects)

Example JSON-LD context pattern:

```
{
  "@context": [
    "https://schema.org",
    "https://www.w3.org/ns/activitystreams",
    "https://civicsocial.org/ns/civic"
  ]
}
```

> The civic extension context document at `https://civicsocial.org/ns/civic` is not yet published; publishing it is a prerequisite for the JSON-LD export format (Section 9.3) and the ActivityStreams bridge (Civic Activity Specification §9), and is tracked as ecosystem work.

---

## 4.1 Canonical Civic Primitives

This specification defines a **minimal interoperable set of civic primitives**. Additional civic processes MAY extend these primitives but MUST remain compatible with them.

Core primitives:

- Person
- Organization
- CivicSpace
- Membership
- Subscription / Following
- Post / DeliberationThread
- Proposal
- Vote
- Delegation
- CivicProcess
- CivicActivity (defined by the Civic Activity Specification)
- Badge / Credential

Petitions, assemblies, participatory budgeting, and similar interactions are **civic process types**, implemented as plugins composed from these primitives (a petition is a CivicProcess with signature actions; an assembly is a CivicProcess with deliberation and membership rules) — they are not separate object primitives.

These primitives enable portable representation of most civic processes including:

- participatory budgeting
- advisory voting
- citizen assemblies
- petitions
- policy proposals
- civic deliberation

Plugins MAY introduce additional specialized objects but MUST declare schema extensions.

---

## 4.2 Person

Reuse: `schema:Person`

Additional civic properties MAY include:

- DID identifier
- civic credentials
- jurisdiction membership

Required fields:

- DID
- display name
- credential references

---

## 4.3 Organization

Reuse: `schema:Organization`

Used for:

- civic organizations
- advocacy groups
- municipal agencies
- community groups

---

## 4.4 CivicSpace

Represents an **interoperable scoped environment capable of hosting civic processes** — the object form of the primitive defined in Section 1.3.

A CivicSpace of community scope (a Civic Hub) is a general-purpose community container that may represent many types of social or institutional groups. Examples include:

- jurisdictional governments (town, county, state)
- nonprofits and civic organizations
- advocacy groups
- coalitions and civic networks
- clubs and associations (e.g., Rotary Clubs)
- religious or cultural communities
- issue-based communities

Individual-scoped and entity-scoped spaces use the same object with their own scope value and type-specific configuration.

Suggested base: `schema:Organization` or `schema:WebSite` with civic extensions.

Fields:

- space identifier (the space DID; see 3.5)
- space scope (`community` | `individual` | `entity` | registered future scopes)
- space type (e.g., `civic.hub`, `civic.dashboard`, `civic.representative_space`)
- space name
- jurisdiction or community scope
- governance configuration
- plugin registry (enabled process types and plugin configuration)
- moderation policy reference
- access control policy

A CivicSpace MAY enable civic processes through plugins, but a full process catalog is not required for space compliance.

---

## 4.5 Membership

Represents a user's **formal participation** within a CivicSpace.

Membership is distinct from subscription or following.

Membership typically grants additional permissions such as the ability to post, participate in governance processes, or access restricted information.

Fields:

- person DID
- space identifier
- membership role
- join timestamp
- membership status

Membership SHOULD be represented using relationships between `Person` and `CivicSpace`.

**Membership models are scope-specific.** Community spaces carry the full membership/subscription model below. Individual spaces have an owner and followers rather than members. Entity spaces have a verified claimant (the entity) plus constituent engagement relationships; the entity's control extends to its own voice, not to third-party accountability data (the asymmetric authorization pattern, defined in the entity space type's own specification).

---

## 4.6 Subscription / Following

Represents a user's **non-member relationship** with a space.

Users MAY follow or subscribe to spaces without becoming members. This allows users to observe activity, learn from other communities, or discover civic processes without participating directly.

Fields:

- follower DID
- space identifier
- subscription timestamp
- notification preferences

Subscriptions typically grant **read access** but not **write access**.

Subscription data is held in the citizen's Personal Data Store (identity layer), not owned by the space; spaces may cache it (see 3.0, Identity and Relationship Storage).

---

## 4.7 Access Control Model

Civic Spaces MUST support **fine-grained read and write permissions**.

Space administrators SHOULD be able to configure policies governing:

- who can read content
- who can write or post content
- who can participate in civic processes
- who can view sensitive governance information

Example configuration:

- Public: anyone may read
- Followers: read access only
- Members: write access and process participation
- Moderators/Admins: governance and moderation permissions

Access control MAY vary by:

- content type
- civic process
- membership role

The specification does not mandate a specific access control implementation but requires that:

- permissions be explicitly defined
- all authorization decisions flow through a **single authorization seam** (a `canActor(actor, action, resource)` equivalent), per the Authorization Model Note, so the policy engine can evolve from roles to capabilities without rewriting interfaces
- access policies be exportable during migration
- space engines support both membership and subscription models where the scope calls for them.

---

## 4.8 Post / DeliberationThread

Reuse where possible:

- `schema:Comment`
- `schema:DiscussionForumPosting`

Fields:

- author DID
- content
- visibility scope
- timestamp
- revision history

These objects form the basis of civic deliberation.

---

## 4.9 Proposal

The **Proposal** object is the central governance primitive used to represent a policy proposal, initiative, decision request, or civic action within a CivicSpace.

Design goals:

- Interoperable across civic platforms (e.g., deliberation platforms, participatory budgeting tools, petition systems).
- Compatible with the semantic web.
- Alignable with emerging civic governance ontologies (such as MetaGov research).
- Compatible with ActivityStreams-style activity publication.

### Base Vocabulary Reuse

The Proposal object SHOULD reuse existing semantic standards where possible.

Suggested base types:

- `schema:CreativeWork`
- `schema:Action` (for procedural context)
- ActivityStreams activity objects for publication

JSON-LD context SHOULD include:

- [https://schema.org](https://schema.org)
- [https://www.w3.org/ns/activitystreams](https://www.w3.org/ns/activitystreams)
- civic extension namespace

### Core Fields

Required fields:

- proposal ID
- title
- description
- creator DID
- creation timestamp
- associated CivicSpace
- proposal status

Optional but recommended fields:

- jurisdiction
- associated CivicProcess
- linked deliberation thread
- supporting documents
- tags or topics

Example conceptual structure:

```
{
  "@type": "civic:Proposal",
  "id": "proposal:123",
  "name": "Ban chokeholds by police",
  "creator": "did:example:12345",
  "dateCreated": "2026-03-05T12:00:00Z",
  "status": "active",
  "space": "did:web:hub.athens.example",
  "process": "process:policy_vote",
  "discussion": "thread:938"
}
```

### Proposal Status States

Standard status values SHOULD include:

- draft
- open
- active
- voting
- closed
- adopted
- rejected
- archived

Platforms MAY extend these states but MUST map them to canonical equivalents for export.

### Process Compatibility

Proposals MUST be compatible with multiple civic processes including:

- deliberation-first processes
- advisory votes
- binding votes
- participatory budgeting
- petition-triggered votes

The Proposal object therefore references a **CivicProcess** rather than embedding process logic.

### Activity Publication

Proposal lifecycle transitions SHOULD be published through the activity layer as Civic Activities (created, updated, opened for voting, closed), per the Civic Activity Specification.

### Immutability Rules

Certain proposal properties SHOULD become immutable once the proposal enters an active governance stage.

Immutable after activation:

- proposal ID
- creator DID
- creation timestamp

Editable fields MAY include:

- description
- attachments
- tags

All edits MUST preserve revision history.

---

## 4.10 Vote

Represents an expression of preference within a civic process.

Votes are intentionally modeled as a portable primitive so that different civic platforms can interoperate while supporting a wide variety of decision-making methods.

### Core Fields

Required fields:

- vote ID
- proposal ID
- voter reference (see disclosure note below)
- vote value
- timestamp
- vote method

Optional fields:

- delegation reference
- weighting factor
- cryptographic proof

Votes SHOULD be cryptographically verifiable when used in binding civic processes.

### Ballot Disclosure

The binding between voter identity and vote value is governed by the process's **disclosure policy** (Civic Process Specification; Civic Activity Specification §7). In a secret-ballot configuration, the stored ledger and any public activities MUST NOT link voter identity to ballot content (participation records and ballot records are kept separable, e.g., a participation record for one-person-one-vote enforcement alongside an unlinked ballot record); in an on-the-record configuration, the linkage is explicit and published. Both configurations are conformant; the process descriptor MUST declare which applies before participation begins.

---

### 4.10.1 Supported Voting Methods

To ensure interoperability across civic platforms, the specification defines a **canonical set of voting method identifiers**.

Examples include:

- yes_no
- approval
- ranked_choice
- score
- quadratic
- delegated
- consensus_signal

Additional methods MAY be defined by plugins or extensions but MUST map to a canonical method for export and migration.

The voting method is typically defined at the **CivicProcess level**, not the Vote object itself.

---

## 4.11 Delegation

Represents the transfer of voting or participation authority from one participant to another within a defined scope.

Fields:

- delegation ID
- delegator DID
- delegate DID
- scope (a process, a process type, a topic, or a space)
- validity window (start / optional end)
- revocation status

Rules:

- Delegations MUST be revocable by the delegator.
- Delegation chains MUST be reconstructible from the record (for tally verification and for export under Section 9).
- Circular delegation MUST be detected and rejected at tally time.
- Delegation records follow the same disclosure policy machinery as votes (4.10).

---

## 4.12 CivicProcess

The **CivicProcess** object represents the procedural framework that governs how a community evaluates proposals or conducts collective decision-making.

Separating CivicProcess from Proposal allows the same proposal structure to be used across many types of civic workflows.

This design aligns with emerging civic governance ontologies developed in the civic technology research ecosystem (including MetaGov work).

### Examples of Civic Processes

- citizen assemblies
- participatory budgeting
- advisory votes
- petitions
- deliberation cycles
- town halls
- policy consultations

### Base Vocabulary Alignment

The CivicProcess object SHOULD reuse existing semantic standards where applicable:

- `schema:Event` for process lifecycle
- `schema:Action` for procedural logic
- ActivityStreams for publishing lifecycle activities

Where governance-specific concepts are required, the civic namespace MAY extend these vocabularies.

### Core Fields

Required fields:

- process ID
- process type
- associated space
- start timestamp
- end timestamp
- voting method (where applicable)

Optional fields:

- quorum rule
- eligibility rules
- credential requirements
- process stages
- associated proposal IDs

The normative process model — lifecycle profiles, actions, descriptors, events — is defined by the **Civic Process Specification**; this object is its portable data representation.

### Example Conceptual Structure

```
{
  "@type": "civic:CivicProcess",
  "id": "process:athens-budget-2026",
  "processType": "participatory_budgeting",
  "space": "did:web:hub.athens.example",
  "startTime": "2026-05-01",
  "endTime": "2026-06-01",
  "voteMethod": "approval",
  "quorum": "0.2"
}
```

---

## 4.13 Badge / Credential Object

Represents civic recognition or verification.

Fields:

- issuer DID
- recipient DID
- criteria reference
- status (active / revoked)

These objects SHOULD be compatible with Verifiable Credential standards.

---

## 4.14 Immutability & Versioning

Civic governance records require integrity guarantees.

Rules:

- Proposal creation activities SHOULD be immutable.
- Votes MUST be append-only records.
- Revision history MUST be preserved for editable objects.

---

## 4.15 Object Integrity Requirements

All canonical civic objects must be:

- Versioned
- Timestamped
- Referenced by stable identifiers
- Exportable in structured format
- Compatible with JSON-LD serialization

Cryptographic verification SHOULD be used where civic legitimacy requires it.

---

# 5. Federation & Interoperability Layer

## 5.1 Federation Protocol

Compliant spaces must support at least one open federation protocol:

- ActivityPub
- Or a Civic Federation Protocol compliant with this spec

Federation is optional in the v0.1 API profile (Section 8); pull-based feed consumption satisfies early interoperability.

## 5.2 Activity Schema

Spaces must emit standardized **Civic Activities** for process lifecycle transitions and participation actions — votes, assemblies, town halls, participatory budgeting, petitions, badge issuance, legislative proposals.

The activity envelope, required fields, type registry, extension namespace, and visibility/disclosure rules are defined normatively by the **Civic Activity Specification**; this document does not define a separate event field set. Activity signing (signature by the emitting space's DID) is planned in the Civic Activity Specification's v0.2+ extensions and is required before migrated or federated history can be independently verified (see 9.9).

## 5.3 Cross-Space Subscriptions

Spaces must allow:

- Subscription to other spaces
- Verification of remote provenance (by space DID once activity signing lands; by source URL in v0.1)
- Display of remote civic objects in the local feed

---

# 6. Plugin Architecture Requirements

## 6.1 Modular Design

Space engines must support modular civic process plugins.

Examples:

- Citizen assembly module
- Participatory budgeting module
- Advisory vote module
- Digital town hall module
- Petition module

The packaging, trust-tier, manifest, and capability model for plugins is defined by the **Civic Plugin Architecture**; the process contract a plugin implements is defined by the **Civic Process Specification**.

## 6.2 Constraints

Plugins must:

- Use canonical civic objects
- Not redefine identity primitives
- Not create proprietary data structures incompatible with export
- Declare their schema extensions
- Declare the capabilities they require and the activity types they emit (per the Civic Plugin Architecture); the space grants least privilege

---

# 7. Space API Profile (v0.1)

> Folded in from the standalone minimal Civic Hub API specification. This profile is the **minimal implementable interface** of a Civic Space — deliberately small, designed for rapid implementation, and the surface the reference implementation ships today. It applies to every scope; a space type MAY extend it. Conformance to this profile is **API Profile compliance**; the full target-state contract (DID identity, portability Level B+, federation) layers on top per the conformance phasing note in the Specification Status section.

## 7.1 Responsibilities

A Civic Space MUST:

- Host Civic Processes
- Accept user actions on processes
- Emit Civic Activities for all process activity
- Expose an activity feed endpoint
- Provide basic identity integration (a stub identity adapter is allowed in v0.1, at a declared assurance level)

## 7.2 Required API Endpoints

### 7.2.0 Discovery Manifest

```
GET /.well-known/civic.json
```

Purpose: enable space discoverability by indexers and external systems.

Response:

```json
{
  "name": "Example Civic Hub",
  "space": {
    "id": "did:web:hub.example",
    "scope": "community",
    "type": "civic.hub"
  },
  "jurisdictions": ["us-va-floyd"],
  "feeds": ["https://hub.example/events"],
  "processes": [],
  "contact": "optional"
}
```

Notes:

- `feeds` MUST include the space's activity feed endpoint.
- `processes` SHOULD list enabled process types (may be empty in v0.1).
- `space.id` is the space DID (3.5). Implementations predating space DIDs serve `"type": "hub"` at the top level; consumers SHOULD accept both forms for the life of v0.1.

### 7.2.1 Create Process

```
POST /process
```

Input: process type, metadata, lifecycle configuration
Output: created process object

The request/response contract is defined in the Civic Process Specification §12.

### 7.2.2 Get Process

```
GET /process/:id
```

Output: the published process descriptor and state (Civic Process Specification §12).

### 7.2.3 Execute Action

```
POST /process/:id/action
```

Input: action name, action payload; actor identity is taken from the authenticated context (never from the request body)
Output: success/error response per the Civic Process Specification's action contract

Behavior:

- Validate action (eligibility via the authorization seam)
- Update process state (if applicable)
- Emit activity/activities through the single emission path

### 7.2.4 Activity Feed

```
GET /events
```

Output: list of Civic Activities, ordered by timestamp (descending). Supports filtering by `process_id` and activity type. (`GET /activities` alias planned; see Civic Activity Specification §14.)

### 7.2.5 Recommended Read Endpoints

- `GET /process` — list processes (UI read layer)
- `GET /health` — health check

## 7.3 Identity Integration (v0.1)

Minimal requirements:

- Accept a user identifier (opaque id or DID) through a **replaceable identity adapter**
- Pass identity into action execution; the actor recorded on activities is the authenticated identity
- Stub credential verification allowed, with the assurance level declared in the space's identity policy

Target state: full DID authentication and verifiable credential checks per Section 3 and the Civic Identity Specification. The adapter seam exists so this upgrade does not require rebuilding the space.

## 7.4 Process Support (v0.1)

The space MUST support at least one process type (recommended: `civic.vote`), registered through a modular, replaceable process interface (registry/handler pattern per the Civic Process Specification and Civic Plugin Architecture). Avoid tight coupling between the space core and specific process implementations.

## 7.5 Data Storage (v0.1)

Allowed implementations: in-memory store, simple database, or managed Postgres. The API profile imposes no scalability or persistence requirements; the portability contract (Section 9) and the immutability rules (4.14) impose the durability obligations that matter.

## 7.6 API Profile Minimal Compliance

To be API Profile compliant, a Civic Space must:

- Implement the required endpoints (7.2.0–7.2.4)
- Support at least one process type
- Emit valid Civic Activities through a single emission path
- Expose the activity feed

Out of scope for the API profile (target-state requirements defined elsewhere in this document): UI, messaging, moderation systems, ranking, full ActivityPub inbox/outbox, real-time delivery.

---

# 8. (Reserved)

Section intentionally reserved to keep major numbering stable across drafts; discovery requirements live in the Discovery Layer Specification and Section 7.2.0.

---

# 9. Portability & Migration Specification

Portability is a first-class requirement of this specification.

A compliant Civic Space ecosystem MUST support migration with structural, relational, and process continuity — for communities moving between engines or providers, and for individuals moving between providers and interfaces.

## 9.1 Migration Conformance Ladder

Migration completeness is a **named conformance ladder**, not a single bar:

- **Level A — Archival.** Complete, deterministic, integrity-verified export of all civic objects, configuration, and activity history. Import produces a readable, verifiable archive space.
- **Level B — Continuity.** Level A, plus identity re-binding (DIDs intact, memberships and roles reconstructed, subscriptions re-pointed via the identity layer) and closed-process integrity (tallies, outcomes, and delegation records reconstructed and verifiable).
- **Level C — Live-state.** Level B, plus active process state (open voting windows, live tallies, delegation chains, plugin runtime state), with the plugin-unavailable fallback of 9.5.

Conformance claims MUST name their level. **The pilot-phase target is Level B**, with migration normally scheduled once active processes reach a terminal state (9.8); Level C is the v1.0 objective, gated on the plugin-state export contract. Emergency restoration from the most recent verified snapshot satisfies Level A semantics (9.8.3).

## 9.2 Migration Scope Definition

A complete migration (Level C; Levels A/B per their definitions above) preserves:

1. Identity continuity (DID references remain intact)
2. Social graph continuity (relationships re-bind)
3. Governance history (immutable records preserved)
4. Active process state (open proposals, live votes, delegations)
5. Plugin configuration and state
6. Badge status and credential references

Migration must not reduce a live civic system to a static archive (beyond Level A claims).

## 9.3 Full Space Export

Each space MUST provide deterministic export functionality including:

- Space metadata (including the space DID and scope)
- Identity references (**DIDs only; never private keys** — key material lives in wallets and identity providers, and MUST NOT appear in any space export)
- Membership lists
- Social graph edges
- Role mappings
- All civic objects (posts, proposals, votes, delegations, badges)
- Active process state (open voting windows, thresholds, quorum rules) — Level C
- Plugin configuration and plugin state
- Moderation configuration (rules, roles, policies — but not necessarily sanctions portability)

Export Format Requirements:

- JSON-LD or compatible structured format, against the published civic context (see 4.0 note)
- Canonical deterministic ordering — the export format specification MUST name its canonicalization algorithm (e.g., RDF dataset canonicalization or a defined key-ordering profile) so that two exports of identical state are byte-identical
- Explicit schema version tag
- Hash-signed archive
- Machine-validated against a published schema

> The export schema and canonicalization profile are deliverables of the Civic Hubs pilot; conformance is validated by the round-trip test in 9.10.

## 9.4 Per-Scope Portability Profiles

The contract shape above is universal; **what the export contains differs by scope**:

- **Community profile (Civic Hub).** The full tenant-migration case: 9.2's list in its entirety. Media and attachments are exported by reference with retrieval manifests (inline where size permits).
- **Individual profile (Citizen Dashboard).** The citizen's own foundation: identity and credential export happens at the wallet/identity-provider layer (never through a space export); the Personal Data Store contents (social graph, subscriptions, preferences, participation references); and **pointers into community and entity spaces, not copies of their content** — the substance of participation belongs to the process that hosted it. Individual portability is continuous by design: the foundation is always portable, so the snapshot machinery of 9.8 applies only weakly to this scope.
- **Entity profile (Representative Space).** Predominantly public record. Entity-voice content exports under the entity's control; third-party accountability data (badge issuances, outcome deliveries, responsiveness records including non-response records) exports **with provenance intact**, so that the record cannot be laundered by migration. This profile depends on activity signing (5.2) for full verifiability.

## 9.5 Active State Preservation (Level C)

The export MUST capture dynamic process state, including:

- Open proposal status
- Vote windows (start/end timestamps)
- Current vote tallies
- Delegation chains
- Quorum thresholds
- Rule configurations

Upon import, the receiving engine MUST:

- Reconstruct open processes
- Preserve object IDs
- Preserve timestamps
- Preserve vote integrity
- Reconstruct delegation graphs
- Resume process logic where feasible

If a plugin is unavailable in the receiving engine, the system MUST:

- Preserve raw data
- Flag incomplete execution state
- Allow manual reconciliation

## 9.6 Identity & Social Graph Rebinding

Upon migration:

- User DIDs MUST remain unchanged.
- Social graph relationships MUST re-bind to the new space instance. The re-binding substrate is the identity layer's relationship store (the Personal Data Store / Civic Identity service, 3.0): because relationships are keyed to user DIDs and the space DID — not to the old engine's internal ids or URL — the new engine re-derives its local caches from identity-layer records plus the imported archive.
- Cached space-local relationship data MUST reconcile against identity-layer graph records.

The migration process MUST NOT require users to re-register.

## 9.7 Non-Loss Guarantee

Migration MUST NOT result in loss of:

- Governance history
- Vote records
- Delegation chains
- Badge issuance records
- Identity references
- Plugin configuration metadata

If full runtime continuity is not technically possible due to engine incompatibility, data integrity MUST still be preserved in canonical form.

## 9.8 Migration Windows & Operational Constraints

This specification does NOT require real-time migration during active high-stakes civic processes.

### 9.8.1 Planned Migration Window

- Migration SHOULD occur during a declared maintenance window.
- Active high-stakes processes (e.g., binding votes, participatory budgeting allocations) SHOULD be completed or formally paused prior to migration.
- Communities MUST have the ability to announce and schedule migration events.

### 9.8.2 Snapshot-Based Migration

Migration SHALL operate on a deterministic snapshot model:

- A snapshot timestamp is defined.
- All civic objects and process state up to that timestamp are exported.
- The receiving engine reconstructs the space at that snapshot state.

This avoids requiring continuous cross-engine state synchronization.

### 9.8.3 Emergency Recovery Scenario

In the event of catastrophic space failure:

- Administrators MAY restore from the most recent verified snapshot.
- Restoration to a previous state SHALL be considered acceptable under this specification.
- Minor temporal gaps between last activity and snapshot time are operational risks, not specification failures.

### 9.8.4 Optional Advanced Continuity

Future versions MAY define advanced live-migration or state-streaming capabilities, but such functionality is NOT required for baseline compliance.

This approach prioritizes simplicity, predictability, administrative control, and reduced engineering burden, while preserving full structural and historical portability guarantees.

## 9.9 Identity & Discovery Continuity Across Migration

Snapshot migration moves *state*; the space DID preserves *identity*. The two together dissolve the tension between snapshot portability and continuous federation:

- The space DID (3.5) is unchanged by migration. Migration updates the DID document's service endpoint to the new serving URL.
- The final activity emitted from the old location is `civic.space.migrated`, carrying the new binding (Civic Activity Specification §4.4).
- The old location SHOULD serve a `moved` tombstone at `/.well-known/civic.json` pointing to the new binding, for the period the old domain remains under the community's control (Discovery Layer Specification).
- Indexers and subscribers re-key on the space DID and re-resolve the serving URL; provenance of previously distributed activities remains valid because it was keyed to `source.space_id`, and becomes independently verifiable once activity signing lands.

## 9.10 Portability Conformance

A portability conformance claim is validated by **round-trip testing**: export from engine 1 → import into an independent engine (or independent instance) → re-export → semantic equality with the original export (byte equality under the canonicalization profile). Export-only validation does not establish conformance. The round-trip test at Level B, executed between two live engine instances with a real community's data, is the pilot-phase acceptance bar.

---

# 10. Public vs Private Space Requirements

## 10.1 Public Civic Space

- Jurisdiction-scoped
- Verified resident participation
- Transparent archives
- Public activity feed

## 10.2 Private Civic Space

- Credential-gated access
- Optional encrypted data layer
- Optional suppression of public feed publication

Both must comply with identity, object, and portability requirements.

---

# 11. Specification Stewardship & Versioning

This document is a technical specification.

The purpose of this section is not to define institutional governance, but to ensure technical continuity, version control, and long-term maintainability.

## 11.1 Reference Implementation

A reference implementation of this specification MAY be developed and maintained by a single coordinating entity during early phases.

However:

- The specification MUST remain implementation-agnostic.
- No clause may require dependency on a specific organization.
- All interfaces must be standards-based and publicly documented.

## 11.2 Versioning

- The specification must follow semantic versioning.
- Breaking changes must be clearly documented.
- Export/import formats must be version-stamped.
- Backward compatibility guarantees should be explicitly defined in future versions.

## 11.3 Extensibility

- Extensions must not break canonical civic primitives.
- New space types register per 1.4 without modifying this specification.
- Plugins must declare schema extensions.
- Future identity, federation, or portability revisions must remain backward compatible wherever feasible.

## 11.4 Conformance Suite

A **Space Conformance Suite** — machine-runnable tests covering manifest validity, the API profile endpoints, single-emission-path behavior, visibility enforcement, and the portability round-trip (9.10) — is the planned companion to this specification, parallel to the plugin development harness in the Civic Process Plugin program. Conformance language in this document is written to be testable by that suite.

---

# 12. Open Design Questions

## 12.1 Identity Future-Proofing

- How do we design for DID migration between identity providers?
- Should user DIDs remain stable when migrating providers, or should credentials be re-issued under new DIDs? (This must be answered before the reference identity service mints identities at scale.)
- Should the Civic Identity steward anchor DIDs to a specific DID method, or support multiple DID methods from inception?
- What mechanisms enable DID method migration in the future (e.g., DID rotation, key rotation, controller updates)?
- How can we ensure long-term portability if a DID method becomes obsolete or politically compromised?

(These questions may require consultation with standards leaders in the DID/VC community, and are consolidated in the Civic Identity Specification.)

## 12.2 Moderation & Governance Portability

- Should moderation decisions be portable across spaces?
- Should bans follow a DID across spaces, or remain local to each community?

## 12.3 Reputation Model

- How should reputation function across spaces?
- Is reputation portable, contextual, or space-scoped?

## 12.4 Federation Model

- Is federation mandatory or optional for compliance? (v0.1 answer: optional; pull-based feeds suffice.)
- Should ActivityPub be required, or is a Civic Federation Protocol sufficient?

## 12.5 Space Type Transitions

- What are the canonical semantics for a space transitioning between types within a scope (e.g., a candidate space after an election)? Which records carry over, and under whose control?

## 12.6 Standards Alignment

- Should this align formally with existing standards bodies (e.g., W3C)?
- Should a Civic Standards Working Group be formed under nonprofit governance?

---

End of Draft v0.2
