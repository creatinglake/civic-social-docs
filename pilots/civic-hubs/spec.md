---
status: stable
last-reviewed: 2026-04-19
owners: [adam]
version: 0.1
---

# Civic Hubs Pilot

Civic.Social Infrastructure Program

---

## Table of Contents

### [How to Read This Document](#how-to-read-this-document-1)

### Executive Overview
1. [Executive Summary](#1-executive-summary)
2. [Purpose of the Pilot](#2-purpose-of-the-pilot)
3. [Relationship to the Civic.Social Pilot Program](#3-relationship-to-the-civicsocial-pilot-program)
4. [Strategic Importance](#4-strategic-importance)

### Strategic Context
5. [What is a Civic Hub](#5-what-is-a-civic-hub)
6. [Architectural Role of Civic Hubs](#6-architectural-role-of-civic-hubs)
7. [Civic Hub Governance](#7-civic-hub-governance)
8. [Social Design Philosophy](#8-social-design-philosophy)
9. [Modular Hub Configuration](#9-modular-hub-configuration)
10. [Example Civic Hub Use Cases](#10-example-civic-hub-use-cases)
11. [Hub Engine Approach](#11-hub-engine-approach)

### Hub Architecture
12. [Minimum Viable Civic Hub Stack](#12-minimum-viable-civic-hub-stack)
13. [Hub API and Discovery Requirements](#13-hub-api-and-discovery-requirements)
14. [Hub Event Model](#14-hub-event-model)
15. [Hub Identity Integration Model](#15-hub-identity-integration-model)
16. [Hub Credential and Participation Policy Model](#16-hub-credential-and-participation-policy-model)
17. [Hub-to-Process Integration Model](#17-hub-to-process-integration-model)
18. [Federation and Discovery Model](#18-federation-and-discovery-model)
19. [Moderation Model](#19-moderation-model)
20. [Portability Requirements](#21-portability-requirements)

### Pilot Implementation
21. [Minimum Viable Pilot Scope (In Scope / Out of Scope / Prototype vs Production)](#22-minimum-viable-pilot-scope)
22. [Pilot Phases and Timeline](#23-pilot-phases-and-timeline)
23. [Pilot Demonstration Scenarios](#24-pilot-demonstration-scenarios)
24. [Success and Validation Criteria (Deliverable Criteria / Technical Validation / Future Usability Evaluation)](#25-success-and-validation-criteria)
25. [Expected Deliverables](#26-expected-deliverables)

### Ecosystem and Partnerships
26. [Relationship to Other Civic.Social Pilots](#27-relationship-to-other-civicsocial-pilots)
27. [Estimated Development Effort and Team Roles](#28-estimated-development-effort-and-team-roles)
28. [Potential Pilot Partners](#29-potential-pilot-partners)
29. [Estimated Budget](#30-estimated-budget)

### Pilot Plan
30. [Risks and Mitigations](#31-risks-and-mitigations)
31. [Open Questions for Further Design](#32-open-questions-for-further-design)

### Conclusion and Future Work
32. [Conclusion](#33-conclusion)

---

## How to Read This Document

This document serves as the single canonical specification for the Civic Hubs Pilot. It is written for multiple audiences, and not every reader needs to read every section.

**Funders and program evaluators** should focus on the Executive Overview (sections 1–4), the Success and Validation Criteria (section 25), and the Pilot Plan (sections 28–32). These sections describe why Civic Hubs matter, how success will be measured, what the pilot will deliver, and the budget and timeline.

**Vendors and technical implementers** should focus on the Hub Architecture (sections 12–21), the Minimum Viable Pilot Scope (section 22), and the Expected Deliverables (section 26). These sections define the technical components to be built, the integration requirements, and the boundaries of the pilot implementation.

**Ecosystem partners** — civic technology platforms, civic organizations, hub operators, and standards bodies — should focus on the Strategic Context (sections 5–11), the Hub Identity Integration Model (section 15), and the Relationship to Other Civic.Social Pilots (section 27). These sections describe how Civic Hubs connect to the broader ecosystem and how partners can participate.

**All readers** benefit from the Executive Summary and the definition of What is a Civic Hub (section 5), which establishes the foundational concept that the entire pilot is built around.

The document is intentionally comprehensive. Sections build on each other, but each group can be read independently depending on the reader's needs.

---

## 1. Executive Summary

The Civic Hubs Pilot will design, deploy, and evaluate community-operated digital civic spaces — Civic Hubs — where citizens gather, participate in democratic processes, and interact within their local civic context.

Today, civic participation is scattered across commercial social media platforms, isolated government portals, email lists, and disconnected civic tools. Communities lack shared digital civic spaces that they own and govern. When civic participation platforms exist, they are typically controlled by technology companies rather than by the communities they serve. This fragmentation means that citizens struggle to discover participation opportunities, civic organizations duplicate infrastructure, and participation remains episodic rather than continuous.

Civic Hubs address this gap. Each hub is independently operated by a community, jurisdiction, or organization while interoperating with other hubs through shared identity, federation protocols, and open standards. Hubs are the community-operated digital civic spaces of the Civic.Social ecosystem — where citizens actually gather and participate.

The pilot will deploy 1–3 Civic Hubs — at least one with a real community — using the Civic.Social Hub Engine as the primary implementation. It will test governance and moderation models, integrate at least one civic process, and prove the hub architecture works in practice. The pilot will demonstrate two stages: standalone hubs that can launch immediately with conventional authentication, and (contingent on Civic Identity Pilot sequencing) integrated hubs connected to the broader Civic.Social architecture including Civic Identity, Civic Processes, and the Civic Activity Feed.

Deliverables include a Civic Hub Compliance Specification defining what it means to be a compliant hub in the ecosystem, reference hub deployments, an operator guide, an interoperability proof of concept demonstrating that independent hubs can discover and consume each other's event feeds, and identity integration demonstrating at minimum standalone authentication with a path to Civic Identity.

This pilot establishes Civic Hubs as shared digital infrastructure for democratic participation — civic spaces governed by communities, not platforms.

### Why This Matters Now

Democratic participation increasingly depends on digital infrastructure, yet communities lack digital civic spaces they own and govern. In a time of increasing mistrust in institutions and deepening polarization, the need for open, community-operated civic infrastructure has never been more urgent.

---

## 2. Purpose of the Pilot

The Civic Hubs Pilot explores the design, capabilities, and operational model for Civic Hubs — community or jurisdiction-based digital civic spaces that enable citizens to access civic information, participate in democratic processes, interface with their representatives, and organize with their fellow citizens.

Civic Hubs are a core component of the Civic.Social architecture. They function as the primary spaces where communities gather, Civic Processes occur, and participation becomes visible and continuous.

This pilot aims to define the minimal architecture, feature set, and ecosystem integrations required to launch the first Civic Hub implementations. It will deploy reference hubs with real communities and test how citizens, organizations, and local governments use them in practice.

---

## 3. Relationship to the Civic.Social Pilot Program

The Civic Hubs Pilot is one component of the broader Civic.Social Infrastructure Program. It operates alongside several complementary pilot initiatives:

**Civic Identity Pilot** — enables trusted participation through decentralized identity and verifiable credentials. Civic Hubs integrate with the identity layer to authenticate citizens and verify participation eligibility.

**Civic Process Plugin Pilot** — defines how civic processes (advisory voting, citizen assemblies, participatory budgeting) integrate as modular plugins across hubs, dashboards, and external applications. Civic Hubs host and execute these processes.

**Civic Activity Feed Pilot** — defines how civic participation opportunities and updates are distributed across the ecosystem. Civic Hubs publish events into the Civic Activity Feed and consume events from other hubs.

**Civic Credentialing & Profiles Pilot** — defines how civic badges are issued, displayed, and verified across the ecosystem. Badges are public credentials representing endorsements, achievements, or verified status that citizens and officials receive and can display on their public profiles.

**Citizen Dashboard** — a personal civic interface that brings a citizen's civic life into one place: aggregating updates from the hubs they participate in, surfacing relevant government and civic information, and providing tools to engage in democratic processes across the ecosystem.

Together these pilots form the foundation of a federated civic participation ecosystem. The Civic Hubs Pilot is where citizens actually gather and participate — the hubs are the execution layer that gives the rest of the ecosystem a place to operate.

---

## 4. Strategic Importance

The civic technology ecosystem currently lacks cohesive digital spaces where citizens can gather, discuss issues, discover participation opportunities, and engage in democratic processes.

Most civic participation currently occurs through fragmented and inadequate channels: commercial social media groups optimized for engagement rather than democratic outcomes, isolated civic engagement tools that don't interoperate, government portals that operate in silos, and email lists and newsletters that offer no structured participation. This fragmentation produces several structural problems.

Citizens struggle to discover participation opportunities. There is no unified civic space where a resident can see what's happening in their community, what decisions are being made, and how they can participate. Civic organizations duplicate infrastructure — each organization builds its own tools, its own accounts, its own notification systems. Communities lack shared digital civic spaces that they own and govern. And participation remains episodic rather than continuous, because there is no persistent civic space that connects isolated civic events into an ongoing civic life.

Many communities currently rely on commercial social media platforms — platforms optimized for advertising revenue and outrage rather than civic sensemaking and democratic participation. These platforms have no accountability to the communities that use them, no alignment with civic values, and no structural incentive to support constructive discourse. When a community's civic life depends on a platform it does not control, that community has no agency over its own civic infrastructure and, therefore, its own civic well-being.

Civic Hubs address these gaps by providing community-operated civic spaces aligned with real-world jurisdictions or civic communities. Each hub is independently governed while interoperating through shared standards. This model allows communities to operate their own digital civic spaces, made cost-effective through shared commons-based infrastructure, while benefiting from the network effects of a connected ecosystem.

---

## 5. What is a Civic Hub

A Civic Hub is a digital space operated by a community, jurisdiction, or organization where citizens can follow issues and public information, participate in structured Civic Processes, interact with their local community, and organize toward shared objectives. Hubs give communities the tools and infrastructure to deliberate, make decisions, coordinate action, and affect change — increasing their capacity to participate in and shape democratic life.

Civic Hubs are not centralized platforms. Each hub is independently operated, interoperable through shared identity and federation protocols. Communities moderate their own hubs according to their own norms and governance — not Civic.Social, not a technology company. A Civic Hub may represent a jurisdiction (a city, county, or state), an organization (a nonprofit, advocacy group, or coalition), a community group (a neighborhood association, mutual aid group, or volunteer network), or an issue network (a climate coalition, housing task force, or education reform community).

At the protocol level, a Civic Hub is a **process host and event node**. It hosts Civic Processes, accepts user actions on those processes, emits standardized Civic Events for distribution across the ecosystem, and exposes a discovery manifest that allows other systems to find and interact with it. This functional definition — host processes, emit events, support discovery — is what makes a hub a hub, regardless of its community type or the specific software it runs. But the purpose behind that protocol is human: to give communities a shared space where participation leads to outcomes.

The key architectural insight is that hubs are independently operated but interoperable. A citizen who participates in a municipal hub and a neighborhood hub and an organizational hub can do so with a single civic identity, and the events from all three hubs flow into the same Civic Activity Feed. This is what distinguishes the Civic Hub model from both centralized platforms (where a single operator controls the civic space) and isolated tools (where each tool is a disconnected silo).

---

## 6. Architectural Role of Civic Hubs

Within the Civic.Social architecture, Civic Hubs serve as the **Civic Hub Layer** — the community-operated spaces where participation actually happens.

The Civic.Social ecosystem is organized into six canonical layers:

**Identity Layer** — Decentralized identity enabled by Decentralized Identifiers (DIDs) and Verifiable Credentials. Citizens verify once and participate across the ecosystem.

**Civic Hub Layer** — Community-operated hubs where groups and communities organize, host processes, and emit events to the ecosystem. This is where this pilot operates.

**Civic Process Layer** — Modular, pluggable participation tools (advisory voting, citizen assemblies, participatory budgeting) that integrate through the Civic Process Plugin Framework.

**Civic Activity Feed Layer** — Aggregation, distribution, and discovery of civic events across the ecosystem, including hubs and processes.

**Discovery Layer** — Hub and process discovery, indexing, and search.

**Citizen Dashboard** — Personal interfaces for citizens to manage participation and credentials.

Civic Hubs sit at the center of this architecture. They host processes from the Process Layer, authenticate citizens through the Identity Layer, publish events into the Civic Activity Feed Layer, and appear in the Discovery Layer. The hub is the primary organizational node in the ecosystem — where existing groups, jurisdictions, and communities gain digital infrastructure they control. Hubs map to real-world social structures, giving them a persistent space to organize and participate.

Importantly, Civic Hubs remain fully accessible without the dashboard or feed. The dashboard and feed act as aggregation and discovery interfaces, not gatekeepers. A citizen can visit a hub directly, authenticate, and participate without ever using a dashboard or activity feed. This ensures that hubs function as independent spaces even before the broader ecosystem infrastructure is fully deployed.

---

## 7. Civic Hub Governance

Civic Hubs are governed according to the principle of **subsidiarity**. Decisions should be made at the smallest effective level of society rather than centralized in distant institutions with misaligned incentives.

Civic Hubs therefore mirror how society already organizes itself: individuals participate in communities such as neighborhoods, organizations, workplaces, and municipalities, and those communities govern their own spaces.

Civic.Social does **not** act as a gatekeeper determining who is allowed to create or operate a hub. Anyone can create a Civic Hub for their community or organization.

Some hubs may receive verification credentials identifying them as official institutions or trusted organizations, but hub creation remains open.

### Governance Principles

**Local autonomy.** Each hub determines its own moderation policies, participation rules, installed processes, and enabled community features. Moderation is local — communities define norms appropriate for their context, with AI-assisted tools available (section 19) but all decisions remaining with the community.

**Open creation.** Hub creation is permissionless. Civic.Social does not control who can operate a hub or what communities they serve.

**Transparent identity and credentials.** Verification credentials can indicate whether a hub represents a municipality, organization, or trusted institution. Citizens can distinguish between an official municipal hub and an informal community hub without restricting who can create either.

---

## 8. Social Design Philosophy

Civic Hubs intentionally diverge from the design philosophy of commercial social media platforms. This section consolidates the principles that guide hub design and explains why those principles matter for democratic participation.

### Structured Participation Over Open-Ended Engagement

Civic Hubs prioritize structured participation rather than open-ended social media dynamics. The goal is to create environments that support productive engagement while avoiding the polarization, attention manipulation, and moderation challenges common on large social media platforms.

Hubs support structured democratic participation such as citizen assemblies, advisory votes, participatory budgeting, and consultations. These structured formats produce actionable outputs — community signals, budget allocations, policy recommendations — rather than endless conversation threads that go nowhere.

### No Engagement-Maximizing Algorithms

Civic Hubs avoid algorithmic systems designed to maximize time-on-site or emotional engagement. Information is organized around relevance rather than engagement metrics. The long-term goal is algorithmic choice — giving communities and citizens control over how content is ranked and surfaced, rather than imposing opaque algorithms optimized for platform interests.

### Healthy Discourse by Design

Structured participation formats and local moderation encourage constructive dialogue. Rather than relying on content moderation to clean up after toxic engagement patterns, hubs use participation design to channel energy into productive formats from the outset. AI-assisted moderation tools support this goal by providing feedback on content quality and civility, but these tools operate transparently and under community governance.

### No Vendor Lock-In

Communities operating hubs retain control of hosting, data, and governance. Because hubs rely on open standards, communities can migrate infrastructure without being dependent on a single technology provider. The Interoperable Civic Hub Specification defines portability requirements ensuring that a community's governance history, social relationships, and process data can move between hub software implementations.

### Community Ownership of Civic Spaces

Unlike many commercial platforms, communities govern their own digital environments. A municipal hub is governed by the municipality. A neighborhood hub is governed by the neighborhood. An organization hub is governed by the organization. The space belongs to the community it serves.

### Progressive Feature Adoption

Hub development will begin with structured processes and formats more conducive to civic health — deliberation, voting, consultations — rather than open-ended social dynamics. More social features such as general discussion spaces and informal community interaction can be added over time at the discretion of hub operators, with appropriate moderation in place.

---

## 9. Modular Hub Configuration

Civic Hubs are intentionally modular and configurable. Hub administrators can enable or disable features depending on the needs of their community.

Configurable components include discussion spaces, civic process plugins, events calendars, the Civic Activity Feed, representative engagement tools, and organizational collaboration spaces.

Example configurations:

| Hub Type | Typical Configuration |
|---|---|
| Municipal hub | civic processes, government information, consultations |
| Neighborhood hub | discussions, events, community projects, lost and found |
| Civic organization hub | issue discussions, campaigns, deliberation |
| Union hub | voting tools, deliberation processes |
| Issue network hub | collective action, campaign planning, deliberation |

Hub operators decide which processes are enabled within their communities. The modular architecture ensures that a small neighborhood hub and a large municipal hub can run the same software with different configurations, and that both can participate in the same ecosystem.

---

## 10. Example Civic Hub Use Cases

### Municipal Civic Hub

A city government operates a hub where residents can follow council agendas, participate in advisory votes, join citizen assemblies, and comment on policy proposals. The hub publishes events to the Civic Activity Feed so residents can discover opportunities through their dashboards.

### Neighborhood Community Hub

A neighborhood association operates a hub where residents can share community updates, coordinate volunteer projects, discuss local issues, and host community meetings. The hub starts as a standalone space and later integrates with the city's municipal hub through federation.

### Civic Organization Hub

A nonprofit operates a hub where members can deliberate on policy issues, organize campaigns, host consultations, and collaborate on initiatives. Members authenticate using Civic Identity credentials that verify organizational membership.

### Issue Network Hub

Coalitions organized around issues such as housing, climate, or education collaborate through shared hubs that connect participants across jurisdictions and organizations.

### Labor Union Hub

Unions use hubs to support workplace democracy including contract deliberation, internal voting, campaign organizing, and cross-local coordination.

### Civic Coalition Hub

Multiple organizations collaborate within a shared hub to coordinate initiatives, host joint consultations, and manage cross-organizational campaigns.

---

## 11. Hub Engine Approach

The Civic Hubs Pilot does not mandate a single software platform for hub implementation. Instead, it defines the capabilities that any hub engine must provide to be compliant with the Civic.Social architecture. The long-term goal is to support multiple interoperable hub implementations rather than a single standardized platform.

### Required Hub Engine Capabilities

A compliant hub engine must support community-operated civic spaces with configurable governance, modular civic process integration through the Civic Process Plugin Framework, event emission conforming to the Civic Event Specification, identity integration with decentralized identity standards (with stub support acceptable in early phases), a discovery manifest at `/.well-known/civic.json`, community moderation tools (with AI-assisted moderation as an option), and data portability guaranteeing that communities can export their full governance history, social relationships, and process data.

### Potential Hub Engine Platforms

Several existing software platforms may serve as foundations for Civic Hubs, either as starting points for adaptation or as reference implementations demonstrating specific capabilities.

**[Civic.Social](https://civic.social) Hub Engine** is the hub engine being developed by Civic.Social as part of this pilot. It is purpose-built to implement the Civic Hub Specification, the Civic Process Plugin Framework, and the Civic Event Specification directly, rather than adapting an existing platform. It will serve as the reference implementation for the ecosystem and as the primary hub engine used in the pilot deployments.

**Bonfire Networks** is a modular, extensible social platform with ActivityPub federation support. Its modular architecture aligns well with the plugin model, and its federation capabilities provide a foundation for cross-hub interoperability. Bonfire is a candidate for adaptation as an alternative hub engine.

**Decidim** is a widely deployed civic participation platform with strong participatory governance features including citizen assemblies, participatory budgeting, and structured deliberation. Decidim demonstrates many of the civic process capabilities that hubs need to support, though its architecture was designed for standalone deployment rather than federation.

**Roundabout (New Public)** is a platform focused on healthier online discourse and civic conversation. Its design philosophy around constructive dialogue aligns with the Civic Hub social design principles.

Other federated platforms, including ActivityPub-based community software, may also serve as hub engines. The specification is designed to accommodate multiple implementations.

### Approach to Engine Selection

The pilot will use the Civic.Social Hub Engine as its primary hub engine, providing the reference implementation and the deployments used in pilot communities. In parallel, Civic.Social will reach out to the other platforms listed above — Bonfire Networks, Decidim, and Roundabout — to explore collaboration and the possibility of adapting their engines to interoperate with the Civic.Social ecosystem. The compatibility requirements defined above are intentionally designed to allow multiple engines to participate, and partner conversations with platform developers (discussed in section 28) will shape which adaptations move forward.

---

## 12. Minimum Viable Civic Hub Stack

The Civic Hub architecture builds on the hub specifications defined for the Civic.Social ecosystem. For the pilot phase, the goal is not to implement a fully mature federated hub system but to demonstrate a working reference implementation that proves community-operated spaces can host real participation and interoperate with the broader ecosystem.

The Minimum Viable Civic Hub Stack defines the core technical components required for this demonstration. This is comparable to the Minimum Viable Civic Identity Stack defined in the Civic Identity Pilot — it establishes what must be built to validate the architecture.

### 1. Hub Core

The hub core is the community platform software that provides the community experience. It must support user accounts and authentication (conventional login for standalone hubs, with a path to Civic Identity integration), community configuration and governance settings, content management for announcements, discussions, and updates, and access control for administrators, moderators, and members — implemented as role-based access control in the pilot, but designed as a replaceable seam so the system can evolve toward capability-based authorization in later phases.

### 2. Civic Process Host

The hub must be capable of hosting at least one structured civic process. In the pilot, this will be an advisory voting process (`civic.vote`). The process host must support the process lifecycle (draft, scheduled, active, closed, finalized), accept participant actions (e.g., submitting a vote), enforce participation rules (eligibility requirements, voting windows), and tally and publish results.

### 3. Event Emitter

Every process action and lifecycle transition must emit a standardized Civic Event. The event emitter is the single point of event creation within the hub. Events must conform to the Civic Event Specification and include source attribution (hub ID and URL), timestamps, process references, and jurisdiction scope.

### 4. Event Store

An append-only record of all events emitted by the hub. In the pilot, this may be an in-memory store or a simple database. The event store serves as the auditable record of all activity within the hub.

### 5. Discovery Manifest

The hub must expose a discovery manifest at `GET /.well-known/civic.json` that enables other hubs, indexers, and citizen interfaces to discover the hub, its jurisdiction, its active processes, and its event feed endpoint.

### 6. API Layer

The hub must expose a REST API supporting the required endpoints defined in section 13. This API enables external systems — including citizen dashboards, the Civic Activity Feed, and other hubs — to interact with the hub programmatically.

### 7. Identity Adapter

An integration layer that connects the hub to the Civic Identity system. In early pilot phases, this may be a stub that accepts a user identifier string. In later phases, it must support DID-based authentication and verifiable credential verification. The adapter must be designed so that upgrading from stub identity to full Civic Identity does not require rebuilding the hub.

### 8. Moderation Tools

Basic moderation capabilities including content review, user management, and community standards enforcement. This includes tools for flagging, reviewing, and acting on content; managing member status such as warnings, suspensions, and removals; and configuring community-specific participation rules. AI-assisted moderation is optional but recommended (see section 19). Moderation tools must operate under local community governance — moderation authority belongs to the community operating the hub, not to Civic.Social or any central platform.

### 9. Community Interface

A web-based interface that citizens use to interact with the hub — browse content, view processes, submit actions, and engage with the community. The interface is built on the hub's public API so that citizen dashboards and other clients can render equivalent experiences. The web interface is the primary user-facing surface of the hub in the pilot phase, with native dashboard clients expected to become the predominant consumer surface as the ecosystem matures.

---

## 13. Hub API and Discovery Requirements

The hub must expose a standardized API that enables interoperability with the broader Civic.Social ecosystem. The following endpoints are required.

### Discovery Manifest

```
GET /.well-known/civic.json
```

Returns a machine-readable manifest describing the hub, its jurisdiction, supported processes, and available endpoints. This enables hub discoverability by indexers, other hubs, and citizen interfaces.

Response structure:

```json
{
  "name": "Example Civic Hub",
  "type": "hub",
  "jurisdictions": ["us-va-floyd"],
  "feeds": ["https://hub.example/events"],
  "processes": [],
  "contact": "optional"
}
```

### Create Process

```
POST /process
```

Creates a new civic process instance within the hub. Input includes process type, metadata, and lifecycle configuration. Returns the created process object.

### Get Process

```
GET /process/:id
```

Returns the full state of a process, including metadata, lifecycle status, participation rules, and (for completed processes) results. This endpoint serves as the process descriptor — a machine-readable description of how the process integrates with the ecosystem.

### List Processes

```
GET /process
```

Returns all processes hosted by the hub, providing a read layer for user interfaces and external systems.

### Execute Action

```
POST /process/:id/action
```

Submits a participant action (e.g., a vote, comment, or proposal) to a process. Input includes the action name, action payload, and user identity. The hub validates the action against the process rules, updates process state if applicable, and emits one or more Civic Events.

### Process State

```
GET /process/:id/state
```

Returns a UI-friendly read model of the process state, including current tallies and participation metrics. This endpoint supports real-time interfaces without requiring clients to reconstruct state from events.

### Event Feed

```
GET /events
```

Returns the hub's event feed — a list of all Civic Events emitted by the hub, ordered by timestamp (descending). This is the primary endpoint that the Civic Activity Feed layer consumes to aggregate civic activity across hubs.

### Health Check

```
GET /health
```

Returns hub operational status for monitoring and discovery.

---

## 14. Hub Event Model

In plain terms: this section defines how hubs publish a standardized record of everything that happens in them, so the rest of the ecosystem — other hubs, activity feeds, citizen dashboards, indexers — can observe and respond to that activity. The specific fields and event types below are the technical contract that makes this possible.

All civic activity within a hub must produce standardized Civic Events. Events are the distribution layer of the ecosystem — they communicate what is happening across processes, hubs, and interfaces.

### Event Compliance

All events emitted by a Civic Hub must conform to the Civic Event Specification v0.1. Every event must include a unique event identifier, a schema version identifier, a canonical event type, a timestamp, the associated process ID, an actor identifier (DID or user ID), jurisdiction scope, an action URL linking to the source, source attribution (hub ID and hub URL), an event-specific data payload, and visibility metadata. Events may also include an optional `dedupe_key` field for deduplication in distributed environments.

### Required Event Types

Hubs must emit the following lifecycle events: `civic.process.created` when a process is instantiated, `civic.process.updated` when process configuration changes, `civic.process.started` when a process becomes active, `civic.process.ended` when participation closes, and `civic.process.result_published` when results are made available.

Hubs must also emit participation events: `civic.process.action_taken` for generic actions, `civic.process.vote_submitted` for votes, `civic.process.comment_added` for comments, and `civic.process.proposal_created` for proposals.

### Extended Lifecycle Event Types

The Civic Process Specification defines additional event types for later lifecycle phases beyond the base set above. These include `civic.process.framed` (emitted when process configuration is captured during the framing phase), `civic.process.aggregation_completed` (emitted when raw inputs are processed into structured results), `civic.process.outcome_recorded` (emitted when a decision or recommendation is formally recorded), and `civic.process.feedback_received` (emitted when participants provide structured feedback on the process). Hubs should emit these events as process support matures beyond advisory voting and as the full eight-phase lifecycle is implemented.

### Event-First Architecture

The hub operates on an event-first principle: every process action and every lifecycle transition must produce at least one event. There must be no silent state changes. The event log is the source of truth for all activity within the hub, and external observers must be able to reconstruct the state of any process by reading its event history.

### Forward Compatibility with ActivityPub

The event model is designed with a forward-compatible mapping to ActivityStreams, enabling future federation through ActivityPub. Each Civic Event can be mapped to an ActivityStreams activity object, providing a clean upgrade path from simple REST-based event distribution to full ActivityPub federation.

---

## 15. Hub Identity Integration Model

Civic Hubs integrate with the Civic Identity layer to enable trusted participation across the ecosystem. This section defines how hubs authenticate citizens, verify credentials, and manage participation eligibility.

### Identity Integration Phases

Identity integration follows a phased approach that allows hubs to launch before the full Civic Identity infrastructure is deployed.

**Phase 1 — Stub Identity.** Hubs accept a user identifier string (email, username, or opaque ID). Authentication uses conventional account systems (email login, OAuth). This allows hubs to operate immediately without depending on decentralized identity infrastructure.

**Phase 2 — Civic Identity Authentication.** Hubs authenticate citizens using Decentralized Identifiers (DIDs) through the Civic Identity Service. Citizens sign a cryptographic challenge to prove control of their identifier. After authentication, session continuity mechanisms allow citizens to move between processes within the hub without repeated authentication.

**Phase 3 — Credential-Verified Participation.** Hubs verify Verifiable Credentials before granting participation access. A process requiring jurisdiction residency requests the relevant credential from the citizen's wallet and verifies issuer signatures, credential integrity, and revocation status. This enables different processes within the same hub to have different participation requirements.

### Architectural Requirements

The identity adapter within the hub must be designed as a replaceable module so that upgrading from stub identity to full Civic Identity does not require rebuilding the hub. The hub must never bind user identity to a server-based account model in a way that prevents migration to decentralized identity. User identifiers used during the stub phase should be mappable to DIDs when the identity layer becomes available.

### Cross-Hub Identity

Because Civic Identity is a shared layer across the ecosystem, a citizen who authenticates once can participate across multiple hubs without creating new accounts or repeating verification. This is a core value proposition of the ecosystem: a single civic identity works everywhere.

---

## 16. Hub Credential and Participation Policy Model

Civic Hubs and the processes they host must define clear policies governing what credentials are required for different levels of participation. This model draws from the Identity Policy Model defined in the Civic Identity Pilot and adapts it to the hub context.

### Participation Tiers

Hubs may define multiple participation tiers based on credential verification. Common tiers include:

**Public access** — anyone may view hub content and browse active processes without signing in.

**Authenticated participation** — citizens must sign in with a persistent identifier to post, comment, or engage in discussions. The hub confirms the citizen controls the identifier but does not verify any attributes about them. Sign-in methods are defined in Section 15 and evolve from conventional login during the stub-identity phase to DID-based authentication once Civic Identity is available.

**Credential-verified participation** — citizens must present specific Verifiable Credentials to participate in gated civic processes. This goes beyond authentication: a trusted issuer has attested an attribute about the citizen, such as residency, membership, personhood, or age. Different processes may require different credentials. A municipal budget vote may require a residency credential; an organizational deliberation may require a membership credential; a general civic forum may require proof of personhood to prevent bot participation.

**Administrative access** — hub administrators and moderators hold role credentials granting governance and moderation permissions.

### Hub-Level vs Process-Level Policy

Identity policy operates at two levels. Hub-level policy defines baseline requirements for membership and general participation. Process-level policy defines additional requirements for specific civic processes hosted within the hub. Process-level policy inherits hub-level requirements and may add additional constraints but should not reduce them.

For example, a municipal hub may require proof of personhood for general participation. An advisory vote hosted within that hub may additionally require a city residency credential. The vote inherits the hub's personhood requirement and adds its own residency requirement.

### Standard Identity Mode Presets

Following the Civic Identity Pilot, hubs may use standard identity mode presets to simplify configuration: **Open** (no credentials required), **Verified Human** (proof of personhood required), **Verified Resident** (jurisdiction credential required), or **Public Profile** (optional real identity disclosure). These presets are UX abstractions of the underlying credential-based access control system.

---

## 17. Hub-to-Process Integration Model

Civic Hubs integrate civic processes through the Civic Process Plugin Framework. Processes are modular, pluggable participation tools that can be installed, configured, and executed within the hub environment — things like advisory votes, proposal submissions, participatory budgets, citizen assemblies, deliberation sessions, petitions, and public comment periods. Existing tools can also be adapted as process plugins; [Polis](https://pol.is/), for example, is a well-known deliberation platform that could be wrapped as a federated process plugin, giving hubs the ability to host structured group sensemaking without re-implementing it.

### Process Registry

The hub maintains a process registry that maps process type identifiers (e.g., `civic.vote`, `civic.assembly`, `civic.budget`) to **plugins** — self-contained modules that contain all the logic specific to a given process type. The registry is the extensibility mechanism: new process types are added by registering a plugin, without modifying the hub itself. All process-specific logic must be contained within the plugin, never in the hub's generic process management code or its HTTP endpoints.

### Process Lifecycle

Every Civic Process follows a standard lifecycle: draft, scheduled, active, closed, finalized. The Civic Process Specification defines a richer eight-phase lifecycle model (initiation, framing, activation, participation, aggregation, outcome/decision, publication, feedback/continuity) that maps onto these states. Hubs must support at least the core lifecycle states. Full lifecycle compliance — including aggregation, outcome recording, and feedback — is recommended for all production deployments.

### Process Observability

Civic Processes derive their legitimacy from transparency. Every lifecycle phase must produce at least one event or observable output. An external observer must be able to reconstruct the current lifecycle phase of any process by reading its event history and current state. This observability is the technical foundation for civic trust.

---

## 18. Federation and Discovery Model

Civic Hubs are designed to operate independently while interoperating through federation. Federation enables network effects without central control — hubs can discover each other, reference each other's processes, and distribute activity across the ecosystem.

### Hub Discovery

Every hub exposes a discovery manifest at `GET /.well-known/civic.json`. This manifest includes the hub's name, jurisdiction, event feed URL, and supported process types. Indexers and other hubs can use this manifest to discover and catalog hubs across the ecosystem.

### Cross-Hub Event Distribution

Hubs publish Civic Events to their event feed endpoint. The Civic Activity Feed layer aggregates events from multiple hubs to create a unified activity stream. Citizens subscribing to multiple hubs receive a merged feed of activity across their communities.

### Federation Protocol

For the pilot, federation operates through REST-based event distribution: hubs publish events, and the Civic Activity Feed layer periodically pulls from hub event endpoints. Future versions will support full ActivityPub federation (inbox/outbox model), enabling real-time cross-hub communication, subscription management, and direct hub-to-hub interaction.

### Cross-Hub Subscriptions

Citizens may subscribe to multiple hubs and follow civic activity across jurisdictions and communities. Subscription data is stored in the citizen's Personal Data Store (PDS), not within any individual hub, ensuring that a citizen's hub subscriptions are portable across applications.

### Interoperability Requirements

For the pilot, the minimum interoperability requirement is that two or more hubs can discover each other's existence, read each other's event feeds, and display cross-hub activity. This demonstrates the federation model at small scale without requiring the full complexity of real-time federation protocols.

---

## 19. Moderation Model

Moderation in Civic Hubs follows the principle of subsidiarity: moderation authority belongs to the community operating the hub, not to Civic.Social, not to a technology vendor, and not to a central platform. Each hub defines its own moderation policies, appoints its own moderators, and enforces its own community standards. There is no centralized moderation system across the ecosystem.

It is also worth noting that many of the structured civic processes at the core of the hub experience — votes, proposals, participatory budgets, structured deliberations — inherently require less moderation than open-ended conversation. Their formats guide participation toward decision-making and collective sensemaking rather than leaving the door open for discussions to devolve into unproductive conflict. Structured process design is itself a moderation strategy.

### Community-Governed Moderation

Hub administrators set moderation policies according to the community's governance model. This includes defining what constitutes acceptable conduct, how violations are handled, who has moderation authority, and how moderation decisions can be appealed. Moderation policies must be transparent and accessible to all hub members — every participant should be able to see the rules they are expected to follow and understand how those rules are enforced.

The hub's moderation configuration — rules, roles, and policies — is part of the hub's exportable state, ensuring that moderation approaches are preserved during hub migration.

### Moderation Transparency

All moderation actions must be auditable. Hub administrators must be able to review what moderation actions have been taken, by whom (or by what system), against what content, and under what policy. Communities may choose to make moderation logs visible to members as well. Transparency in moderation is essential to maintaining trust in the civic space — participants need to know that moderation serves the community's stated rules, not hidden agendas.

---

## 20. AI-Assisted Moderation (Optional)

Hubs may optionally adopt AI-assisted moderation to help manage the volume and complexity of community participation. AI moderation is not baked into the hub architecture — it is an optional tool that hub administrators can enable, configure, or disable based on what their community needs. When enabled, a human must always be in the loop: AI assists moderators, it does not replace them.

### Capabilities

AI-assisted moderation can help with content quality (evaluating sources, flagging unsupported claims, suggesting more credible references), civility (detecting personal attacks, harassment, or profanity and responding with warnings or rewrite suggestions based on community rules), and deliberation support (identifying off-topic comments, encouraging constructive framing, and helping keep discussions productive).

### Constraints

AI does not decide civic outcomes. AI does not filter viewpoints invisibly. AI moderation operates under the community's moderation policies, not its own judgment. All AI moderation actions must be auditable — hub administrators must be able to review what the AI flagged, what actions it took, and why, so they can verify that AI behavior aligns with community intent. If AI moderation is enabled, participants should be informed that it is in use.

The AI moderation strategy is documented separately in the Civic Hub AI Moderation Strategy document.

---

## 21. Portability Requirements

Portability is a first-class requirement of the Civic Hub architecture. Communities must be able to migrate their full community data, relationships, governance history, and operational configuration from one hub software implementation to another without losing structure, participation records, or social graph continuity.

### Migration Scope

A complete migration must preserve identity continuity (DID references remain intact), social graph continuity (relationships re-bind to the new hub), governance history (immutable records of proposals, votes, and deliberations), plugin configuration and state, and credential references and badge status. Migration is designed to occur once all processes have reached a finalized state. Migration of active process state (open proposals, live votes) is a long-term goal but is not guaranteed in the pilot.

### Export Requirements

Each hub must provide deterministic export functionality including hub metadata, identity references, membership lists, social graph edges, role mappings, all civic objects (posts, proposals, votes, deliberations, badges), active process state, plugin configuration, and moderation configuration.

Export data must be in a structured format (JSON-LD or compatible), with canonical ordering, explicit schema version tags, and integrity verification.

### Non-Loss Guarantee

Migration must not result in loss of governance history, vote records, delegation chains, badge issuance records, identity references, or plugin configuration metadata. If full runtime continuity is not technically possible due to engine incompatibility, data integrity must still be preserved in canonical form.

### Portability and Vendor Lock-In Prevention

These portability requirements ensure that no software vendor can lock a community into a single platform. A community that starts on one hub engine can migrate to another without losing its history or social relationships. This is a concrete implementation of the "no vendor lock-in" principle that distinguishes Civic Hubs from commercial platforms.

---

## 22. Minimum Viable Pilot Scope

This section defines the boundary between what the pilot will build and demonstrate and what remains part of the broader architectural vision for future phases.

### What the Pilot Will Demonstrate

The pilot will demonstrate a complete end-to-end hub experience:

1. A community launches a Civic Hub using reference hub software.
2. Citizens create accounts and join the hub (initially using conventional authentication, with a path to Civic Identity).
3. The hub hosts at least one structured civic process (e.g., an advisory vote or consultation).
4. Citizens participate in the process and receive results.
5. The hub publishes Civic Events to its event feed, demonstrating the event-first architecture.
6. A second hub discovers the first hub's existence and reads its event feed, demonstrating basic federation.
7. When Civic Identity integration is available, citizens authenticate using decentralized identity and present credentials for participation.

This loop demonstrates the core value proposition: community-operated civic spaces that host real participation and interoperate through open standards.

### What is In Scope

The pilot will produce:

**Civic Hub Compliance Specification.** A formal specification defining what it means to be a compliant Civic Hub — the required components, APIs, event contracts, identity integration requirements, and portability guarantees that any hub engine must implement to participate in the Civic.Social ecosystem. This is a primary deliverable of the pilot: it establishes the standard that enables multiple independent hub engines to interoperate and ensures that communities can migrate between them with full continuity of their governance history, social relationships, and process data.

**Reference hub deployments** with 1–3 real communities (neighborhoods, cities, or civic organizations), using the Civic.Social Hub Engine as the primary implementation.

**At least one civic process** hosted within a hub environment (e.g., advisory voting), demonstrating the full process lifecycle from creation through finalization.

**Event emission and feed publication** conforming to the Civic Event Specification.

**Identity integration** demonstrating standalone authentication. Civic Identity (DID-based) authentication is a stretch goal, dependent on the sequencing of the Civic Identity Pilot — if the identity infrastructure is available, the pilot will demonstrate it; if not, the hub's identity adapter will be designed to support it in a subsequent phase.

**Minimum viable admin panel** enabling hub operators to manage members, configure processes, review content, and perform basic moderation. This establishes the foundation for more advanced administration and moderation tools in future phases.

**Interoperability proof of concept** demonstrating cross-hub discovery and event distribution between at least two independent hubs.

**Operator guide** — practical documentation for communities to stand up, configure, and manage their own hubs.

**Hub engine evaluation report** assessing candidate platforms against the compliance specification.

### What is Explicitly Out of Scope

The following are part of the architectural vision but are not deliverables of this pilot: production-scale hub infrastructure (the pilot produces working prototypes, not production systems), full ActivityPub federation (the pilot demonstrates REST-based event distribution; full inbox/outbox federation is a future phase), Personal Data Store implementation (defined architecturally but delivered in a subsequent phase), advanced governance models such as liquid democracy or delegative voting, real-time event delivery (the pilot uses pull-based event distribution; push-based delivery is a future phase), and standalone process execution outside of hubs (the Civic Process Pilot covers process execution independently; this pilot focuses on processes hosted within hub environments).

### Prototype vs Production Intent

The pilot produces working prototypes that demonstrate the viability of community-operated civic hubs. Architectural decisions, API designs, event schemas, and integration patterns produced during the pilot are intended to serve as the foundation for production development. The goal is to prove the model, validate the architecture with real communities, and produce clear guidance for scaling.

---

## 23. Pilot Phases and Timeline

**Estimated duration: 9–12 months**

### Phase 1 — Architecture and Hub Design (2–3 months)

Finalize the hub architecture specification, configure the Civic.Social Hub Engine, define process integration models, design the identity integration adapter, and establish governance and moderation frameworks. This phase includes initial conversations with pilot community partners and outreach to other hub engine developers (Bonfire, Decidim, Roundabout) regarding potential collaboration.

Key deliverables: hub architecture specification, hub engine configuration, pilot community agreements, governance framework draft.

### Phase 2 — Hub Infrastructure Development (3–4 months)

Develop the reference hub implementation including the process host, event emitter, API layer, discovery manifest, and community interface. Implement the identity adapter (initially stub, with Civic Identity integration hooks). Build operator tools for hub configuration and management.

Key deliverables: working reference hub software, process integration (advisory voting), event emission, API endpoints, operator configuration tools.

### Phase 3 — Community Deployment and Integration (2–3 months)

Deploy reference hubs with pilot communities. Onboard community administrators and members. Test civic processes with real participants. Integrate with Civic Identity when available. Demonstrate cross-hub discovery and event distribution. Iterate based on community feedback.

Key deliverables: live pilot hubs with active communities, Civic Identity integration, interoperability demonstration, community feedback documentation.

### Phase 4 — Demonstration, Reporting, and Handoff (2 months)

Prepare a live demonstration environment for funders and ecosystem partners. Conduct stakeholder demos showcasing hub operations, civic processes, and federation. Produce a pilot report documenting findings, community feedback, and recommendations for production. Package deliverables — including architecture specs and operator guide — for handoff to the next phase of development.

Key deliverables: live demonstration environment, pilot report, operator guide, hub engine evaluation report.

---

## 24. Pilot Demonstration Scenarios

### Standalone Hub Scenario (Early Pilot)

This scenario demonstrates that Civic Hubs can operate as standalone spaces without depending on the full Civic.Social ecosystem.

1. A community (e.g., the Floyd County community in rural Virginia) launches a Civic Hub.
2. Residents create accounts on the hub using conventional authentication.
3. The hub presents a civic issue with structured background information.
4. Residents engage through structured processes: expressing positions, identifying concerns, and suggesting solutions.
5. The hub runs an advisory vote on the issue.
6. Results are aggregated into a civic brief — participation metrics, position breakdown, key concerns, suggested solutions — and shared with local officials (Board of Supervisors, Town Council).

This scenario validates that structured civic participation can produce actionable community signal even without ecosystem-wide infrastructure.

### Integrated Ecosystem Scenario (Later Pilot Phase)

This scenario demonstrates the full Civic.Social integration model. It is contingent on the Civic Identity Pilot preceding or running in parallel with the Civic Hub Pilot — the first two steps require identity infrastructure that this pilot does not build independently.

1. Citizens authenticate using Civic Identity (DID + Verifiable Credentials).
2. The hub verifies jurisdiction credentials and grants participation access.
3. The hub hosts a civic process (e.g., participatory budgeting) through the Civic Process Plugin Framework.
4. Participation events are published to the Civic Activity Feed.
5. Citizens in other hubs and dashboards discover the process through their activity feeds.
6. A second hub discovers the first hub's civic brief and references it in a related policy discussion.

If the Civic Identity Pilot has not yet delivered DID-based authentication, this scenario can still be partially demonstrated using standalone authentication for steps 3–6.

### Multi-Community Scenario

This scenario demonstrates that a citizen can receive updates in one feed from multiple hubs.

Multiple pilot hubs operate simultaneously — for example, a municipal hub hosting consultations, a neighborhood hub coordinating local projects, and an organizational hub hosting policy deliberation. The specific hub types will depend on which communities participate in the pilot. All hubs emit events to a simple Civic Activity Feed prototype. A citizen subscribed to multiple hubs sees a unified stream of civic activity across their communities. This is not a production-ready feed — the objective is to demonstrate the core pattern: events from independent hubs aggregated into a single citizen view.

---

## 25. Success and Validation Criteria

### Deliverable Criteria

The pilot will be considered successful upon production of:

**Working reference hub deployments** — at least two Civic Hubs deployed, at least one with a real community hosting active civic processes and demonstrating real citizen participation. The second hub may be a synthetic deployment used to demonstrate cross-hub interoperability and federation.

**Hub architecture specification** — a formal specification defining how Civic Hubs function, communicate with other hubs, and integrate civic processes. This becomes the canonical blueprint for all future hub implementations.

**Civic process integration** — at least one civic process type (advisory voting) hosted within a hub environment, demonstrating the full process lifecycle from creation through participation, aggregation, and result publication.

**Event emission and feed compliance** — all hub activity emitting valid Civic Events conforming to the Civic Event Specification, consumable by the Civic Activity Feed layer.

**Cross-hub interoperability** — at least two hubs demonstrating mutual discovery and event feed consumption, proving the federation model works at small scale.

**Identity integration** — demonstration of standalone authentication at minimum. If the Civic Identity Pilot has been executed or is running in parallel, a full demonstration of DID-based authentication with credential verification. If not, a minimum demonstration showing that the identity adapter is designed to support Civic Identity integration in a subsequent phase.

**Operator documentation** — sufficient for a community organization to stand up and manage a Civic Hub with reasonable effort.

### Technical Validation

The pilot will validate the following through real community deployments:

**Process lifecycle completion** — the full lifecycle of at least one civic process (draft, scheduled, active, closed, finalized) executes end to end within a hub environment, with each state transition emitting the corresponding Civic Event.

**Event compliance** — all hub events conform to the Civic Event Specification and are consumable by external systems.

**API compliance** — all required API endpoints function correctly and return valid responses. For example, `GET /.well-known/civic.json` returns a valid discovery manifest, `POST /process/:id/action` accepts a vote submission and emits a `civic.process.vote_submitted` event, and `GET /events` returns the hub's full event history in the correct format.

**Discovery and federation** — two hubs can discover each other via the `/.well-known/civic.json` manifest and read each other's event feeds.

**Identity adapter modularity** — the identity adapter can be upgraded from stub identity to Civic Identity integration without rebuilding the hub and without losing continuity of participant identity or verification status.

**Portability validation** — hub data can be exported in structured format conforming to the portability specification.

### Future Usability Evaluation

User-facing metrics — such as participation rates, onboarding completion, civic process engagement, discourse quality, and community satisfaction — will be evaluated through the pilot community deployments. The pilot will document findings and recommended evaluation criteria to inform future hub deployments.

---

## 26. Expected Deliverables

The Civic Hubs Pilot is intended to produce both architectural knowledge and practical infrastructure for the Civic.Social ecosystem.

**Hub architecture specification.** A detailed specification defining how Civic Hubs function, their required API endpoints, event compliance, identity integration, process hosting, federation model, and portability requirements. This specification becomes the canonical blueprint for all future hub implementations.

**Reference hub deployments.** At least two Civic Hubs deployed — at least one with a real community, demonstrating the model is operationally viable and citizen-usable.

**Hub engine evaluation report.** A detailed assessment of candidate hub engine platforms, evaluating each against the required capabilities defined in the architecture specification. Documents what worked, what didn't, scaling bottlenecks, and refinements needed for production.

**Operator guide.** Practical documentation for communities to stand up, configure, and manage their own Civic Hubs.

**Civic process integration.** Working implementation of at least one civic process (e.g., advisory voting) hosted within the hub environment, demonstrating the Civic Process Plugin Framework integration model.

**Identity integration implementation.** Working implementation of standalone authentication. Full Civic Identity (DID-based) integration demonstrated if the Civic Identity Pilot has been executed or is running in parallel; otherwise, a minimum demonstration that the identity adapter supports the upgrade path.

**Interoperability proof of concept.** Working demonstration that two or more hubs can discover each other and consume each other's event feeds, proving the federation model works.

**Live demonstration environment.** An interactive environment where funders, ecosystem partners, and prospective hub operators can experience Civic Hub operations firsthand — including community participation, civic processes, and cross-hub discovery.

**Pilot report.** A written report documenting what was built, what was validated, community feedback, key findings, and recommendations for moving toward production infrastructure.

---

## 27. Relationship to Other Civic.Social Pilots

The Civic Hubs Pilot has dependencies and integration points with every other pilot in the Civic.Social Infrastructure Program.

### Civic Identity Pilot

**Relationship:** The Civic Identity Pilot provides the authentication and credential verification infrastructure that hubs use to enable trusted participation. This is the most important cross-pilot dependency.

**Integration:** Hubs integrate with Civic Identity through the identity adapter (section 15). Citizens authenticate using DIDs, and hubs verify Verifiable Credentials to enforce participation requirements. The phased identity integration model allows hubs to launch with stub identity and upgrade to full Civic Identity as that infrastructure becomes available.

**Sequencing:** Hubs can launch before the Civic Identity Pilot completes by using conventional authentication. Civic Identity integration occurs when the identity infrastructure is available, potentially during Phase 3 of the hub pilot.

### Civic Process Plugin Pilot

**Relationship:** The Civic Process Plugin Pilot defines the framework through which civic processes integrate into hubs. Hubs are the primary host environment for civic processes.

**Integration:** Hubs host processes through the process registry pattern (section 17). The process plugin pilot defines process descriptor schemas, action contracts, and lifecycle models that hubs must support.

**Sequencing:** The hub pilot implements process hosting capabilities in Phase 2. The initial process (e.g., advisory voting) may be built as a plugin within the Civic.Social Hub Engine. Additional process types integrate through the Civic Process Plugin Framework as it matures.

### Civic Activity Feed Pilot

**Relationship:** The Civic Activity Feed Pilot defines how civic events are aggregated and distributed across the ecosystem. Hubs are the primary source of events that the feed aggregates.

**Integration:** Hubs emit Civic Events through their event feed endpoint (section 14). The Civic Activity Feed layer pulls from hub event feeds to build aggregated civic activity streams for citizens.

**Sequencing:** Hubs emit events from the outset. Integration with the Civic Activity Feed layer occurs when that infrastructure is available.

### Civic Credentialing & Profiles Pilot

**Relationship:** The Civic Credentialing & Profiles Pilot defines how Verifiable Credentials, in the form of publicly displayed badges, are issued, verified, and used throughout the ecosystem. Hubs are one of the environments where badges are displayed on citizen profiles and where participation badges may be earned.

**Integration:** Hubs display citizen badges within the community interface and on citizen profiles. Hubs may issue participation badges to citizens who engage in civic processes.

### Citizen Dashboard

**Relationship:** The Citizen Dashboard provides a personal civic interface that aggregates activity across hubs. Hubs are the primary source of the content that dashboards display. The hub's API-first design (section 12) is what makes dashboard integration possible — the dashboard is a client of the same APIs that power the hub's own web interface.

**Integration:** Dashboards consume hub event feeds and process listings. Citizens can navigate from dashboard activity items directly into hub processes.

**Sequencing:** The hub pilot produces the APIs and event feeds that dashboards consume. Dashboard development can proceed in parallel once the hub API layer is stable (Phase 2 onward).

---

## 28. Estimated Development Effort and Team Roles

### Estimated Timeline

9–12 months

### Preliminary Team Roles

The following roles are preliminary. A single person may fulfill more than one role, and roles will be further defined as the pilot team is established.

**Project Lead.** Overall project coordination, community partnerships, and ecosystem architecture alignment.

**Technical Lead.** Hub architecture design, system implementation, and coordination of technical development.

**Hub Infrastructure Engineers (2–3).** Implementation of the reference hub software including the process host, event system, API layer, and community interface.

**Civic Technology Integration Engineer.** Integrating hub infrastructure with Civic Identity, Civic Processes, and the Civic Activity Feed.

**Community and UX Design.** Citizen-facing hub experience, onboarding flows, process interfaces, and operator experience design.

**Community Engagement Lead.** Recruiting and supporting pilot communities, facilitating feedback, and documenting operational learnings.

**Ecosystem Partnerships.** Collaboration with hub engine developers, civic technology partners, and pilot community organizations.

---

## 29. Potential Pilot Partners

### Hub Engine Partners

The pilot will use the Civic.Social Hub Engine as its primary implementation. Civic.Social will reach out to the following platforms to explore collaboration, but their participation is not required for the pilot to proceed.

**[Roundabout (New Public)](https://newpublic.org/).** Roundabout's focus on healthy online discourse and civic conversation aligns with hub social design principles. A partnership could involve integrating Roundabout's discourse design patterns into hub community features or adapting their engine to interoperate with the Civic.Social ecosystem.

**[Bonfire Networks](https://bonfirenetworks.org/).** Bonfire's modular, extensible architecture and ActivityPub federation support make it a strong candidate for adaptation as a hub engine. Civic.Social is exploring a partnership with Bonfire to adapt their platform for civic hub use cases.

**[Decidim](https://decidim.org/).** Decidim's mature civic participation features — particularly citizen assemblies and participatory budgeting — demonstrate capabilities that hubs need. A partnership could involve integrating Decidim's process capabilities into the Civic Hub framework or using Decidim as a reference for civic process design.

### Civic Technology Partners

Civic process organizations and communities running civic participation tools — including organizations developing deliberation platforms, voting tools, and participatory budgeting systems — are potential integration partners for the process plugin framework.

### Pilot Community Partners

The pilot will deploy hubs with 1–3 real communities. Candidate communities include municipalities interested in digital civic engagement, neighborhood associations seeking community coordination tools, civic organizations wanting to host structured deliberation, and issue-based coalitions coordinating across jurisdictions. Community partners should represent diverse geographic, demographic, and organizational contexts to test the hub model across different use cases.

**[Better Together America](https://www.bettertogetheramerica.org/)** and their network of civic hub partners are potential collaborators for identifying and supporting pilot communities. Their existing relationships with local civic organizations could help recruit communities that are ready to participate and provide a built-in support network for pilot deployment.

### Identity Infrastructure Partners

The hub pilot will coordinate with the Civic Identity Pilot's identity infrastructure partner (potentially Digital Bazaar or another decentralized identity provider) to ensure hub identity integration is compatible with the identity architecture.

Final partners for each category are to be confirmed as the pilot moves into execution.

---

## 30. Estimated Budget

**Estimated total pilot cost: $350,000–$600,000**

**Major cost categories include:**

**Hub infrastructure development.** Design and implementation of reference hub software including process hosting, event systems, API layer, identity integration, and community interface. This is the largest cost category.

**Hub engine adaptation and integration (conditional).** If a hub engine partner (e.g., Roundabout, Bonfire, Decidim) joins the pilot, this covers adapting their platform to interoperate with the Civic.Social ecosystem. This cost category applies only if an external engine partnership is established.

**Community deployment and support.** Deploying hubs with pilot communities, supporting community administrators, onboarding participants, and iterating based on feedback.

**Civic process integration.** Implementing at least one civic process (e.g., advisory voting) within the hub environment, including process lifecycle, participation validation, and result aggregation.

**Identity integration development.** Building the identity adapter layer supporting both standalone and Civic Identity authentication.

**Interoperability and federation.** Implementing cross-hub discovery, event feed distribution, and the interoperability proof of concept.

**Operator guide.** Creating practical documentation for community hub operators.

**Pilot operations and testing.** Running pilot deployments, coordinating with communities, conducting evaluations, and producing the pilot report.

Some cost categories — particularly hub engine adaptation and community deployment — will depend on the hub engine partner selected and the number of pilot communities. The final budget allocation will be refined during partner engagement.

---

## 31. Risks and Mitigations

**Community Adoption Risk**

**Risk:** Pilot communities may be slow to adopt or may not sustain engagement with the hub over the pilot period.

**Mitigation:** Select pilot communities with demonstrated interest in civic engagement and existing organizational capacity. Provide active community support during the pilot. Start with high-value civic processes (e.g., an advisory vote on a real local issue) that give participants an immediate reason to engage. Design the hub experience so that the barrier to participation is low and the civic value is immediately visible.

**Hub Engine Maturity Risk**

**Risk:** Candidate hub engine platforms may lack capabilities needed for civic hub use cases, requiring significant adaptation work.

**Mitigation:** Evaluate hub engine candidates early in Phase 1 against the required capability list. Civic.Social is developing its own hub engine, which serves as the primary implementation for the pilot regardless of whether external engine partners join. If partnerships with Roundabout, Bonfire, or Decidim materialize, their engines can be adapted to interoperate with the ecosystem; if not, the pilot proceeds on the Civic.Social Hub Engine alone.

**Identity Integration Timing Risk**

**Risk:** The Civic Identity Pilot may not deliver identity infrastructure on a timeline compatible with the hub pilot's Phase 3 integration.

**Mitigation:** The phased identity integration model (section 15) is specifically designed to handle this risk. Hubs launch with conventional authentication and upgrade to Civic Identity when available. The identity adapter architecture ensures this upgrade is modular rather than requiring a hub rebuild.

**Moderation Challenges**

**Risk:** Community-level moderation may be insufficient to maintain constructive discourse, particularly in politically contentious contexts.

**Mitigation:** The primary mitigation is structural: Civic Hubs prioritize structured civic processes — votes, proposals, deliberations, participatory budgets — that guide participation toward decision-making and collective sensemaking rather than open-ended conversation that can devolve into conflict. Structured process design is itself the most effective moderation strategy. Beyond that, community-governed moderation policies give each hub control over its own standards, and AI-assisted moderation tools may be available as an optional supplement for hubs that need additional support.

**Interoperability Complexity**

**Risk:** Cross-hub federation and interoperability may prove more complex than anticipated, consuming development resources.

**Mitigation:** Scope the pilot's interoperability requirement to the minimum viable demonstration: two hubs discovering each other and consuming each other's event feeds. Defer full ActivityPub federation to a future phase. Focus development resources on the core hub experience rather than advanced federation.

**Scope Creep**

**Risk:** The breadth of the hub concept may lead to feature expansion beyond what the pilot can deliver.

**Mitigation:** The Minimum Viable Pilot Scope (section 22) explicitly defines what is in scope and out of scope. The pilot focuses on the core civic space experience and one civic process type, deferring advanced features to future phases.

---

## 32. Open Questions for Further Design

### Hub Engine Partnerships

**Question:** Should the pilot invest in partnering with an external hub engine developer in addition to the Civic.Social Hub Engine?

**Discussion:** The Civic.Social Hub Engine is the primary implementation and will serve as the reference deployment. The open question is whether to pursue the additional effort of working with an external partner — particularly Roundabout (New Public) — to adapt their engine for the Civic.Social ecosystem. A strong partner would demonstrate that the compliance specification works across multiple implementations, which is a powerful validation of the architecture. If a good partner fit emerges during Phase 1 outreach, the investment is likely worthwhile; if not, the pilot proceeds on its own engine.

### Governance and Moderation Defaults

**Question:** What should the default governance and moderation framework look like for a new Civic Hub?

**Discussion:** Different hub types — municipal, neighborhood, organizational — may need different governance structures and moderation approaches, but every hub needs a reasonable starting point. The pilot should work with communities and experts in civic discourse to define a minimum viable governance and moderation framework with sensible defaults: default participation rules, default moderation policies, default escalation paths. The goal is to establish good defaults that hub operators can customize as needed, balancing flexibility with development complexity. How much should be configurable versus standardized? What defaults promote healthy civic discourse without being overly prescriptive? These are questions the pilot should explore through real community deployments.

### ActivityPub Federation

**Question:** How much effort is required to implement ActivityPub federation with the Civic.Social Hub Engine, and should the pilot attempt it?

**Discussion:** ActivityPub provides a mature federation standard with existing ecosystem support, and the Civic Event model is already designed with forward-compatible mapping to ActivityStreams. The real question is the level of implementation complexity — full inbox/outbox federation involves subscription management, delivery guarantees, and content negotiation that go beyond simple event distribution. With current AI-assisted development tools, the implementation effort may be more manageable than it would have been previously, but the scope still needs careful assessment. The pilot should evaluate during Phase 1 whether a basic ActivityPub implementation is achievable within the timeline or whether pull-based event distribution is sufficient for the pilot period.

### Cross-Hub Event and Process References

**Question:** How should hubs and citizens follow events and processes from other hubs?

**Discussion:** Citizens should be able to follow processes and events from multiple hubs through their dashboard, with updates aggregating into a unified feed. Hubs themselves should also be able to follow events from other hubs — for example, a neighborhood hub surfacing relevant outcomes from a municipal hub's budget vote in its own feed. The mechanism for these cross-hub references (event-based subscriptions, API queries, or manual linking) needs design work during the pilot. This is closely related to the federation model and the Civic Activity Feed architecture.

### Citizen Dashboard Integration

**Question:** How tightly should the hub pilot coordinate with early Citizen Dashboard development?

**Discussion:** The hub pilot is building the APIs, event feeds, and process schemas that the Citizen Dashboard will consume. Decisions made in the hub pilot — endpoint design, event format, process state representations, discovery manifest structure — directly shape what the dashboard can render and how citizens experience the ecosystem. The question is whether to develop a minimal dashboard prototype during the hub pilot (even a simple web-based feed aggregator) to validate the hub-to-dashboard contract, or defer dashboard work entirely to its own pilot. A lightweight prototype during Phase 3 would test the integration early and surface contract issues before they become expensive to fix. This is also where the "ecosystem browser" pattern described in the Client Architecture Note becomes concrete.

### Scaling from Pilot to Production

**Question:** What infrastructure, governance, and federation challenges emerge when scaling from a small number of pilot hubs to hundreds or thousands?

**Discussion:** The pilot tests the hub model at small scale. Scaling introduces challenges around event distribution performance, discovery and indexing across a large hub directory, credential verification at volume, and operational support for communities standing up new hubs. Local autonomy is architecturally baked in — that does not degrade with scale. But the infrastructure that supports federation, discovery, and feed aggregation will need to evolve. The pilot report should include specific observations and recommendations for production scaling based on what the pilot deployments reveal.

---

## 33. Conclusion

The Civic Hubs Pilot addresses a fundamental gap in democratic infrastructure: the absence of community-operated digital civic spaces. Today, civic life online is fragmented across commercial platforms, government portals, and disconnected tools — none of which are governed by the communities they serve. Civic Hubs provide an alternative: digital civic spaces that communities own, govern, and operate, connected through open standards to a broader ecosystem of civic tools and processes.

This pilot will demonstrate that the model works in practice. By deploying reference hubs with real communities, hosting real civic processes, and proving basic federation between independent hubs, the pilot will validate the architecture and produce the artifacts — specifications, documentation, and operational learnings — needed to move toward production infrastructure.

The Civic Hub is where citizens primarily gather and participate — the central interaction environment of the Civic.Social ecosystem. Identity, processes, feeds, and dashboards each serve their own purposes and can operate beyond the hub context, but the hub is where most civic activity comes together. This pilot establishes the foundation for that civic space.

---

## Appendix A. Glossary

This glossary defines terms used throughout the document in plain language for readers who are not developers. Authoritative technical definitions live in the Civic.Social ecosystem specifications referenced in Section 30.

**[ActivityPub](https://www.w3.org/TR/activitypub/).** The open protocol that powers federated social networks like Mastodon. It defines how independent servers share posts and interactions. Civic.Social's event model is designed to map cleanly onto ActivityPub so hubs can federate with the broader fediverse in the future.

**[ActivityStreams](https://www.w3.org/TR/activitystreams-core/).** The data format ActivityPub uses to describe actions (someone posting, voting, joining a group). Civic Events are designed to be translatable into ActivityStreams.

**Civic Event.** A standardized record of something that happened in a hub — a process was created, a vote was submitted, a result was published. Events are the common language that lets hubs, feeds, and dashboards share activity.

**Civic Hub.** A community-operated digital civic space. A hub hosts civic processes, manages participation, and emits events to the rest of the ecosystem. Each hub is independently operated by a community, organization, or government.

**Civic Process.** A structured participation unit — a vote, a proposal, a deliberation, a participatory budget. Processes have defined lifecycles, participation rules, and outcomes, unlike open-ended conversation.

**[Credential (Verifiable Credential / VC)](https://www.w3.org/TR/vc-data-model/).** A digitally signed, cryptographically verifiable statement about a person — for example, "this person is a resident of Portland," issued by the city. Credentials let hubs confirm eligibility without citizens revealing more personal information than necessary.

**Dedupe key.** A field on an event that lets receivers recognize and discard duplicate copies of the same event. When events travel across multiple hubs and networks, the same event can arrive more than once; the dedupe key prevents double-counting.

**[DID (Decentralized Identifier)](https://www.w3.org/TR/did-core/).** An identifier that the citizen owns and controls, not tied to any platform or company. Citizens use their DID to sign in across hubs without creating new accounts on each one.

**Discovery manifest.** A file published at a well-known URL (`/.well-known/civic.json`) that tells other systems what a hub is, what jurisdiction it serves, what processes it hosts, and where to find its event feed.

**Event emitter.** The internal component of a hub responsible for creating and publishing Civic Events. Every action in the hub passes through the emitter so there is a single source of truth for what happened.

**Federation.** The principle that independent hubs can interoperate — share events, reference each other's processes, recognize each other's credentials — without being owned by a single company or controlled by a central server.

**Jurisdiction.** The geographic or organizational scope that a hub or process applies to — a neighborhood, a city, a state, an association, a membership body. Jurisdiction scoping is how the ecosystem ensures that, for example, only residents vote in a residents-only process.

**Lifecycle.** The sequence of states a civic process moves through (draft, scheduled, active, closed, finalized). Each transition emits an event.

**Plugin framework (Civic Process Plugin Framework).** The pattern that lets new process types (new kinds of votes, deliberations, budgets) be added to a hub without modifying the hub's core code. Each process type is a plugin that registers itself with the hub.

**Portability.** The guarantee that a community can export its full history, relationships, and process data from one hub engine and move it to another. Portability prevents lock-in and is a core commitment of the ecosystem.

**Process registry.** The internal catalog in each hub that maps process type names (like `civic.vote`) to their plugin implementations. The registry is how the hub knows which plugin handles which type of process.

**Subsidiarity.** The governance principle that decisions should be made at the most local level capable of making them effectively. Civic Hubs reflect this principle by putting governance in the hands of local communities rather than a central platform.

---

*Civic.Social — civic.social | adam@civic.social*
*A project of the Mosaic Foundation, a Virginia non-stock corporation*
*Civic Hubs Pilot v0.6*
