---
status: stable
last-reviewed: 2026-04-19
owners: [adam]
version: 0.1
---

# Interoperable Civic Hub Specification (ICHS)

## Draft v0.1

## Executive Summary

The Interoperable Civic Hub Specification (ICHS) proposes a standards-based architecture for autonomous civic communities to operate online without dependence on a single software platform or identity provider. The specification defines how community hubs, civic processes, identity systems, and data structures can interoperate while preserving both community governance autonomy and individual identity sovereignty.

ICHS introduces a layered architecture combining decentralized identity (DIDs and Verifiable Credentials), canonical civic data objects, modular civic process plugins, and open federation protocols. Together, these components enable communities to deliberate, propose, vote, organize, and coordinate while maintaining portability of their governance history and social relationships.

A core goal of the specification is **portability of civic communities across hub engines**. Communities must be able to migrate their full community data, relationships, governance history, and operational configuration from one Civic Hub software implementation to another without losing structure, participation records, or social graph continuity. This ensures that no software vendor can lock a community into a single platform and that the ecosystem can support multiple interoperable implementations.

Another key objective is to support **multiple interoperable hub engines**. Existing community platform projects—such as Bonfire Networks, Social Roots, Roundabout, and other civic or community software—could potentially implement this specification so that communities using different platforms can still interoperate and migrate between them. By enabling multiple compatible implementations, the ecosystem can avoid vendor lock‑in while encouraging innovation among hub providers.

This draft (v0.1) focuses on defining the foundational architecture and canonical data models needed for interoperability. It is intended as an early working document to solicit feedback from civic technologists, governance researchers, standards bodies, and software developers.

# Specification Status

This document represents **Draft Version 0.1** of the Interoperable Civic Hub Specification (ICHS).

The purpose of this early draft is to:

- Present a coherent technical architecture for interoperable civic community platforms
- Demonstrate how decentralized identity, civic processes, and community software can interoperate
- Invite feedback from civic technologists, governance researchers, open‑standards communities, and software builders
- Identify gaps, edge cases, and implementation challenges before formal standardization

This document should be considered **exploratory and collaborative** rather than finalized.

Version 0.1 focuses on defining:

- Core architectural layers
- Canonical civic objects
- Identity integration requirements
- Federation and event models
- Portability and migration guarantees

Future versions may refine or expand:

- Civic process schemas
- Federation protocols
- governance interoperability
- identity provider portability
- reputation and moderation portability

The long‑term goal is to support an **ecosystem of interoperable civic platforms** rather than a single centralized application.

Feedback from implementers, civic organizations, and standards bodies is strongly encouraged.

---

# Table of Contents

1. Specification Status
2. Purpose & Scope
   - 1.1 Purpose
   - 1.2 Scope
   - 1.3 Definition of Civic Hub
3. Architectural Overview
   - 2.1 Design Principles
   - 2.2 Layered Architecture
4. Identity Layer Specification
   - 3.0 Identity Architecture Model
   - 3.1 Core Requirements
   - 3.2 Authentication
   - 3.3 Credential Types
   - 3.4 Identity Portability
5. Canonical Civic Object Model
   - 4.1 Canonical Civic Primitives
   - 4.2 Person
   - 4.3 Organization
   - 4.4 CivicHub
   - 4.5 Membership
   - 4.6 Subscription / Following
   - 4.7 Access Control Model
   - 4.8 Post / DeliberationThread
   - 4.9 Proposal
   - 4.10 Vote
   - 4.11 CivicProcess
   - 4.12 Badge / Credential Object
   - 4.13 Immutability & Versioning
   - 4.14 Object Integrity Requirements
6. Federation & Interoperability Layer
7. Plugin Architecture Requirements
8. Portability & Migration Specification
9. Public vs Private Hub Requirements
10. Specification Stewardship & Versioning
11. Open Design Questions

---

---

# 1. Purpose & Scope

## 1.1 Purpose

The purpose of this specification is to establish a standards-based foundation for autonomous Civic Hubs — community-governed digital spaces that retain sovereignty over their governance, data, and identity integration.

This specification exists to ensure that:

- Civic communities govern their own spaces (local, jurisdictional, or issue-based).
- No software vendor can trap a community through technical lock-in.
- Individuals can use a decentralized Civic ID to access any compliant hub.
- Communities can migrate between hub engines without loss of structure, history, or legitimacy.
- Independent hub engines can interoperate through open protocols.

Autonomy is the primary design objective.

Autonomy applies at two levels:

1. **Community Autonomy** — Each Civic Hub governs its own moderation policies, civic processes, plugin configuration, and data stewardship.
2. **Individual Identity Autonomy** — Individuals authenticate using decentralized identity standards (DIDs + Verifiable Credentials) rather than server-bound accounts.

This specification defines the minimum technical requirements necessary to preserve both forms of autonomy while enabling network-level interoperability.

---

## 1.2 Scope

This specification defines:

- Identity integration requirements (DID / VC support)
- Canonical civic object structures
- Federation and event interoperability requirements
- Plugin architecture constraints
- Portability and migration requirements
- Minimum export/import guarantees

This specification does NOT:

- Mandate a specific hub engine implementation
- Mandate a specific user interface
- Define governance rules for individual communities
- Require centralization of moderation or authority

This is a protocol-layer and data-layer specification — not a product definition.

---

## 1.3 Definition of Civic Hub

A Civic Hub is defined as a digitally hosted community space that:

- Represents a jurisdiction, institution, or civic community
- Hosts deliberation, proposals, voting, assemblies, or other civic processes
- Integrates decentralized identity for authentication
- Can export its full governance and social history in structured form
- Can interoperate with other hubs via open standards

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
- Communities retain control over their community data and governance history.
- Open standards over proprietary APIs.
- Integration without dependency.
- Local community autonomy with network-level interoperability.

## 2.2 Layered Architecture

The ecosystem consists of the following layers:

1. Identity Layer (DID / VC / Authentication)
2. Hub Engine Layer (Community software implementation)
3. Civic Object Layer (Canonical data primitives)
4. Plugin Layer (Modular civic processes)
5. Federation Layer (Cross-hub communication)
6. Feed & Discovery Layer (Aggregation and awareness)
7. Portability Layer (Export / Import)

Each layer must be separable and replaceable.

---

# 3. Identity Layer Specification

## 3.0 Identity Architecture Model

This specification adopts a **Managed Sovereign Identity Model**.

The objective is to preserve individual identity sovereignty without requiring every user to self-custody cryptographic keys.

Key principles:

- Identity is based on Decentralized Identifiers (DIDs).
- Verifiable Credentials (VCs) are portable across hubs.
- Identity must not be permanently bound to any single hub engine.

However:

- Key custody MAY be managed by a trusted civic identity provider (managed wallet model).
- Self-custody MUST remain an available option.
- Users MUST be able to export their identity keys and credentials.

This enables:

- Practical usability (no mass key-loss problem)
- Reduced onboarding friction
- Eventual migration toward full self-sovereign custody
- Portability across hub engines

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
- Social graph relationships are associated with the user's Civic Identity account, not owned by any individual hub engine.

This means:

- Relationship data (follows, memberships, delegations, endorsements, reputation signals) is logically tied to the user's identity record within the Civic Identity service layer.
- Hub engines may cache or index relationship data for performance, but they must not treat relationship data as proprietary or exclusive.
- Users must be able to export their full social graph in structured form.
- If a hub engine is replaced, the user's relationships remain intact and re-bind to the new hub instance.

Operationally, the Civic Identity service layer MUST:

- Maintain DID records
- Maintain credential references
- Maintain user-associated social graph metadata
- Expose portable, standards-based APIs for relationship retrieval
- Provide full identity + graph export in structured format

This preserves:

- Community autonomy at the hub layer
- Individual portability at the identity layer
- Migration without relational amnesia

---

## 3.1 Core Requirements

A compliant Civic Hub must:

- Support Decentralized Identifiers (DIDs)
- Support Verifiable Credentials (VCs)
- Support selective disclosure
- Support credential revocation checks
- Not bind user identity to a server-based account model

## 3.2 Authentication

- OIDC for Verifiable Credentials (OIDC4VC)
- SIOPv2 compatible
- Wallet-based authentication supported

## 3.3 Credential Types (Initial Registry)

- Resident credential (jurisdiction-scoped)
- Organizational credential
- Moderator credential
- Elected official credential
- Civic process facilitator credential

## 3.4 Identity Portability

Users must retain:

- Their DID
- Their credentials
- Their attestations

Hub engines may store role mappings locally but must not control identity issuance.

---

# 4. Canonical Civic Object Model

The Civic Object Model defines the canonical data primitives used across interoperable Civic Hubs.

The design philosophy is:

1. **Reuse existing standards wherever possible** (especially Schema.org and ActivityStreams).
2. **Extend existing vocabularies only where civic functionality requires it.**
3. **Align with emerging civic process ontologies**, including the work of the civic technology research community such as the ontology efforts associated with MetaGov.

The goal is not to invent a new web ontology but to define a **minimal civic extension layer** that allows civic platforms to interoperate while remaining compatible with the broader semantic web.

All civic objects SHOULD be represented using **JSON‑LD** and SHOULD reference existing vocabularies when applicable.

Recommended contexts:

- Schema.org (general web entities)
- ActivityStreams (social and activity events)
- Civic extension vocabulary (for governance‑specific objects)

Example JSON‑LD context pattern:

```
{
  "@context": [
    "https://schema.org",
    "https://www.w3.org/ns/activitystreams",
    "https://civicsocial.org/ns/civic"
  ]
}
```

---

## 4.1 Canonical Civic Primitives

This specification defines a **minimal interoperable set of civic primitives**. Additional civic processes MAY extend these primitives but MUST remain compatible with them.

Core primitives:

- Person
- Organization
- CivicHub
- Membership
- Post / DeliberationThread
- Proposal
- Vote
- Delegation
- CivicProcess
- CivicEvent
- Badge / Credential
- Petition
- Assembly

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

## 4.4 CivicHub

Represents an **interoperable community space capable of hosting civic processes**.

A CivicHub is a general-purpose community container that may represent many types of social or institutional groups.

Examples include:

- jurisdictional governments (town, county, state)
- nonprofits and civic organizations
- advocacy groups
- coalitions and civic networks
- clubs and associations (e.g., Rotary Clubs)
- religious or cultural communities
- issue-based communities

Suggested base: `schema:Organization` or `schema:WebSite` with civic extensions.

Fields:

- hub identifier
- hub name
- jurisdiction or community scope
- governance configuration
- plugin registry
- moderation policy reference
- access control policy

A CivicHub MAY enable civic processes through plugins, but civic processes are not required for hub compliance.

---

## 4.5 Membership

Represents a user's **formal participation** within a CivicHub.

Membership is distinct from subscription or following.

Membership typically grants additional permissions such as the ability to post, participate in governance processes, or access restricted information.

Fields:

- person DID
- hub identifier
- membership role
- join timestamp
- membership status

Membership SHOULD be represented using relationships between `Person` and `CivicHub`.

---

## 4.6 Subscription / Following

Represents a user's **non-member relationship** with a hub.

Users MAY follow or subscribe to hubs without becoming members. This allows users to observe activity, learn from other communities, or discover civic processes without participating directly.

Fields:

- follower DID
- hub identifier
- subscription timestamp
- notification preferences

Subscriptions typically grant **read access** but not **write access**.

---

## 4.7 Access Control Model

Civic Hubs MUST support **fine-grained read and write permissions**.

Hub administrators SHOULD be able to configure policies governing:

- who can read content
- who can write or post content
- who can participate in civic processes
- who can view sensitive governance information

Common access patterns include:

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
- access policies be exportable during migration
- hub engines support both membership and subscription models.

---

## 4.8 Post / DeliberationThread

Represents a user's participation within a hub.

Fields:

- person DID
- hub identifier
- membership role
- join timestamp

Membership SHOULD be represented using relationships between `Person` and `CivicHub`.

---

## 4.6 Post / DeliberationThread

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

The **Proposal** object is the central governance primitive used to represent a policy proposal, initiative, decision request, or civic action within a CivicHub.

Design goals:

- Interoperable across civic platforms (e.g., deliberation platforms, participatory budgeting tools, petition systems).
- Compatible with the semantic web.
- Alignable with emerging civic governance ontologies (such as MetaGov research).
- Compatible with ActivityStreams-style event publication.

### Base Vocabulary Reuse

The Proposal object SHOULD reuse existing semantic standards where possible.

Suggested base types:

- `schema:CreativeWork`
- `schema:Action` (for procedural context)
- ActivityStreams activity objects for event publication

JSON‑LD context SHOULD include:

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
- associated CivicHub
- proposal status

Optional but recommended fields:

- jurisdiction
- associated CivicProcess
- linked deliberation thread
- supporting documents
- tags or topics

Example conceptual structure:

{ "@type": "civic\:Proposal", "id": "proposal:123", "name": "Ban chokeholds by police", "creator": "did\:example:12345", "dateCreated": "2026-03-05T12:00:00Z", "status": "active", "hub": "hub\:athens", "process": "process\:policy\_vote", "discussion": "thread:938" }

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

### Event Publication

Proposal lifecycle events SHOULD be published through the federation layer.

Example events:

- proposal created
- proposal updated
- proposal opened for voting
- proposal closed

These events SHOULD be compatible with ActivityStreams-style activity objects.

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
- voter DID
- vote value
- timestamp
- vote method

Optional fields:

- delegation reference
- weighting factor
- cryptographic proof

Votes SHOULD be cryptographically verifiable when used in binding civic processes.

---

### 4.10.1 Supported Voting Methods

To ensure interoperability across civic platforms, the specification defines a **canonical set of voting method identifiers**.

Examples include:

- yes\_no
- approval
- ranked\_choice
- score
- quadratic
- delegated
- consensus\_signal

Additional methods MAY be defined by plugins or extensions but MUST map to a canonical method for export and migration.

The voting method is typically defined at the **CivicProcess level**, not the Vote object itself.

---

## 4.11 CivicProcess

The **CivicProcess** object represents the procedural framework that governs how a community evaluates proposals or conducts collective decision‑making.

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
- ActivityStreams for publishing lifecycle events

Where governance‑specific concepts are required, the civic namespace MAY extend these vocabularies.

### Core Fields

Required fields:

- process ID
- process type
- associated hub
- start timestamp
- end timestamp
- voting method

Optional fields:

- quorum rule
- eligibility rules
- credential requirements
- process stages
- associated proposal IDs

### Example Conceptual Structure

{ "@type": "civic\:CivicProcess", "id": "process\:athens-budget-2026", "processType": "participatory\_budgeting", "hub": "hub\:athens", "startTime": "2026-05-01", "endTime": "2026-06-01", "voteMethod": "approval", "quorum": "0.2" }

---

## 4.12 Badge / Credential Object

Represents civic recognition or verification.

Fields:

- issuer DID
- recipient DID
- criteria reference
- status (active / revoked)

These objects SHOULD be compatible with Verifiable Credential standards.

---

## 4.13 Immutability & Versioning

Civic governance records require integrity guarantees.

Rules:

- Proposal creation events SHOULD be immutable.
- Votes MUST be append‑only records.
- Revision history MUST be preserved for editable objects.

---

## 4.14 Object Integrity Requirements

All canonical civic objects must be:

- Versioned
- Timestamped
- Referenced by stable identifiers
- Exportable in structured format
- Compatible with JSON‑LD serialization

Cryptographic verification SHOULD be used where civic legitimacy requires it.

---

# 5. Federation & Interoperability Layer

## 5.1 Federation Protocol

Compliant hubs must support at least one open federation protocol:

- ActivityPub
- Or a Civic Federation Protocol compliant with this spec

## 5.2 Event Schema

Hubs must emit standardized Civic Event objects for:

- Votes
- Assemblies
- Town halls
- Participatory budgeting
- Petitions
- Badge issuance
- Legislative proposals

Events must include:

- Unique ID
- Source hub
- Jurisdiction
- Timestamp
- Required credentials (if gated)
- Deep link to source
- Signature

## 5.3 Cross-Hub Subscriptions

Hubs must allow:

- Subscription to other hubs
- Verification of remote signatures
- Display of remote civic objects in local feed

---

# 6. Plugin Architecture Requirements

## 6.1 Modular Design

Hub engines must support modular civic process plugins.

Examples:

- Citizen assembly module
- Participatory budgeting module
- Advisory vote module
- Digital town hall module
- Petition module

## 6.2 Constraints

Plugins must:

- Use canonical civic objects
- Not redefine identity primitives
- Not create proprietary data structures incompatible with export
- Declare their schema extensions

---

# 7. Portability & Migration Specification

Portability is a first-class requirement of this specification.

A compliant Civic Hub ecosystem MUST support full community migration with structural, relational, and process continuity.

Migration completeness level: **C — Data + Identity + Social Graph + Active Process State Continuity**.

---

## 7.1 Migration Scope Definition

A complete migration MUST preserve:

1. Identity continuity (DID references remain intact)
2. Social graph continuity (relationships re-bind)
3. Governance history (immutable records preserved)
4. Active process state (open proposals, live votes, delegations)
5. Plugin configuration and state
6. Badge status and credential references

Migration must not reduce a live civic system to a static archive.

---

## 7.2 Full Community Export

Each hub MUST provide deterministic export functionality including:

- Hub metadata
- Identity references (DIDs only; no private keys)
- Membership lists
- Social graph edges
- Role mappings
- All civic objects (posts, proposals, votes, delegations, badges)
- Active process state (open voting windows, thresholds, quorum rules)
- Plugin configuration and plugin state
- Moderation configuration (rules, roles, policies — but not necessarily sanctions portability)

Export Format Requirements:

- JSON-LD or compatible structured format
- Canonical deterministic ordering
- Explicit schema version tag
- Hash-signed archive
- Machine-validated against published schema

---

## 7.3 Active State Preservation

The export MUST capture dynamic process state, including:

- Open proposal status
- Vote windows (start/end timestamps)
- Current vote tallies
- Delegation chains
- Quorum thresholds
- Rule configurations

Upon import, the receiving hub engine MUST:

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

---

## 7.4 Identity & Social Graph Rebinding

Upon migration:

- User DIDs MUST remain unchanged.
- Social graph relationships MUST re-bind to the new hub instance.
- Cached hub-local relationship data MUST reconcile against identity-layer graph records.

The migration process MUST NOT require users to re-register.

---

## 7.5 Non-Loss Guarantee

Migration MUST NOT result in loss of:

- Governance history
- Vote records
- Delegation chains
- Badge issuance records
- Identity references
- Plugin configuration metadata

If full runtime continuity is not technically possible due to engine incompatibility, data integrity MUST still be preserved in canonical form.

---

## 7.6 Migration Windows & Operational Constraints

This specification does NOT require real-time migration during active high-stakes civic processes.

To reduce unnecessary technical complexity, the following model is adopted:

### 7.6.1 Planned Migration Window

- Migration SHOULD occur during a declared maintenance window.
- Active high-stakes processes (e.g., binding votes, participatory budgeting allocations) SHOULD be completed or formally paused prior to migration.
- Communities MUST have the ability to announce and schedule migration events.

### 7.6.2 Snapshot-Based Migration

Migration SHALL operate on a deterministic snapshot model:

- A snapshot timestamp is defined.
- All civic objects and process state up to that timestamp are exported.
- The receiving engine reconstructs the hub at that snapshot state.

This avoids requiring continuous cross-engine state synchronization.

### 7.6.3 Emergency Recovery Scenario

In the event of catastrophic hub failure:

- Administrators MAY restore from the most recent verified snapshot.
- Restoration to a previous state SHALL be considered acceptable under this specification.
- Minor temporal gaps between last activity and snapshot time are operational risks, not specification failures.

### 7.6.4 Optional Advanced Continuity

Future versions MAY define advanced live-migration or state-streaming capabilities, but such functionality is NOT required for baseline compliance.

This approach prioritizes:

- Simplicity
- Predictability
- Administrative control
- Reduced engineering burden

while preserving full structural and historical portability guarantees.

---

# 8. Public vs Private Hub Requirements Public vs Private Hub Requirements

## 8.1 Public Civic Hub

- Jurisdiction-scoped
- Verified resident participation
- Transparent archives
- Public event feed

## 8.2 Private Civic Hub

- Credential-gated access
- Optional encrypted data layer
- Optional suppression of public feed publication

Both must comply with identity, object, and portability layers.

---

# 9. Specification Stewardship & Versioning

This document is a technical specification.

The purpose of this section is not to define institutional governance, but to ensure technical continuity, version control, and long-term maintainability.

## 9.1 Reference Implementation

A reference implementation of this specification MAY be developed and maintained by a single coordinating entity during early phases.

However:

- The specification MUST remain implementation-agnostic.
- No clause may require dependency on a specific organization.
- All interfaces must be standards-based and publicly documented.

## 9.2 Versioning

- The specification must follow semantic versioning.
- Breaking changes must be clearly documented.
- Export/import formats must be version-stamped.
- Backward compatibility guarantees should be explicitly defined in future versions.

## 9.3 Extensibility

- Extensions must not break canonical civic primitives.
- Plugins must declare schema extensions.
- Future identity, federation, or portability layers must remain backward compatible wherever feasible.

---

# 10. Open Design Questions

## 10.1 Identity Future-Proofing

- How do we design for DID migration between identity providers?
- Should user DIDs remain stable when migrating providers, or should credentials be re-issued under new DIDs?
- Should the Civic Identity steward anchor DIDs to a specific DID method, or support multiple DID methods from inception?
- What mechanisms enable DID method migration in the future (e.g., DID rotation, key rotation, controller updates)?
- How can we ensure long-term portability if a DID method becomes obsolete or politically compromised?
- Should we design explicit identity provider portability protocols in v1, or treat that as a future version?

(These questions may require consultation with standards leaders such as Manu Sporny and other DID/VC architects.)

## 10.2 Moderation & Governance Portability

- Should moderation decisions be portable across hubs?
- Should bans follow a DID across hubs, or remain local to each community?

## 10.3 Reputation Model

- How should reputation function across hubs?
- Is reputation portable, contextual, or hub-scoped?

## 10.4 Federation Model

- Is federation mandatory or optional for compliance?
- Should ActivityPub be required, or is a Civic Federation Protocol sufficient?

## 10.5 Standards Alignment

- Should this align formally with existing standards bodies (e.g., W3C)?
- Should a Civic Standards Working Group be formed under nonprofit governance?

---

End of Draft v0.1
