---
status: review
last-reviewed: 2026-07-26
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

> **Which horizon is this?** This document specifies what can be built and verified **today**; where it defers a capability, limits a guarantee, or declines to require something the wider Civic.Social material describes — federation, live-state portability, machine-checkable conformance — that is a schedule, not a retreat. **[Civic.Social Horizons](../canon/phasing.md)** maps each of those deferrals to the pilot that closes it and to the destination it is headed toward.

---

## Notation and Conformance Language

**Notation.** The key words MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, SHOULD NOT, RECOMMENDED, MAY, and OPTIONAL in this document are to be interpreted as described in RFC 2119 and RFC 8174 when, and only when, they appear in all capitals. The same words in lower case carry their ordinary English meaning and impose no requirement.

---

# 0. Conformance at a Glance

Conformance here is not a single grade. A civic space can be excellent at one thing and not yet doing another, and collapsing that into one pass/fail would either lock out early implementations or let weak ones claim too much. So this document measures three separate things: whether the space speaks the shared interface, how much of itself it can hand back to its holder if they decide to leave, and how strongly it checks who its participants are. A space states where it stands on each. All three are expected to reach the target state — the axes exist to show progress honestly, not to make any of them permanently optional.

This document defines conformance along **three orthogonal axes**, not one ladder. A space can be strong on one axis and weak on another: a space with excellent portability may still authenticate nobody, and a space with full DID identity may export nothing. Read this table first — the rest of the document fills it in.

| Axis | Values | What it measures | Defined in |
|---|---|---|---|
| **API Profile** | *compliant* / *not compliant* | Whether the space serves the minimal implementable interface: the discovery manifest, the process endpoints, the action endpoint, and the activity feed — with every activity emitted through a single emission path | Section 7; the checklist is 7.6 |
| **Portability Level** | **A** — Archival · **B** — Continuity · **C** — Live-state | How much of a space survives migration to an independent engine: a verifiable archive (A), plus identity re-binding and closed-process integrity (B), plus live process state (C) | Section 9.1; validated by the round-trip test in 9.10 |
| **Identity Assurance** | the assurance level the space declares in its identity policy — `none`, email verification, `proof_of_personhood`, residency credentials, role credentials | How strongly the space verifies who a participant is. A stub identity adapter at a declared low level is conformant during this phase; full DID and credential verification is the target state | Sections 3.1 and 7.3; the value vocabulary is the Identity Policy Object, Civic Identity Specification §8 |

**How to state a claim.** A conformance claim MUST name a value on each of the three axes — for example, *"API Profile compliant, Portability Level A, assurance `proof_of_personhood`."* A claim that names only one axis says nothing about the other two and MUST NOT be read as implying them.

**What "conformant" alone means.** Where this document, or an implementer, says **"conformant Civic Space"** with no qualifier, it means **API Profile compliant at Portability Level A**. That is the floor: a space that speaks the interface, and that can hand its holder a complete, verifiable, importable copy of everything it stewards. Everything above the floor — Portability Level B or C, full DID identity, federation — is either named explicitly or is not being claimed.

**Target state versus today.** These requirements describe the target state. Reference implementations converge on it incrementally through the pilot program; the axes exist so that convergence can be stated as a fact rather than a mood (see Specification Status, below).

---

# How to Read This Specification

- **Implementing a space?** Read **Section 0** (what you will be claiming), then **Section 7** — the Space API Profile, which is the shortest complete statement of what you must build — then **Section 9.3** for what your export must contain at the level you are targeting. Sections 4 and 5 tell you what goes inside the payloads.
- **Evaluating adoption?** The Executive Summary, **Section 0**, and **Section 9**. Portability is the guarantee that makes leaving possible, and is therefore the guarantee worth checking first.
- **Building a new space type?** **Section 1.4** (how a scope and type register — a conformant new type requires no change to this document), then **Section 9.4** for the portability profile your scope owes.
- **Looking for rationale?** Section 1.4 on why scope is mandatory, Section 1.5 on infrastructure roles, and Section 2.2 on the layered architecture. Passages marked *rationale, not requirement* explain why the design is shaped as it is and impose no obligations.
- **Looking for a term you have not met yet?** The box after Section 1.2 defines the foundation nouns this document uses; the Civic.Social Terminology Glossary is the canonical reference for the ecosystem's vocabulary.

---

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

**A note on conformance phasing.** Specifications in this ecosystem define the *target state*. Reference implementations converge on them incrementally through the pilot program; each pilot brings the running software up to a further slice of this contract. Non-conformance of an early implementation is a sequencing fact, not a specification failure — the portability conformance ladder (Section 9.1) and the API Profile requirements (Section 7.6) exist precisely to make that convergence measurable. Section 0 shows how the two, together with declared identity assurance, compose into a single conformance claim.

Feedback from implementers, civic organizations, and standards bodies is strongly encouraged.

---

# Table of Contents

- **Executive Summary**
- Notation and Conformance Language
- **0. Conformance at a Glance**
- How to Read This Specification
- Specification Status
- **1. Purpose & Scope**
  - 1.1 Purpose
  - 1.2 Scope
  - Terms Used in This Document
  - 1.3 Definition of Civic Space
  - 1.4 Space Scopes and Space Types
  - 1.5 Infrastructure Roles (Not Spaces)
  - 1.6 The Civic Hub (Community-Scoped Space)
- **2. Architectural Overview**
  - 2.1 Design Principles
  - 2.2 Position in the Layered Architecture
- **3. Identity Layer Specification**
  - 3.0 Identity Architecture Model
  - 3.1 Core Requirements
  - 3.2 Authentication
  - 3.3 Credential Types (Initial Registry)
  - 3.4 Identity Portability
  - 3.5 Space Identity (Space DIDs)
- **4. Canonical Civic Object Model**
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
- **5. Federation & Interoperability Layer**
  - 5.1 Federation Protocol
  - 5.2 Activity Schema
  - 5.3 Cross-Space Subscriptions
- **6. Plugin Architecture Requirements**
  - 6.1 Modular Design
  - 6.2 Constraints
- **7. Space API Profile**
  - 7.1 Responsibilities
  - 7.2 Required API Endpoints (authentication; manifest; process; action; feed)
  - 7.3 Identity Integration
  - 7.4 Process Support
  - 7.5 Data Storage
  - 7.6 API Profile Minimal Compliance
- **9. Portability & Migration Specification**
  - 9.1 Migration Conformance Ladder
  - 9.2 Migration Scope Definition
  - 9.3 Full Space Export
  - 9.4 Per-Scope Portability Profiles
  - 9.5 Active State Preservation (Level C)
  - 9.6 Identity & Social Graph Rebinding
  - 9.7 Non-Loss Guarantee
  - 9.8 Migration Windows & Operational Constraints
  - 9.9 Identity & Discovery Continuity Across Migration
  - 9.10 Portability Conformance
- **10. Public vs Private Space Requirements**
  - 10.1 Public Civic Space
  - 10.2 Private Civic Space
  - 10.3 Feed Exposure in Private Spaces
- **11. Specification Stewardship & Versioning**
- **12. Open Design Questions**
- **References**

*Section 8 is deliberately not used. Earlier drafts reserved it for discovery requirements, which are specified in 7.2.0 (the discovery manifest) and in the Discovery Layer Specification. Sections 9 through 12 keep their existing numbers so that citations made against Draft v0.2 remain valid.*

---

# 1. Purpose & Scope

## 1.1 Purpose

The purpose of this specification is to establish a standards-based foundation for autonomous Civic Spaces — scoped digital environments that retain sovereignty over their governance, data, and identity integration.

This specification exists to ensure that:

- Civic communities govern their own spaces (local, jurisdictional, or issue-based).
- Individuals and accountable public entities have spaces at their own scope on the same protocol.
- No software vendor can trap a community, an individual, or an entity through technical lock-in.
- Individuals can use a decentralized Civic Identity to access any compliant space.
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

## Terms Used in This Document

Seven foundation nouns appear below before they are fully defined, and one of them — assurance level — carries a requirement. One line each; the normative definition is where noted. The Civic.Social Terminology Glossary is the canonical reference for the rest of the ecosystem's vocabulary.

| Term | Meaning here |
|---|---|
| **Community Node** | The sovereign identity anchor of a community: a collective DID plus its credentials. Normative: Civic Identity Specification §5. |
| **Citizen Node** | The sovereign identity anchor of a person: a DID plus verifiable credentials held in a wallet. Normative: Civic Identity Specification §2. |
| **Entity Node** | The sovereign identity anchor of an accountable public role — an elected official, a candidate, an institutional body: a DID plus role credentials. Normative: Civic Identity Specification §5. |
| **Community / Personal / Entity Data Store** | The data store paired with each Node, holding the relationships, memberships, preferences, and participation references its holder owns — separate from identity, and separate from any application. The Personal Data Store (PDS) is specified in Civic Identity Specification §3; the community and entity stores are the same shape at their scope. |
| **Single emission path** | One chokepoint inside a space through which every Civic Activity flows and is validated, so that no observable state change can happen silently or bypass the feed. Normative: Civic Activity Specification §10. |
| **Identity adapter** | The replaceable component through which a space obtains the authenticated participant's identifier. Keeping it replaceable is what lets a space start on stub identity and upgrade to full DID authentication without being rebuilt (3.1, 7.3). |
| **Assurance level** | How strongly a participant's identity has been verified. The value vocabulary is the `assurance_requirements` dimension of the **Identity Policy Object** (Civic Identity Specification §8), which runs `none` → email verification → `proof_of_personhood` → residency credentials such as `resident.city` → role credentials. Where this document requires a space to *declare its assurance level*, it means: publish a value from that vocabulary in the space's identity policy. A space that verifies nothing declares `none` rather than omitting the declaration. |

---

## 1.3 Definition of Civic Space

A **Civic Space** is a scoped host environment in the Civic.Social ecosystem: a network-addressable application that

- (a) is anchored to a **sovereign identity-and-data foundation** appropriate to its scope (see 1.4),
- (b) **hosts Civic Processes** through the plugin contract defined by the Civic Process Specification and the Civic Plugin Architecture,
- (c) **emits and consumes Civic Activities** through a single emission path, per the Civic Activity Specification,
- (d) **publishes a discovery manifest** (see Section 7.2.0 and the Discovery Layer Specification),
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

**Exporter obligations follow scope.** Because a space's portability profile is determined by its scope (9.4), an exporter MUST read the space's declared scope before producing an export and MUST apply that scope's profile. A community export is a full tenant migration. An individual export carries **pointers into other spaces, not copies of their content** — copying would exfiltrate other participants' contributions through a personal export. An entity export MUST preserve third-party accountability data with its provenance intact, so that a record cannot be laundered by migration. An exporter facing an unscoped space cannot know which obligation applies, and either wrong guess causes real harm.

*Rationale, not requirement.* **Why scope is mandatory (design rationale).** Conformance establishes that a space speaks the protocol; scope establishes **who holds it** — and three contracts are undecidable without that answer. First, the sovereign anchor: every space binds to a holder's Node and Data Store, and the binding cannot be inferred from behavior. Second, the membership and relationship model (4.5–4.6). Third, and most consequentially, the portability profile (9.4), whose exporter obligations are stated normatively just above. Scope also protects the open type set: because consumers will encounter space types they have never seen, behavior must key off a coarse classifier rather than the type itself — a feed, indexer, or migration tool that has never met a new community-scoped type still handles it correctly the moment it reads `scope: community`. (This is the same mechanism that lets ActivityPub consumers handle unfamiliar actors via the actor type.) The cost is a single required field. Note the distinction between software and instance: a space *engine* may be scope-generic, but a deployed *instance* has an actual holder and actual data, and therefore declares exactly one scope.

**Scope stability and type transitions.** A space's scope is stable for its lifetime. A space's *type within a scope* MAY transition where the type's own specification defines it (for example, a candidate's entity-scoped space transitioning after an election). Type-transition semantics are an open design area (Section 12) and are defined by space-type specifications, not here.

**Entity scope naming.** The entity scope covers elected officials, candidates, and institutional governance bodies, and generalizes to any role in which a person or collective body holds accountable leadership over a constituency. Earlier documents used "office-scoped" or "public-office-scoped"; **`entity`** is the canonical scope name.

---

## 1.5 Infrastructure Roles (Not Spaces)

Infrastructure roles are ecosystem participants that serve the foundation rather than hosting processes at a scope. They are **service providers, not Civic Spaces**. The current specifications define contracts for two, both governed by the Civic Identity Specification:

- **Citizen Account Provider** — hosts a citizen's foundation (Citizen Node and Personal Data Store) on the citizen's behalf; federated, like an email provider for civic identity. Citizens can migrate between providers without losing credentials or relationships.
- **Badge / Credential Issuer** — a third-party organization issuing verifiable credentials into the ecosystem (pledges, endorsements, attestations of office, participation records).

**The role set is open.** Further infrastructure roles are expected as the ecosystem grows — for example, a **Space Hosting Provider** that operates Civic Spaces on behalf of their holders (the managed-hosting model described in the Assisted Creation and Managed Hosting concept document; the Section 9 portability contract is what keeps a hosted holder sovereign, because migration away is always available), or a **Discovery Index Operator** that runs a discovery index over space manifests (the Discovery Layer's reference indexer is the first instance; anyone may operate one, and no index has exclusive claim to the ecosystem's map). Plugin registry operators and hosting certifiers (Civic Plugin Architecture) follow the same pattern. A new role enters the taxonomy by defining its service contract in the relevant specification, exactly as the provider and issuer contracts live in the identity layer.

*This passage is rationale, not requirement.* Keeping these out of the Space taxonomy keeps the primitive crisp and keeps their obligations where they belong: in the service contracts of the layer each role serves.

---

## 1.6 The Civic Hub (Community-Scoped Space)

A Civic Hub is a community-scoped Civic Space: a digitally hosted community space that

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

All compliant hubs MUST satisfy the autonomy and portability guarantees defined in this document.

---

# 2. Architectural Overview

## 2.1 Design Principles

- Participants — persons, entities, and communities alike — control their own digital identity through decentralized identity standards (DIDs and Verifiable Credentials), rather than identities being owned by any single platform.
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
- Identity MUST NOT be permanently bound to any single space engine.

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
- Space engines MAY cache or index relationship data for performance, but they MUST NOT treat relationship data as proprietary or exclusive.
- Users MUST be able to export their full social graph in structured form.
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

A compliant Civic Space (target state) MUST:

- Support Decentralized Identifiers (DIDs)
- Support Verifiable Credentials (VCs)
- Support selective disclosure
- Support credential revocation checks
- Not bind user identity to a server-based account model

The Space API Profile (Section 7) permits **stub identity at a declared assurance level** during early phases: a space MAY accept opaque user identifiers through the identity adapter seam, provided the adapter is replaceable and the space declares its assurance level in its identity policy (see the assurance-level entry in *Terms Used in This Document*). Upgrading from stub identity to full Civic Identity MUST NOT require rebuilding the space.

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

Users MUST retain:

- Their DID
- Their credentials
- Their attestations

Space engines MAY store role mappings locally but MUST NOT control identity issuance. Role and authority bindings SHOULD be keyed to the participant's identifier (DID or portable id), not to provider-specific attributes such as email addresses, so that identity-provider migration does not strand the authorization layer.

## 3.5 Space Identity (Space DIDs)

Every Civic Space MUST hold a **stable decentralized identifier — the space DID** — distinct from its serving URL. The space DID is the sovereign anchor of its scope (Community Node, Citizen Node, or Entity Node) or a DID controlled by it.

- The space's serving URL is a **current binding**, resolvable via the space DID's document (service endpoint).
- Activities emitted by the space carry the space DID in `source.space_id` (Civic Activity Specification §2); `source.hub_id`/`hub_url` remain the v0.1 compatibility serialization. Holding a space DID is required now; *transmitting* it on every activity is SHOULD in v0.1 and becomes MUST in v0.2, per that specification's field table. A space that holds a DID but omits it from emissions is conformant with this section in v0.1 and will not be in v0.2, so emitters SHOULD populate it from the start.
- Cross-space references, subscriptions, and provenance SHOULD be keyed to the space DID, because it survives migration to a new URL or engine (Section 9.9).

DID method guidance: `did:web` is acceptable for spaces expected to remain on a stable domain; spaces that anticipate domain or provider migration SHOULD prefer a method with verifiable history and controller continuity (e.g., `did:webvh`, formerly named `did:tdw`). Method selection criteria are consolidated in the Civic Identity Specification.

---

# 4. Canonical Civic Object Model

> **Read this before implementing Section 4.** The field lists throughout this section name the *concepts* every conformant object carries. They are **informative as to spelling and type, not normative**: this section does not yet fix JSON key names or value types, so two implementations can both satisfy it and still fail to interoperate — one emitting `spaceId`, another `space_id`, another `space`. Normative property names and types arrive with the civic JSON-LD context at `https://civicsocial.org/ns/civic`, which is **not yet published** (see the note at the end of this introduction). Until it publishes, implementers should follow the two subsections that already show the intended serialization concretely — **4.9 (Proposal)** and **4.12 (CivicProcess)**, each with a worked JSON-LD example. Those examples are the model: JSON-LD with a `civic:` `@type`, camelCase property names, DIDs as identifier values, RFC 3339 timestamps. Where this section and the published context eventually disagree, the context governs.

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

Required fields:

- **participant identifier** — the person's DID where they have one, or the space's opaque identifier for them where the space is running on a stub identity adapter (3.1, 7.3). A DID is the target state and the only form that is portable across spaces; an opaque identifier is scoped to this space and MUST NOT be treated as denoting the same person anywhere else.
- display name
- credential references

Additional civic properties MAY include:

- jurisdiction membership

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
  "startTime": "2026-05-01T00:00:00Z",
  "endTime": "2026-06-01T00:00:00Z",
  "voteMethod": "approval",
  "quorum": 0.2
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

All canonical civic objects MUST be:

- Versioned
- Timestamped
- Referenced by stable identifiers
- Exportable in structured format
- Compatible with JSON-LD serialization

Cryptographic verification SHOULD be used where civic legitimacy requires it.

---

# 5. Federation & Interoperability Layer

## 5.1 Federation Protocol

**Federation is OPTIONAL in v0.1.** A space that never federates can still be a conformant Civic Space (Section 0): pull-based consumption of the activity feed endpoint (7.2.4) satisfies early interoperability, and nothing in the API Profile (Section 7) requires an inbox, an outbox, or push delivery.

A space that **does** federate MUST do so over one of:

- **ActivityPub**, or
- a **Civic Federation Protocol** compliant with this specification.

The requirement is not that a space federate, but that it not invent a private protocol when it does — an open protocol on the wire is what keeps the choice to federate from becoming a choice of vendor. Whether federation becomes mandatory in a later version is an open question (12.4).

## 5.2 Activity Schema

Spaces MUST emit standardized **Civic Activities** for process lifecycle transitions and participation actions — votes, assemblies, town halls, participatory budgeting, petitions, badge issuance, legislative proposals.

The activity envelope, required fields, type registry, extension namespace, and visibility/disclosure rules are defined normatively by the **Civic Activity Specification**; this document does not define a separate event field set. Activity signing (signature by the emitting space's DID) is planned in the Civic Activity Specification's v0.2+ extensions and is required before migrated or federated history can be independently verified (see 9.9).

## 5.3 Cross-Space Subscriptions

A space that implements federation MUST allow:

- Subscription to other spaces
- Verification of remote provenance (by space DID once activity signing lands; by source URL in v0.1)
- Display of remote civic objects in the local feed

---

# 6. Plugin Architecture Requirements

Civic Plugins are the ecosystem's general packaging unit; this section states the requirements for hosting the first specified kind, the Civic Process Plugin. Future kinds — display and lens plugins in particular — reuse the same manifest and trust tiers (Civic Plugin Architecture).

## 6.1 Modular Design

Space engines MUST support modular civic process plugins.

Examples:

- Citizen assembly module
- Participatory budgeting module
- Advisory vote module
- Digital town hall module
- Petition module

The packaging, trust-tier, manifest, and capability model for plugins is defined by the **Civic Plugin Architecture**; the process contract a plugin implements is defined by the **Civic Process Specification**.

## 6.2 Constraints

Plugins MUST:

- Use canonical civic objects
- Not redefine identity primitives
- Not create proprietary data structures incompatible with export
- Declare their schema extensions
- Declare the capabilities they require and the activity types they emit (per the Civic Plugin Architecture); the space grants least privilege

---

# 7. Space API Profile

> Folded in from the standalone minimal Civic Hub API specification. This profile is the **minimal implementable interface** of a Civic Space — deliberately small, designed for rapid implementation, and the surface the reference implementation ships today. It applies to every scope; a space type MAY extend it. Conformance to this profile is **API Profile compliance** (Section 0, first axis); the full target-state contract (DID identity, portability Level B or C, federation) layers on top per the conformance phasing note in the Specification Status section.
>
> **Versioning.** This profile carries the version of this document — currently **v0.2**. Earlier drafts numbered it independently as "v0.1", which is why older references speak of the "v0.1 API profile"; they mean this profile. It is not a separately versioned artifact.

## 7.1 Responsibilities

A Civic Space MUST:

- Host Civic Processes
- Accept user actions on processes
- Emit Civic Activities for all process activity
- Expose an activity feed endpoint — always, including in private spaces, with the response filtered to what the caller may see (10.3)
- Provide basic identity integration (a stub identity adapter is allowed in this profile, at a declared assurance level)

## 7.2 Required API Endpoints

### Authentication and sessions (applies to every endpoint below)

The endpoints in this section repeatedly say that the actor is "taken from the authenticated context." This subsection says how a client gets one.

**This profile does not define its own session model.** The normative model is the cryptographic challenge-response flow of the **Civic Identity Specification §6.2**: the participant proves control of their DID by signing a challenge, the space verifies that control, the space requests any credentials the action requires, and the result is a session. The full flow typically runs once per session. A space using a stub identity adapter (7.3) runs the same shape with an opaque identifier in place of the DID, at its declared assurance level.

The **v0.1 default transport for the resulting session is a bearer token in the HTTP `Authorization` header**:

```
Authorization: Bearer <session-token>
```

Token format, lifetime, and refresh are implementation choices; this profile fixes only the header, so that an independently written client can talk to an independently written space. Rules:

- `GET /.well-known/civic.json` and `GET /health` MUST be servable without authentication.
- `POST /process/:id/action` MUST require an authenticated session. The actor recorded on the resulting activities is the authenticated identity; a space MUST reject an actor identifier supplied in the request body or query string rather than honouring it.
- `POST /process` and the read endpoints MAY require authentication, per the space's access control policy (4.7).
- A request that requires authentication and carries no valid session receives `401`. An authenticated request that fails an eligibility or credential check receives `403`. This matches the error model in Civic Process Specification §12.2, and error bodies take the same shape: `{ "status": "error", "code": "string", "message": "string" }`.

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

Served with `Content-Type: application/json`, without authentication.

Field requirements:

| Field | Requirement | Notes |
|---|---|---|
| `space.id` | MUST | The space DID (3.5) — the stable key consumers re-bind on across migration |
| `space.scope` | MUST | `community` \| `individual` \| `entity` \| a registered scope (1.4). Consumers key behaviour off this field, so it cannot be omitted |
| `space.type` | MUST | The space type identifier, e.g. `civic.hub`, `civic.dashboard`, `civic.representative_space` |
| `feeds` | MUST | Array of absolute URLs; MUST include the space's activity feed endpoint (7.2.4) |
| `name` | SHOULD | Human-readable name of the space |
| `jurisdictions` | SHOULD | Array of jurisdiction strings in the form defined by Civic Activity Specification §3.1. A space with no meaningful jurisdiction omits the field rather than sending an empty label. This is deliberately the opposite convention from an activity's singular `jurisdiction`, which is a required scalar and therefore uses the literal `"none"`: an optional array can be absent and mean "not stated", while a required scalar cannot, and a filtering consumer needs an explicit answer from it |
| `processes` | SHOULD | Enabled process types; MAY be an empty array while none are enabled |
| `contact` | MAY | Contact address for the space operator |

Consumers MUST ignore fields they do not recognise rather than rejecting the manifest, so that later versions can add fields without breaking existing indexers.

**Canonical form, and the deprecated alternative.** The nested `space` object shown above is **canonical**: producers MUST emit it. Implementations predating space DIDs instead serve a flat `"type": "hub"` at the top level of the manifest; that form is **accepted for back-compatibility and is deprecated**. Consumers MUST accept both for the life of v0.1 and MUST prefer `space.type` where both appear. The flat form is scheduled for removal in the version that makes `source.space_id` required on activities (Civic Activity Specification §13).

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

Returns the Civic Activities this space has emitted, ordered by `timestamp` descending. Served with `Content-Type: application/json`. (`GET /activities` is a planned alias; see Civic Activity Specification §14.)

Query parameters, all OPTIONAL:

| Parameter | Type | Meaning |
|---|---|---|
| `process_id` | string | Return only activities carrying this `process_id` |
| `type` | string | Return only activities whose `event_type` matches exactly, e.g. `civic.process.vote_submitted` |
| `since` | RFC 3339 timestamp | Return only activities whose `timestamp` is strictly later than this value |
| `limit` | integer | Maximum items in the response. Default 50, maximum 200. A space MAY cap lower, and MUST NOT return more items than `limit` |
| `cursor` | opaque string | Resume from a previous response's `next_cursor` |

Response envelope:

```json
{
  "items": [],
  "next_cursor": null
}
```

- `items` is an array of Civic Activities in the envelope defined normatively by the Civic Activity Specification §2–§3. This document does not restate the activity fields.
- `next_cursor` is an opaque string to send back as `cursor` to retrieve the next page, or `null` when the caller has reached the end of the feed. It MUST always be present, so that a client never has to distinguish "no more pages" from "field omitted".

Visibility filtering is normative: the response MUST carry only the activities the caller is authorised to see, per each activity's `meta.visibility` and the rule in 10.3.

### 7.2.5 Recommended Read Endpoints

- `GET /process` — list processes (UI read layer)
- `GET /health` — health check

## 7.3 Identity Integration

Minimal requirements:

- Accept a user identifier (opaque id or DID) through a **replaceable identity adapter**
- Pass identity into action execution; the actor recorded on activities is the authenticated identity
- Stub credential verification allowed, with the assurance level declared in the space's identity policy

Target state: full DID authentication and verifiable credential checks per Section 3 and the Civic Identity Specification. The adapter seam exists so this upgrade does not require rebuilding the space.

## 7.4 Process Support

The space MUST support at least one process type (recommended: `civic.vote`), registered through a modular, replaceable process interface (registry/handler pattern per the Civic Process Specification and Civic Plugin Architecture). Avoid tight coupling between the space core and specific process implementations.

## 7.5 Data Storage

Allowed implementations: in-memory store, simple database, or managed Postgres. The API profile imposes no scalability or persistence requirements; the portability contract (Section 9) and the immutability rules (4.14) impose the durability obligations that matter.

## 7.6 API Profile Minimal Compliance

To be API Profile compliant, a Civic Space MUST:

- Implement the required endpoints (7.2.0–7.2.4), and authenticate clients per the authentication subsection of 7.2
- Support at least one process type
- Emit valid Civic Activities through a single emission path
- Expose the activity feed, filtered per 10.3

Out of scope for the API profile (target-state requirements defined elsewhere in this document): UI, messaging, moderation systems, ranking, full ActivityPub inbox/outbox, real-time delivery.

---

# 9. Portability & Migration Specification

Portability is a first-class requirement of this specification.

A compliant Civic Space ecosystem MUST support migration with structural, relational, and process continuity — for communities moving between engines or providers, and for individuals moving between providers and interfaces.

## 9.1 Migration Conformance Ladder

Migration completeness is a **named conformance ladder**, not a single bar:

- **Level A — Archival.** Complete, deterministic, integrity-verified export of all civic objects, configuration, and activity history. Import produces a readable, verifiable archive space.
- **Level B — Continuity.** Level A, plus identity re-binding (DIDs intact, memberships and roles reconstructed, subscriptions re-pointed via the identity layer) and closed-process integrity (tallies, outcomes, and delegation records reconstructed and verifiable).
- **Level C — Live-state.** Level B, plus active process state (open voting windows, live tallies, delegation chains, plugin runtime state), with the plugin-unavailable fallback of 9.5.

Conformance claims MUST name their level, alongside a value on the other two conformance axes (Section 0). **The pilot-phase target is Level B**, with migration normally scheduled once active processes reach a terminal state (9.8); Level C is the v1.0 objective, gated on the plugin-state export contract. Emergency restoration from the most recent verified snapshot satisfies Level A semantics (9.8.3). What an export must actually *contain* at each level is the table in 9.3 — that table and this ladder are one artifact, not two.

**How levels are validated until canonicalization publishes.** Level A calls for a *deterministic* export, and 9.3 asks that two exports of identical state be byte-identical. That test cannot be run yet: it depends on the canonicalization profile and on the civic JSON-LD context, and neither is published (see the banner opening Section 4 and the deliverables note in 9.3). Read literally, no space could claim any level today, which would make the ladder decorative rather than strict. So, as an interim rule:

- Until both the canonicalization profile and the civic JSON-LD context are published, a claim at **any** level is validated by **semantic round-trip equality only** (9.10): export, import into an independent engine, re-export, and compare the two exports as data — same objects, same identifiers, same relationships, same values — rather than as bytes.
- **Byte equality becomes normative** in the version that publishes the canonicalization profile. Claims made under the interim rule remain valid as historical claims, but MUST be re-validated against the byte test to be carried forward.

## 9.2 Migration Scope Definition

A complete migration (Level C; Levels A/B per their definitions above) preserves:

1. Identity continuity (DID references remain intact)
2. Social graph continuity (relationships re-bind)
3. Governance history (immutable records preserved)
4. Active process state (open proposals, live votes, delegations)
5. Plugin configuration and state
6. Badge status and credential references

Migration MUST NOT reduce a live civic system to a static archive (beyond Level A claims).

## 9.3 Full Space Export

Every space MUST provide export functionality. **What the export must contain depends on the level being claimed** (9.1): the table below is the single authoritative contents list, and it governs over any reading of the ladder that would put live process state or plugin runtime state into a Level A export.

| Export item | Level A | Level B | Level C |
|---|---|---|---|
| Space metadata (space DID, scope, space type) | MUST | MUST | MUST |
| Identity references (**DIDs, or the space's opaque participant identifiers where it runs a stub identity adapter; never private keys**) | MUST | MUST | MUST |
| All civic objects (posts, proposals, votes, delegations, badges) | MUST | MUST | MUST |
| Activity history | MUST | MUST | MUST |
| Membership lists | MUST | MUST | MUST |
| Role mappings | MUST | MUST | MUST |
| Social graph edges | MUST | MUST | MUST |
| Access and moderation configuration (rules, roles, policies — sanctions portability not required) | MUST | MUST | MUST |
| Plugin configuration (which plugins, at which versions, configured how) | MUST | MUST | MUST |
| Badge status and credential references | MUST | MUST | MUST |
| Identity re-binding on import: DIDs intact, memberships and roles reconstructed, subscriptions re-pointed via the identity layer (9.6) | — | MUST | MUST |
| Closed-process integrity: tallies, outcomes, and delegation records reconstructed and independently verifiable | — | MUST | MUST |
| Active process state (open voting windows, thresholds, quorum rules, live tallies) — see 9.5 | — | — | MUST |
| Plugin runtime state, with the plugin-unavailable fallback of 9.5 | — | — | MUST |

A dash means the item is not required at that level. A space MAY export more than its level requires; it MUST NOT claim a level whose rows it does not satisfy.

**Key material, at every level.** Private keys live in wallets and identity providers. They MUST NOT appear in any space export at any level (Civic Identity Specification §11.2). A space export carries identity *references* only.

Export Format Requirements:

- JSON-LD or compatible structured format, against the published civic context (see the banner opening Section 4 — the context is not yet published, and 9.1 states the interim validation rule)
- Canonical deterministic ordering — the export format specification MUST name its canonicalization algorithm (e.g., RDF dataset canonicalization or a defined key-ordering profile) so that two exports of identical state are byte-identical
- Explicit schema version tag
- Hash-signed archive
- Machine-validated against a published schema

> The export schema and canonicalization profile are deliverables of the Civic Hubs pilot; conformance is validated by the round-trip test in 9.10.

## 9.4 Per-Scope Portability Profiles

The contract shape above is universal; **what the export contains differs by scope**:

- **Community profile (Civic Hub).** The full tenant-migration case: at Level C, 9.2's list in its entirety; at Levels A and B, the rows those levels require in 9.3. Media and attachments are exported by reference with retrieval manifests (inline where size permits).
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

A portability conformance claim is validated by **round-trip testing**: export from engine 1 → import into an independent engine (or independent instance) → re-export → compare against the original export. The comparison is **semantic equality** — same objects, identifiers, relationships, and values — and becomes byte equality once the canonicalization profile is published; 9.1 states that interim rule and when it lapses. Export-only validation does not establish conformance. The round-trip test at Level B, executed between two live engine instances with a real community's data, is the pilot-phase acceptance bar.

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
- Optional suppression of public feed publication (see 10.3 for exactly what this does and does not permit)

Both MUST comply with identity, object, and portability requirements.

## 10.3 Feed Exposure in Private Spaces

Section 7.1 requires every space to expose an activity feed endpoint; 10.2 permits a private space to suppress public feed publication. Those two statements resolve as follows, and this rule is normative.

- A space MUST serve the feed endpoint (7.2.4). Being private is not grounds for omitting it: a missing endpoint is an API Profile conformance failure regardless of the space's privacy posture.
- The response MUST be filtered to exactly the activities the calling identity is authorised to see. Authorisation is decided by each activity's `meta.visibility` — `public` or `restricted`, per Civic Activity Specification §7 — and, for `restricted` activities, by the space's single authorization seam (4.7). Process-level visibility (`public`, `participants-only`, `jurisdiction-only`) maps down onto the wire value per that same section; the finer distinction between the two restricted policies is enforced at the seam, not carried on the wire.
- An unauthenticated caller to a fully private space therefore receives `200` with an empty `items` array — **not** `401`, `403`, or `404`. An unauthorised caller and a caller for whom no activities exist MUST be indistinguishable in the response.

The reason the failure mode is an empty list rather than an error is disclosure: signalling "there is something here you may not see" is itself a disclosure, and in civic contexts the mere existence of a restricted process can reveal that a jurisdiction is deliberating something sensitive.

Accordingly, "suppression of public feed publication" in 10.2 means exactly two things: this filtering, and not pushing the feed outward to federation peers or indexers. It never means declining to serve the endpoint.

**What the manifest reveals.** A private space's discovery manifest discloses that the space exists, its scope, its type, and where its feed lives. It discloses nothing about the space's membership, its content, its processes, or its activity — a private space MAY omit the optional `name`, `jurisdictions`, and `processes` fields entirely, and its feed endpoint MAY return an empty list to unauthorized callers (10.2). The endpoint is mandatory because of re-binding: a space that cannot be resolved cannot be migrated to, and portability is what keeps a community's space its own. A community that wants no public presence at all is choosing not to be discoverable in the ecosystem, which is its right — it is then not a conformant Civic Space, and it is better to say so here than to let it be discovered at audit.

---

# 11. Specification Stewardship & Versioning

This document is a technical specification.

The purpose of this section is not to define institutional governance, but to ensure technical continuity, version control, and long-term maintainability.

## 11.1 Reference Implementation

A reference implementation of this specification MAY be developed and maintained by a single coordinating entity during early phases.

However:

- The specification MUST remain implementation-agnostic.
- No clause may require dependency on a specific organization.
- All interfaces MUST be standards-based and publicly documented.

## 11.2 Versioning

- The specification MUST follow semantic versioning.
- Breaking changes MUST be clearly documented.
- Export/import formats MUST be version-stamped.
- Backward compatibility guarantees SHOULD be explicitly defined in future versions.

## 11.3 Extensibility

- Extensions MUST NOT break canonical civic primitives.
- New space types register per 1.4 without modifying this specification.
- Plugins MUST declare schema extensions.
- Future identity, federation, or portability revisions SHOULD remain backward compatible; where a break is unavoidable it MUST be documented as a breaking change.

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

- Is federation mandatory or optional for compliance? **The v0.1 answer is settled: OPTIONAL** (5.1) — pull-based feeds suffice, and a space that does federate MUST use ActivityPub or a compliant Civic Federation Protocol. What remains open is whether a later version raises federation to a requirement, and on what schedule.
- Should ActivityPub specifically be required, or does a Civic Federation Protocol remain a permanent alternative? (v0.1 accepts either.)

## 12.5 Space Type Transitions

- What are the canonical semantics for a space transitioning between types within a scope (e.g., a candidate space after an election)? Which records carry over, and under whose control?

## 12.6 Standards Alignment

- Should this align formally with existing standards bodies (e.g., W3C)?
- Should a Civic Standards Working Group be formed under nonprofit governance?

---

# References

Every companion document this specification cites normatively is listed below with its status as of this release. **Published** means the document exists in the form cited and can be implemented against. **Planned** means this specification defers a requirement to it, but it has not been published — an implementer should treat anything deferred to a planned document as unsettled, and should not assume the shape it will take.

| Document | Cited by this specification for | Status |
|---|---|---|
| **Civic Process Specification** (v0.2) | The process model, and the request/response contracts behind 7.2.1–7.2.3 (its §12) | Published |
| **Civic Activity Specification** (v0.1) | The activity envelope and required fields, the type registry, the visibility model, and the `GET /events` transport (5.2, 7.2.4, 10.3) | Published |
| **Civic Identity Specification** (v0.1) | DIDs and credentials (Section 3), the challenge-response session model (its §6.2, used by 7.2), the Identity Policy Object and the assurance-level vocabulary (its §8) | Published |
| **Civic.Social Terminology Glossary** (v0.2) | Canonical ecosystem vocabulary, including the five-layer model (2.2) | Published |
| **Civic Plugin Architecture** | Plugin packaging, trust tiers, manifests, capability declarations, and the opt-in host-requirements mechanism (1.4, 6.1, 6.2) | Companion document; status not pinned by this release — verify before relying on it |
| **Discovery Layer Specification** | Manifest ingestion and index behaviour (1.3, 7.2.0), and the `moved` tombstone and re-binding protocol (9.9) | Companion document; status not pinned by this release — verify before relying on it |
| **Authorization Model Note** | The single authorization seam and the role-to-capability evolution path (4.7) | Companion note; status not pinned by this release — verify before relying on it |
| **Assisted Creation and Managed Hosting** | The managed-hosting model behind the Space Hosting Provider role (1.5) | Concept document, non-normative |
| **Civic extension JSON-LD context** (`https://civicsocial.org/ns/civic`) | Normative property names and types for canonical civic objects (Section 4) and for the export format (9.3) | **Planned — not yet published.** Section 4 is informative as to field names until it does |
| **Export schema and canonicalization profile** | Byte-deterministic export ordering and machine validation (9.3) | **Planned** — a Civic Hubs pilot deliverable; 9.1 states the interim validation rule |
| **Space Conformance Suite** | Machine-runnable conformance tests for this document (11.4) | **Planned** |

External standards referenced: W3C Decentralized Identifiers, W3C Verifiable Credentials Data Model, JSON-LD 1.1, ActivityStreams 2.0 and ActivityPub, OpenID4VCI / OpenID4VP / SIOPv2, Schema.org, RFC 2119, RFC 8174, and RFC 3339.

---

End of Draft v0.2
