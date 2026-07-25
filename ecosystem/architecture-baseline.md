---
status: review
last-reviewed: 2026-07-03
owners: [adam]
version: 0.4
---

# Civic.Social Architecture Baseline (v0.4 Draft)

## 1. Purpose of This Document

This document establishes the baseline conceptual architecture for the Civic.Social ecosystem. It serves as the "source of truth" describing how the major components of the system fit together and provides a stable reference for defining pilot projects.

> **Relationship to the canonical specifications.** This document is the conceptual companion to the four canonical Civic.Social specifications — **Civic Space · Civic Process · Civic Activity · Civic Identity**. Where this baseline conflicts with those specifications, the specifications govern.

The architecture described here is intentionally **conceptual rather than deeply technical**. Detailed protocol specifications, APIs, schemas, and infrastructure decisions will be developed during pilot implementations.

This document exists to ensure that all pilots remain logically consistent as Civic.Social evolves.

---

# 2. System Vision

Civic.Social aims to create **shared digital civic infrastructure** that connects citizens, civic organizations, and governments through interoperable digital spaces.

Instead of isolated civic tools and platforms, the system creates a **federated civic ecosystem** where communities operate their own Civic Hubs while remaining connected through shared identity, standards, and information flows.

The Civic Hub is the community-scoped instance of a more general primitive, the **Civic Space** — a scoped host environment whose types form an open set distinguished by scope: community-scoped Civic Hubs, individual-scoped Citizen Dashboards, entity-scoped Representative Spaces, and future scopes registered without redesigning the primitive.

The system is designed so that civic processes can appear across many interfaces — hubs, dashboards, feeds, and external websites — while remaining interoperable through open standards.

Goals:

- Reduce fragmentation across civic engagement tools
- Enable modular civic processes
- Allow communities to operate independent civic hubs
- Protect citizen privacy and autonomy
- Enable verified human participation
- Create network effects across civic organizations

---

# 3. Core Architectural Principles

## Federation

The network is **not centralized**. Communities, organizations, and jurisdictions can operate independent Civic Hubs while still participating in the broader ecosystem.

## Open Creation

Anyone can create and operate a Civic Hub. The system does not gatekeep participation.

However hubs may receive **verification credentials** identifying them as:

- official government jurisdictions
- trusted civic organizations
- recognized institutions

This allows openness while maintaining transparency.

## Open Standards

Protocols and integration standards are open so independent developers can build compatible tools, civic spaces, and civic processes.

## Modular Civic Processes

Democratic processes are implemented as modular capabilities rather than monolithic platforms.

## Identity Sovereignty

Citizens control their own credentials and identity proofs.

## Interoperability

Tools remain independent but communicate through shared standards.

## Local Autonomy

Each space controls its own moderation policies, enabled plugins, and governance rules.

---

# 4. Core System Entities

## Citizen

An individual participant in the civic ecosystem.

### Citizen Node

At the protocol level, a citizen's presence in the ecosystem is represented by a **Citizen Node** — the minimum viable unit of participation. A Citizen Node consists of a Decentralized Identifier (DID) paired with one or more Verifiable Credentials held in an identity wallet. It is not a product feature or UI element — it is the foundational identity object from which all civic participation derives.

A Citizen Node must be capable of three operations: authenticating (proving control of a DID), presenting credentials (surfacing verifiable claims for eligibility checks), and acting across systems (participating in any compliant civic environment using a single identity).

The Citizen Node explicitly excludes profiles, feeds, platform-owned state, and user interface elements. These are application-layer constructs that may vary across platforms. This separation ensures the Citizen Node remains portable and interoperable.

### Personal Data Store (PDS)

Alongside the Citizen Node, each citizen may maintain a **Personal Data Store (PDS)** — a user-controlled data layer that preserves the citizen's relationships, preferences, and context as they move between applications. The PDS stores the citizen's social graph (hub subscriptions, organizational follows, citizen-to-citizen connections), participation references, and user preferences.

The PDS does not store content contributions, feeds, or platform-owned data. Feeds are application-layer constructs built dynamically from PDS subscription data and hub activity.

Together, the Citizen Node (Identity Layer) and PDS (PDS Layer) form the citizen-controlled foundation of the ecosystem. Applications read from both layers but do not own the underlying data, which means citizens can switch between interfaces without losing their identity or accumulated civic relationships.

See the **Civic Identity Pilot** for the full specification of the Citizen Node, PDS, and the Identity Layer vs PDS Layer vs Application Layer separation.

### Citizen Capabilities

- holds identity credentials
- participates in civic processes
- subscribes to hubs and organizations
- receives civic updates

## Civic Hub

A digital civic space representing a jurisdiction, organization, or community.

### Types of Civic Hubs

While anyone may create a hub, several common categories are expected to emerge in the ecosystem:

**Government Jurisdiction Hubs**

- city or town governments
- counties
- states or provinces
- public agencies

These hubs may receive verification credentials confirming their official status.

**Civic Organization Hubs**

- nonprofits
- advocacy organizations
- professional associations
- civic initiatives

These hubs allow organizations to host discussions, civic processes, and updates related to their work.

**Issue Network Hubs**

- climate networks
- housing coalitions
- education reform communities

Issue networks connect participants around shared policy concerns that may span multiple jurisdictions.

**Community Group Hubs**

- neighborhood groups
- volunteer networks
- mutual aid groups
- local associations

These hubs support grassroots collaboration and civic participation at the community level.

All hub types operate using the same infrastructure and plugin framework.

Examples:

- town hub
- county hub
- civic organization hub
- issue network hub

Functions:

- host civic information
- enable civic processes
- publish updates
- manage community participation

Anyone can create a hub, but some hubs may carry **verification credentials**.

## Civic Process

A structured democratic participation activity.

Examples:

- citizen assemblies
- advisory voting
- participatory budgeting
- digital town halls

Civic processes are implemented through plugins.

## Plugin

A plugin represents a **capability within the civic ecosystem**.

Plugins may represent:

- civic processes
- community tools
- civic utilities
- data providers

Plugins are not owned by a single interface. Instead they may appear across multiple surfaces in the ecosystem.

## Credential

A verifiable digital claim about a participant or organization.

Examples:

- resident credential
- voter eligibility credential
- organization membership credential
- verified government hub credential

## Civic Activity Feed

A unified stream of civic activity aggregated from civic spaces, civic processes, and organizations.

Feed items may include:

- civic process openings
- vote results
- hub announcements
- participation opportunities

## Citizen Dashboard

A personal civic interface where citizens:

- access civic tools
- view civic feed
- manage space subscriptions
- discover civic opportunities

The reference Civic.Social dashboard is **curated for reliability and neutrality**.

However open standards allow anyone to build alternative dashboards.

---

# 5. System Architecture Overview

The Civic.Social ecosystem can be understood as a network of **interfaces connected to shared civic processes through identity.** (This is an informal view; the canonical five-layer model is given in Section 6.)

```
Citizens
   │
Identity Credentials
   │
──────── Interfaces ────────
Citizen Dashboard
Civic Hubs
External Websites
Civic Activity Feed
──────── Processes ──────
Plugin Ecosystem
   │
   ├ Civic Processes
   ├ Community Tools
   ├ Civic Utilities
   └ Data Providers
```

Key architectural idea:

**Processes (plugins) exist independently from interfaces.**

This allows a capability to appear in multiple contexts simultaneously.

Example: a legislative testimony capability could appear:

- inside a Civic Hub
- inside the citizen dashboard
- inside the civic activity feed
- embedded on a government website

Citizens authenticate using their civic credentials regardless of the interface used.

Important architectural note:

- Civic Hubs remain fully accessible **without the dashboard or feed**.
- The dashboard and feed act as **aggregation and discovery interfaces**, not gatekeepers.

---

# 6. Infrastructure Layers

The ecosystem is organized into the five canonical architecture layers (defined in the terminology glossary), read bottom → top.

## Layer 1 — Open Web Standards

The bedrock, adopted rather than invented:

- Decentralized Identifiers (DIDs)
- Verifiable Credentials (VCs)
- OpenID4VCI / OpenID4VP / SIOPv2
- ActivityPub / ActivityStreams
- AT Protocol
- JSON-LD, OAuth 2.0 / OIDC

This list is not comprehensive — the ecosystem adopts additional open standards as needs emerge.

## Layer 2 — Civic Specifications

Civic.Social's open specifications, which extend the web standards: **Civic Space · Civic Process · Civic Activity · Civic Identity**, plus companion documents (Civic Plugin Architecture, Discovery Layer, Authorization Model Note).

## Layer 3 — Sovereign Foundation

Identity and data owned by the participant, labeled by holder — person, entity, or community. For the person, this layer encompasses the Citizen Node (DID + credentials) and the Personal Data Store (relationships + preferences), forming the citizen-controlled foundation of the ecosystem; entities and communities hold parallel node + data store foundations. Applications built on top of this layer are replaceable — a participant's identity and accumulated civic relationships travel with them.

Responsibilities:

- issue credentials
- verify participation eligibility
- enable secure authentication
- maintain participant-controlled data (PDS and its entity/community counterparts)

## Layer 4 — Components

The layer that answers *what is the stuff interfaces are made of*: reusable building blocks, each implementing a slice of the specifications, each built once and assembled into every interface that needs it — one engine, many lenses. Two components (the Activity Feed and Civic Processes) are shippable as standalone embeds on any web page.

### Plugin Framework and Process Runtime

Provides modular extensibility for spaces. Plugins may be deployed in two ways.

**Embedded Plugins** — modules that run inside the space software.

Examples:

- events
- announcements
- simple polls

**Federated Plugins** — external services integrated through APIs and activity protocols.

Examples:

- citizen assembly platforms
- deliberation tools
- legislative testimony tools

The hybrid model allows simple tools to run locally while complex civic processes remain independent services. External civic tools — deliberation platforms, voting tools, participatory budgeting systems, legislative testimony tools — integrate through this framework.

### Civic Activity Feed Engine

Aggregates updates across the ecosystem.

Possible feed items:

- process openings
- vote results
- space announcements
- civic participation opportunities

### Identity Adapter and Access Control

The replaceable seams through which spaces authenticate participants, verify credentials, and enforce authorization (see the Authorization Model Note).

## Layer 5 — Interfaces

The distinct pieces of software people use: **Civic Spaces by scope**, plus infrastructure roles. Civic Spaces form an open, extensible set of types distinguished by scope.

### Civic Hub (community-scoped)

Hosts digital civic spaces for communities.

Capabilities:

- information publishing
- hub governance
- moderation
- plugin management

Hubs operate independently but may federate with others.

### Citizen Dashboard (individual-scoped)

A personal interface for navigating civic life.

Capabilities include:

- civic feed
- civic tools
- space subscriptions
- participation history

The dashboard is designed to be **a curated civic interface** rather than an open information marketplace.

### Representative Space (entity-scoped)

The neutral, public-facing surface for elected officials, candidates, and institutional bodies. Entities control their voice; they do not control the accountability data displayed alongside it.

### Infrastructure Roles

Ecosystem participants that serve the foundation rather than hosting processes at a scope: Citizen Account Providers (host a citizen's foundation on their behalf, federated like email providers) and Badge / Credential Issuers.

---

Two cross-cutting properties are deliberately **not** layers: **federation** is a capability of the Civic Activity Specification and the Discovery Layer, and **portability** is a contract of the Civic Space Specification over the Sovereign Foundation.

---

# 7. Plugin Model

Plugins represent ecosystem capabilities that may appear across multiple interfaces.

Plugins may be distributed through:

- a shared plugin marketplace
- independent installations by spaces

Marketplace plugins may be certified, while spaces remain free to install additional plugins independently.

## Plugin Categories

All plugins exist within a **single marketplace** but may be categorized for discovery.

### Civic Process Plugins

Examples:

- citizen assemblies
- advisory voting
- participatory budgeting
- deliberation platforms

### Community Plugins

Examples:

- events
- skill directories
- volunteer coordination

### Civic Utility Plugins

Examples:

- representative messaging tools
- petition systems
- legislative testimony tools

### Data Plugins

Examples:

- ballot information
- legislative tracking
- policy databases

## Plugin Surfaces

Plugins may expose interfaces across multiple surfaces.

Possible surfaces include:

- Hub interface
- Citizen dashboard interface
- Civic activity feed items
- External website embeds

---

# 8. Example Plugin Implementations

## Example 1 — Citizen Assembly Plugin

Type: Civic Process Plugin

Capabilities:

- organize citizen deliberation
- facilitate structured dialogue
- produce consensus outputs

Surfaces:

- hub interface (host assembly)
- civic feed (assembly announced)

## Example 2 — Ballot Information Plugin

Type: Data Plugin

Capabilities:

- provide ballot summaries
- display candidate information
- link to official election resources

Surfaces:

- citizen dashboard widget
- hub information panel

## Example 3 — Legislative Testimony Plugin

Type: Civic Utility Plugin

Capabilities:

- allow citizens to submit testimony on legislation
- verify participant jurisdiction
- aggregate public comments

Surfaces:

- civic hub
- citizen dashboard
- civic feed announcement
- embedded government website interface

Citizens authenticate using their civic credential regardless of the interface used.

---

# 9. Data Flow Model

Typical participation flow:

1. Citizen accesses a Civic Hub or dashboard
2. Citizen selects a civic process
3. Identity credentials verify eligibility
4. Citizen participates in the process
5. Process emits civic activities
6. The hosting space records summary data
7. Civic Activity Feed distributes updates

Example activities:

- civic.process.created
- civic.process.started
- civic.process.ended

This ensures spaces retain civic records even when processes run externally.

---

# 10. Federation Model

Spaces may federate information across the ecosystem. Federation is a capability of the Civic Activity Specification and the Discovery Layer, not a layer of the stack.

Examples:

- cross-space civic feeds
- federated directories
- multi-jurisdiction civic processes

Federation enables network effects without central control.

---

# 11. Example User Journey

Before Civic.Social, citizens navigate fragmented platforms and disconnected civic processes.

After Civic.Social:

A citizen logs into their Civic.Social dashboard once and sees:

- their local civic hub
- a participatory budgeting process in their county
- an advisory vote closing soon
- a civic organization discussion relevant to their interests

They can:

- vote in a civic process
- participate in discussions
- message a representative

All without creating new accounts.

Participation becomes continuous and visible across civic life.

---

# 12. Governance Model

Governance occurs at multiple levels.

## Space Governance

Each space (e.g., a Civic Hub) determines:

- moderation policies
- enabled plugins
- participation rules

## Ecosystem Governance

Shared standards may be maintained by a collaborative alliance of participating organizations.

---

# 13. Pilot Alignment

Pilot projects test individual layers of the architecture.

Example pilots include:

1. Plugin Framework Pilot
2. Civic Hub Deployment Pilot
3. Civic Process Integration Pilot
4. Citizen Dashboard Pilot
5. Decentralized Identity Pilot

Each pilot validates key aspects of the architecture.

---

# 14. Maturity and Evolution

This document defines the **baseline conceptual architecture (v0.4)** for Civic.Social.

Future versions of the architecture may expand:

- federation protocols
- plugin capability standards
- governance mechanisms
- identity and credential frameworks

The architecture is intentionally modular so that components can evolve through pilot experimentation.

---

# Version

Civic.Social Architecture Baseline Version 0.4 Draft
