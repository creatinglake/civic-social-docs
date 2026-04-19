---
status: stable
last-reviewed: 2026-04-19
owners: [adam]
version: 0.1
---

# Civic Identity Pilot

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
5. [Architectural Rationale](#5-architectural-rationale)
6. [Identity Design Principles](#6-identity-design-principles)
7. [Identity Architecture Overview](#7-identity-architecture-overview)
8. [Citizen Node Definition](#8-citizen-node-definition)
9. [Personal Data Store (PDS) — Minimal Scope](#9-personal-data-store-pds--minimal-scope)
10. [Identity Layer vs PDS Layer vs Application Layer](#10-identity-layer-vs-pds-layer-vs-application-layer)
11. [Role of Civic Identity in the Civic.Social Architecture](#11-role-of-civic-identity-in-the-civicsocial-architecture)
12. [Why Civic Identity Matters for Democracy](#12-why-civic-identity-matters-for-democracy)

### Identity Architecture
13. [Civic Identity Integration Architecture](#13-civic-identity-integration-architecture)
14. [Civic Identity Integration Requirements](#14-civic-identity-integration-requirements)
15. [Core Identity Model](#15-core-identity-model)
16. [Credential Types](#16-credential-types)
17. [Proof of Personhood Model (includes Pilot Position on Proof of Personhood)](#17-proof-of-personhood-model)
18. [Jurisdiction Credentials](#18-jurisdiction-credentials)
19. [Civic Role Credentials](#19-civic-role-credentials)
20. [Identity Verification Paths](#20-identity-verification-paths)
21. [Authentication and Ecosystem Access Model](#21-authentication-and-ecosystem-access-model)

### Identity Policy
22. [Civic Identity Policy Model (includes Standard Identity Mode Presets)](#22-civic-identity-policy-model)
23. [Identity Policy Object](#23-identity-policy-object)
24. [Result Stratification Model](#24-result-stratification-model)

### Identity Infrastructure
25. [Identity Wallet Strategy](#25-identity-wallet-strategy)
26. [Privacy Model](#26-privacy-model)
27. [Interoperability Standards Foundation](#27-interoperability-standards-foundation)
28. [Credential Issuers](#28-credential-issuers)
29. [Civic Identity Service Model](#29-civic-identity-service-model)
30. [Identity Governance Model](#30-identity-governance-model)
31. [Minimum Viable Civic Identity Stack](#31-minimum-viable-civic-identity-stack)

### Pilot Implementation
32. [Minimum Viable Pilot Scope (In Scope / Out of Scope / Prototype vs Production)](#32-minimum-viable-pilot-scope)
33. [Pilot Phases](#33-pilot-phases)
34. [Expected Deliverables](#34-expected-deliverables)
35. [Civic Identity User Journey (includes Social Graph Portability)](#35-civic-identity-user-journey)
36. [Example Civic Identity Use Cases](#36-example-civic-identity-use-cases)
37. [Success and Validation Criteria (Deliverable Criteria / Technical Validation / Future Usability Evaluation)](#37-success-and-validation-criteria)

### Ecosystem and Partnerships
38. [Estimated Development Effort](#38-estimated-development-effort)
39. [Relationship to Other Civic.Social Pilots (includes Relationship to Civic Credentialing Pilot)](#39-relationship-to-other-civicsocial-pilots)

### Pilot Plan
40. [Pilot Team Roles](#40-pilot-team-roles)
41. [Potential Pilot Partners](#41-potential-pilot-partners)
42. [Estimated Budget](#42-estimated-budget)
43. [Timeline](#43-timeline)
44. [Risks and Mitigations](#44-risks-and-mitigations)

### Conclusion and Future Work
45. [Conclusion](#45-conclusion)
46. [Open Questions for Further Design](#46-open-questions-for-further-design)
[References](#references)

---

## How to Read This Document

This document serves as the single canonical specification for the Civic Identity Pilot. It is written for multiple audiences, and not every reader needs to read every section.

**Funders and program evaluators** should focus on the Executive Overview (sections 1–4), the Success and Validation Criteria (section 37), and the Pilot Plan (sections 40–44). These sections describe why civic identity matters, how success will be measured, what the pilot will deliver, and the budget and timeline.

**Vendors and technical implementers** should focus on the Identity Architecture (sections 13–21), the Minimum Viable Civic Identity Stack (section 31), and the Minimum Viable Pilot Scope (section 32). These sections define the technical components to be built, the integration requirements, and the boundaries of the pilot implementation.

**Ecosystem partners** — civic technology platforms, civic organizations, hub operators, and standards bodies — should focus on the Strategic Context (sections 5–12), the Integration Architecture (sections 13–14), and the Relationship to Other Civic.Social Pilots (section 39). These sections describe how civic identity connects to the broader ecosystem and how partners can integrate.

**All readers** benefit from the Executive Summary and the Citizen Node Definition (section 8), which establishes the foundational identity primitive that the entire system is built around.

The document is intentionally comprehensive. Sections build on each other, but each group can be read independently depending on the reader's needs.

---

## 1. Executive Summary

The Civic Identity Pilot will design and demonstrate decentralized identity infrastructure that enables trusted participation across the Civic.Social ecosystem.

Current civic technology systems rely on fragmented account systems. Citizens must repeatedly create accounts, eligibility is difficult to verify, and civic organizations must rebuild identity infrastructure for each platform. These limitations prevent civic technologies from functioning as a coherent ecosystem.

The pilot will implement a reference civic identity architecture based on established web identity standards developed through the World Wide Web Consortium (W3C), including Decentralized Identifiers (DIDs) [1] and Verifiable Credentials (VCs) [2]. These standards underpin a rapidly growing decentralized identity ecosystem that has already been adopted in deployments by major technology companies, governments, and global standards organizations. The system will support identity creation, authentication, credential issuance, and credential verification across civic hubs and civic processes.

The pilot will demonstrate a complete participation flow in which a citizen:

- creates a civic identity
- receives proof-of-personhood and jurisdiction credentials
- authenticates to a civic hub
- participates in civic processes across multiple platforms without creating new accounts

Deliverables include:

- a reference Civic Identity Service prototype
- credential schemas and verification tools
- developer integration libraries
- a cost and operational model for running civic identity infrastructure

This pilot establishes civic identity as shared digital infrastructure for democratic participation. Importantly, the architecture is designed from the outset to support multiple identity providers — the reference Civic Identity Service built during this pilot is not a centralized authority but a first implementation of an open, interoperable standard that other providers can adopt and extend.

## Why This Matters Now

Democratic institutions increasingly depend on digital participation, yet the infrastructure supporting civic engagement remains fragmented and vulnerable to manipulation. Online civic processes struggle to distinguish real participants from automated agents, verify jurisdictional eligibility, or enable citizens to participate across multiple civic platforms without repeated onboarding. As civic engagement increasingly moves online, trusted digital identity becomes essential infrastructure. Establishing an open, interoperable civic identity layer can help civic institutions, civic technology platforms, and democratic communities support legitimate participation at scale while preserving citizen privacy and autonomy.

---

## 2. Purpose of the Pilot

The Civic Identity Pilot explores how decentralized digital identity can enable trusted participation across a federated ecosystem of civic hubs, civic processes, and citizen interfaces.

Civic.Social aims to create shared digital infrastructure for democratic participation. Trusted identity is a foundational component of this infrastructure because civic processes often require verification of participation eligibility.

Examples include:

- verifying that a participant is a real person
- verifying that a participant resides within a jurisdiction
- verifying that a participant belongs to an organization or civic group
- verifying that a participant is eligible to participate in a specific civic process

Traditional identity systems rely on centralized accounts controlled by a single platform. Civic.Social instead explores a decentralized identity model in which individuals control their own identifiers and credentials while civic spaces define their own participation requirements.

The pilot will design and prototype a flexible civic identity infrastructure built on decentralized identity standards.

---

## 3. Relationship to the Civic.Social Pilot Program

The Civic Identity Pilot is one component of the broader Civic.Social Infrastructure Program.

Other pilots include:

- **Civic Process Plugin Pilot** — Defines how civic processes integrate across civic hubs, dashboards, and external applications.
- **Civic Activity Feed Pilot** — Defines how civic participation opportunities are distributed across civic activity feeds and citizen dashboards.
- **Civic Hubs Pilot** — Explores the architecture of jurisdiction-based civic spaces that host civic information and participation processes.

The Civic Identity Pilot enables trusted participation across these components.

Citizens authenticate once and can then participate across multiple civic processes and civic hubs without repeated onboarding.

---

## 4. Strategic Importance

The civic technology ecosystem currently suffers from fragmented identity systems.

Citizens frequently encounter:

- repeated registrations across civic platforms
- incompatible user accounts
- difficulty verifying participation eligibility
- inability to reuse identity across civic tools

For civic organizations, this fragmentation results in duplicated infrastructure and unnecessary technical complexity.

A shared civic identity layer reduces friction for both citizens and civic organizations by enabling:

- single onboarding across civic platforms
- portable identity credentials
- privacy-preserving verification of civic eligibility
- interoperability between civic tools and civic hubs

The goal is not to create a centralized identity platform but to establish a decentralized identity framework that enables trusted participation across a federated civic ecosystem.

---

## 5. Architectural Rationale

The Civic.Social identity architecture intentionally combines **open standards, federated interoperability, and reference infrastructure**. This approach reflects lessons learned from the history of the web and other open ecosystems.

In many technology ecosystems, standards alone are not sufficient to catalyze adoption. Without usable infrastructure, ecosystems often stall because no single organization is responsible for building the first working system.

For this reason, Civic.Social will pursue a dual strategy:

### 1. Define Open Identity Standards for the Ecosystem

The identity layer will be built on widely adopted decentralized identity standards such as W3C Decentralized Identifiers and Verifiable Credentials. These standards ensure that identity credentials remain portable and interoperable across multiple providers and platforms.

### 2. Build a Reference Civic Identity Service

To ensure the ecosystem can function immediately, Civic.Social will build and operate a reference Civic Identity Service that citizens and civic platforms can use from the outset.

This reference service provides a practical implementation of the architecture while remaining compatible with other identity providers that adopt the same standards.

This model allows the ecosystem to begin operating without waiting for external providers while still preserving long-term decentralization.

### 3. Enable Multiple Identity Providers Over Time

As the ecosystem grows, additional identity providers—including civic organizations, governments, and independent infrastructure providers—should be able to offer compatible civic identity services.

Citizens should ultimately be able to:

- create an identity using the Civic.Social service
- migrate their identity to another provider
- connect an existing decentralized identity to the ecosystem

This approach prevents vendor lock-in while maintaining a coherent identity layer for civic participation.

### 4. Phased Identity Adoption Strategy

The ecosystem will likely evolve in phases.

**Phase 1 — Reference Identity Service**

Civic.Social provides a complete identity service so the ecosystem can operate immediately.

**Phase 2 — Interoperable Identity Providers**

Additional organizations begin offering compatible identity services using the same standards.

**Phase 3 — Bring-Your-Own Identity**

Citizens may connect external decentralized identities or identity wallets to the ecosystem.

This phased strategy balances practicality with long-term decentralization goals.

---

## 6. Identity Design Principles

The Civic.Social identity system is guided by the following principles.

### Self-Sovereign Identity

Individuals control their own identifiers and credentials rather than relying on platform-controlled accounts.

### Decentralization

Identity should not depend on a central authority or database.

### Interoperability

Identity credentials must work across multiple civic hubs, civic processes, and external civic applications.

### Privacy by Design

Citizens should only disclose the minimum information required to participate in a civic process.

### Portability

Individuals must be able to migrate their identity and credentials between identity providers or wallets.

### Open Standards

The identity system should be built on widely adopted open standards rather than proprietary identity solutions.

---

## 7. Identity Architecture Overview

In the Civic.Social architecture, identity functions as a foundational interoperability layer for the entire ecosystem.

Just as federation protocols allow information to move across civic hubs, identity allows people to move across the ecosystem.

Together, these layers form the connective infrastructure of the Civic.Social network:

- **Identity Layer** — enables citizens to authenticate and present credentials across the ecosystem.
- **Activity Federation Layer** — protocols (such as ActivityPub [8] and related event standards) distribute civic updates, participation opportunities, and outcomes across hubs and activity feeds.

Identity therefore becomes the **interoperability fabric that ties the civic ecosystem together**, allowing a federated network of independent civic hubs, civic processes, and civic interfaces to function as a coherent experience for citizens.

A citizen should be able to:

- authenticate once using their civic identity
- discover opportunities through a civic activity feed
- move between civic hubs
- access civic processes across jurisdictions and organizations

All while the underlying infrastructure remains decentralized and independently operated.

### Architectural Layers

At a high level, the Civic.Social ecosystem consists of several interoperating layers:

**Civic Identity Layer**

Decentralized identifiers and verifiable credentials enable trusted authentication and participation across the network.

**Activity Federation Layer**

Protocols such as ActivityPub and standardized civic event schemas allow civic hubs and processes to publish participation opportunities and updates into shared civic activity feeds.

**Civic Spaces Layer**

Civic Hubs represent communities, jurisdictions, or civic organizations where participation processes occur.

**Civic Process Layer**

Modular civic processes (citizen assemblies, advisory voting, participatory budgeting, petitions, etc.) integrate through the Civic Process Plugin Framework.

**Citizen Interface Layer**

Feeds, dashboards, hubs, and external websites provide entry points where citizens discover and participate in civic processes.

Identity and activity federation together enable this federated civic ecosystem to feel seamless while preserving local autonomy and independence.

The Civic.Social identity architecture is built around decentralized identity technologies.

Key components include:

- **Decentralized Identifiers (DIDs)** — Each individual controls a cryptographic identifier.
- **Verifiable Credentials (VCs)** — Organizations issue cryptographically verifiable credentials to individuals.
- **Identity Wallets** — Citizens store and manage their credentials.
- **Credential Verification** — Civic hubs and processes verify credentials required for participation.

This architecture allows identity verification without requiring centralized identity databases.

---

## 8. Citizen Node Definition

The Citizen Node is the minimum viable unit of participation in the Civic.Social network. It is a protocol-level primitive that represents a single citizen's capacity to authenticate, hold credentials, and act across any system within the federated civic ecosystem. The Citizen Node is not a product feature, not a user interface element, and not a platform-specific construct. It is the foundational identity object from which all civic participation derives.

At its core, a Citizen Node consists of a Decentralized Identifier (DID) conforming to the W3C DID specification, together with one or more Verifiable Credentials held in an identity wallet. The DID provides a globally unique, cryptographically controlled identifier that the citizen owns and operates independently of any single platform. The Verifiable Credentials represent claims about that citizen—proof of personhood, jurisdiction residency, organizational membership, civic roles—that have been issued by trusted authorities and can be presented and verified across the ecosystem without contacting the original issuer.

The Citizen Node must be capable of three fundamental operations. First, it must authenticate: the citizen must be able to prove control of their DID through cryptographic challenge-response, establishing their identity to any civic hub, process, or platform that supports the Civic.Social identity protocols. Second, it must present credentials: when a civic process requires verification of eligibility, the Citizen Node surfaces the relevant credentials from the citizen's wallet, allowing the verifying system to confirm that the citizen meets participation requirements. Third, it must act across systems: a single Citizen Node must be sufficient to participate in any compliant civic environment, whether that environment is a local municipal hub, a state-level civic assembly, an organizational deliberation process, or an external civic technology application that has integrated with the Civic.Social identity layer.

Critically, the Citizen Node explicitly excludes several things that are often conflated with identity in traditional platform architectures. The Citizen Node does not include a profile. Profiles are application-layer constructs that may display a citizen's name, avatar, bio, or participation history, but they are not part of the identity primitive itself. Different applications may render profiles differently, or not at all. The Citizen Node does not include a feed. Feeds are constructed dynamically by applications based on the citizen's subscriptions, the activity of the hubs they follow, and the algorithms or curation logic of the interface they are using. The Citizen Node does not include any user interface layer. How a citizen interacts with their identity—whether through a mobile wallet, a browser extension, a custodial service, or a command-line tool—is an implementation choice, not a property of the node itself. The Citizen Node does not include any platform-owned state. No platform may claim ownership of a citizen's identifier or credentials; these remain under the citizen's control at all times.

This separation is essential because it ensures that the Citizen Node remains portable and interoperable across the entire ecosystem. Because the Citizen Node is defined at the protocol level rather than the product level, it can be implemented by any identity provider, wallet, or civic platform that conforms to the underlying standards. A citizen who creates their identity through the Civic.Social reference service can later migrate to an independent identity provider without losing their credentials or their ability to participate. A citizen who already holds a compatible DID and credentials from another ecosystem can connect that identity to Civic.Social without starting over. The Citizen Node is the unit that moves across the system, and because it carries only identity and credentials—not application state—it can move freely without being tethered to any single platform's data model or user experience.

This design also allows the ecosystem to evolve without locking users into any particular generation of applications and experiences that can be built on top of it.

The Citizen Node is the identity half of the citizen-controlled foundation. The other half — the citizen's relationships, subscriptions, and preferences — is maintained in the Personal Data Store (PDS), described in the following section. Together, these two components ensure that everything a citizen needs to participate in the ecosystem travels with them: their verified identity and credentials via the Citizen Node, and their accumulated civic relationships and context via the PDS.

---

## 9. Personal Data Store (PDS) — Minimal Scope

The Personal Data Store is a user-controlled data layer that operates alongside the Citizen Node but is architecturally distinct from it. Where the Citizen Node handles identity—who a citizen is and what claims can they verifiably make about themselves—the PDS handles the relationships, preferences, and references that a citizen accumulates as they participate in the civic ecosystem. The PDS exists to solve a specific problem: without it, citizens who switch applications, interfaces, or platforms risk losing the connections, subscriptions, or context they have built up over time.

In its minimal scope for this pilot, the PDS stores four categories of user-owned data. First, it stores the citizen's social graph: the set of follows, subscriptions, and connections that define the citizen's relationship to civic hubs, organizations, other citizens, and communities within the ecosystem. This includes which hubs a citizen has subscribed to, which organizations they are affiliated with, and which other citizens they have chosen to follow or connect with. Second, it stores hub memberships and subscriptions: a record of which civic hubs and communities a citizen has joined, along with any subscription preferences that govern how they receive updates from those hubs. Third, it stores participation preferences: lightweight pointers to civic processes the citizen has engaged with, such as consultations they have contributed to, assemblies they have attended, or votes they have cast. These are references rather than full content—the PDS does not store the substance of a citizen's contributions, which remains with the civic process that hosted it. Fourth, it stores user preferences: settings and configurations that govern the citizen's experience across applications, such as notification preferences, accessibility settings, language choices, and display options.

The PDS explicitly does not store several categories of data that might be expected in a more expansive personal data system. It does not store full posts or content contributions. When a citizen participates in a deliberation, submits a comment, or casts a vote, the content of that participation belongs to the civic process that hosted it, not to the citizen's PDS. The PDS may store a reference indicating that the citizen has engaged with that process, but not the substance of what they said or how they voted. The PDS does not store feeds. Feeds are constructed dynamically by applications based on the citizen's subscriptions, the activity of the hubs they follow, and the algorithms or curation logic of the interface they are using. The Civic Activity Feed may provide the subscription data that feeds are built from, but the feed itself is an application-layer construct. The PDS does not store platform-owned application data. Each civic hub, process, or interface may maintain its own internal data about citizen interactions, but this data belongs to the application, not to the citizen's PDS.

This scoping is intentional and serves several important architectural purposes. By limiting the PDS to relationships and preferences rather than content and application state, the system preserves freedom of association. A citizen's social graph—who they follow, which hubs they belong to, which organizations they are connected to—is fundamentally a matter of personal association. Storing this data under the citizen's control rather than within any single platform ensures that no platform can hold a citizen's network hostage. If a citizen decides to leave one civic interface for another, their follows, subscriptions, and hub memberships travel with them because this data lives in their PDS, not in the application they are leaving.

This design also prevents platform lock-in at the data level. In traditional civic technology platforms, a citizen's connections and participation history are stored within the platform's own database. If the citizen wants to switch to a different platform, they must start over, rebuilding their network of connections and losing the context of their prior engagement. With the PDS, the citizen's core relational data is portable. A new civic interface can read the citizen's PDS to reconstruct their hub subscriptions, surface relevant activity from the hubs they follow, and present a coherent experience from the first interaction—because the data that defines the citizen's place in the ecosystem is not locked inside any single application.

The relationship between the PDS and the identity wallet deserves clarification. Verifiable credentials may be stored in the identity wallet, referenced via the PDS, or both, depending on the specific architectural choices made during implementation. In some configurations, the wallet serves as the authoritative store for credentials while the PDS maintains user state. In other configurations, the PDS may serve as a broader container that includes or wraps wallet functionality. For the purposes of this pilot, the key architectural constraint is that credentials and user state must both be portable and user-controlled, regardless of which specific component stores them. The PDS is distinct from the identity layer but works alongside it: identity enables authentication and verification, while the PDS preserves the continuity of the citizen's relationships and context across the ecosystem.

---

## 10. Identity Layer vs PDS Layer vs Application Layer

The Civic.Social architecture maintains a clear separation of concerns across three distinct layers: the Identity Layer, the PDS Layer, and the Application Layer. This separation is fundamental to the system's design and ensures that citizens retain control over the components that matter most—their identity and their data—while allowing the application ecosystem to evolve freely.

The Identity Layer encompasses the citizen's DID and their verifiable credentials. This layer is responsible for authentication and verification. When a civic process requires verification of eligibility, the Identity Layer answers the question: who is this person, and what claims can they verifiably make about themselves? The Identity Layer is persistent, portable, and entirely under the citizen's control. It does not depend on any particular application, platform, or interface. The Citizen Node—the DID plus the associated credentials—can move freely between applications, wallet implementations, or identity providers. The Identity Layer does not include any platform-owned state. No platform may claim ownership of a citizen's identifier or credentials; these remain under the citizen's control at all times.

The PDS Layer encompasses the citizen's user-owned data: their social graph, hub subscriptions, participation preferences, and preferences. This layer is responsible for preserving continuity. When a citizen follows a new civic hub, when they subscribe to updates from a community organization, when they participate in a deliberative process, those subscriptions are recorded in their PDS rather than solely within the application they happen to be using. This ensures that if the citizen later switches to a different civic interface, their relationships, follows, and preferences travel with them. Like the Identity Layer, the PDS is persistent, portable, and under the citizen's control. It does not belong to any application. Applications can read the PDS to understand the citizen's subscriptions and connections, but no application owns the citizen's relational data. The PDS Layer answers the question: what relationships and context has this person built within the ecosystem?

The Application Layer encompasses everything that citizens see and interact with: feeds, profiles, hub interfaces, civic process UIs, dashboards, and all other user-facing experiences. Applications are replaceable because identity and user data live in layers that the citizen controls. A citizen can switch from one civic dashboard to another, from one feed interface to another, or from one hub platform to another, because their Identity and their PDS—the two components that define their participation capability and their place in the ecosystem—are not locked inside any single application. Applications read from the Identity Layer to authenticate citizens and verify credentials. Applications read from the PDS Layer to understand the citizen's subscriptions, follows, and preferences. Applications write to the PDS Layer when citizens take actions that should persist across platforms, such as following a hub or changing notification preferences. Applications generate feeds, profiles, host civic processes, and provide user interfaces, but the citizen's fundamental identity and relational data live in the Identity and PDS layers, not in the application layer.

This separation has several critical implications that the architectural design deliberately enforces. Feeds are not a primitive. A feed is an application-layer construct that is assembled from underlying data—hub activity, subscription preferences, algorithmic curation—and rendered by a specific interface. Different applications may construct different feeds from the same underlying PDS data. A citizen's feed is not part of their identity, not part of their PDS, and not something that any single application has exclusive control over. Profiles are not required. A profile is an application-layer construct that may display information about a citizen, but it is not a component of the Citizen Node or the PDS. A citizen can participate fully in the Civic.Social ecosystem without ever creating a profile. Some applications may offer profile experiences; others may offer none at all. The citizen's ability to authenticate and participate does not depend on the existence of a profile. Applications are replaceable. Because identity and user data live in layers that the citizen controls, a citizen can switch from one civic interface to another without losing anything essential. Their DID and credentials travel with them via the Identity Layer. Their subscriptions, follows, and preferences travel with them via the PDS Layer. The specific application is merely a view onto these layers and can be replaced without disrupting the citizen's place in the ecosystem. This is a deliberate architectural choice that reinforces the system's core commitments: no centralized social graph, no forced profiles, and trust rooted in cryptographic verification and web-of-trust relationships rather than centralized authority.

---

## 11. Role of Civic Identity in the Civic.Social Architecture

The broader Civic.Social system architecture—including Civic Hubs, Civic Processes, and federated civic activity feeds—is described in the Civic.Social Architectural Baseline document. This pilot document focuses specifically on the identity layer required to enable trusted participation across this ecosystem.

Within the Civic.Social architecture, civic identity serves as a **cross-platform trust and authentication layer**. It allows citizens to authenticate once and present verifiable credentials when participating in civic processes hosted across different hubs, processes, or civic interfaces.

Rather than functioning as a centralized platform gatekeeper, the civic identity layer provides authentication and credential verification services that participating civic hubs, civic processes, and external civic platforms can use.

At a conceptual level, civic identity connects citizens with the civic participation systems they interact with.

- **Citizens** — Individuals control decentralized identifiers and hold credentials representing civic attributes such as personhood, residency, or organizational membership.
- **Civic Identity Layer** — Authentication and credential verification infrastructure that allows civic systems to confirm identity claims without requiring centralized user databases.
- **Civic Participation Systems** — Civic Hubs, Civic Processes, dashboards, and other civic platforms where citizens discover and participate in civic processes.

In this architecture, identity acts as a **shared trust layer** that allows these independent civic systems to interoperate while preserving local autonomy and user control.

---

## 12. Why Civic Identity Matters for Democracy

Modern democratic participation increasingly occurs through digital systems, yet the identity infrastructure supporting these systems is fragmented, platform-controlled, and often incompatible across civic technologies.

This fragmentation produces several problems for democratic participation:

- citizens must repeatedly create accounts across civic platforms
- participation eligibility is difficult to verify
- some civic processes struggle to prevent bots, impersonation, or duplicate participation — as demonstrated by incidents such as the mass fabrication of public comments during the FCC's 2017 net neutrality proceeding and similar integrity concerns in participatory budgeting and online public comment systems
- without verified identity, the outputs of civic processes — advisory votes, public consultations, deliberative results — lack the credibility needed for government institutions and elected officials to act on them
- civic organizations must rebuild identity infrastructure repeatedly

A shared civic identity layer addresses these problems by enabling trusted, portable identity across the civic ecosystem.

Civic identity infrastructure enables:

- verified human participation
- jurisdiction-based civic processes
- cross-platform civic participation
- reduced onboarding friction for citizens
- interoperability across civic technologies

In this sense, civic identity functions as foundational digital infrastructure for democratic participation, much like payment infrastructure enables commerce or communication protocols enable the internet.

By establishing an open, decentralized civic identity layer, the Civic.Social ecosystem seeks to make democratic participation easier, more trustworthy, and more scalable.

---

## 13. Civic Identity Integration Architecture

The Civic Identity layer functions as shared infrastructure that enables citizens to authenticate and present credentials across the Civic.Social ecosystem.

Rather than acting as a centralized platform gatekeeper, the identity layer provides authentication and credential verification services that participating civic hubs, civic processes, and external civic platforms can use.

### Identity Authentication Flow

A typical authentication flow may proceed as follows:

1. Citizen accesses a civic hub, civic process, or external civic platform.
2. The platform requests authentication through the Civic Identity system.
3. The citizen authenticates using their decentralized identifier through their wallet or identity provider.
4. The platform verifies the citizen's control of the identifier.
5. The platform requests any credentials required for participation.
6. The citizen's wallet presents the requested credentials.
7. The platform verifies credential signatures and issuer trust.
8. If requirements are satisfied, the citizen is granted access to the civic process.

### Integration with Civic Hubs

Civic hubs may use the identity infrastructure to:

- authenticate citizens
- verify jurisdiction credentials
- manage participation eligibility

Each hub defines its own credential requirements for participation.

### Integration with Civic Processes

Civic processes integrated through the plugin architecture can request credentials required for participation.

Critically, citizens do not need to access civic processes through a hub or dashboard. A civic process may be encountered anywhere — through a link in a civic activity feed, embedded on a government website, shared in an email, or accessed directly by URL. In all cases, the civic process can authenticate the citizen and verify their credentials directly through the Civic Identity layer. The identity infrastructure works the same regardless of how or where the citizen encounters the process.

When a citizen does access a civic process through a hub or dashboard, the process may rely on the identity session already established with that environment, avoiding repeated authentication.

### Integration with Civic Feeds and Dashboards

Citizen dashboards and civic feeds aggregate participation opportunities across multiple civic hubs.

When a citizen selects an action from a feed, they may be taken directly to the relevant hub or civic process.

Because authentication occurs through the shared identity layer, the citizen does not need to create new accounts for each platform.

### Integration with External Civic Platforms

Civic.Social identity is not limited to hubs and processes within the Civic.Social ecosystem. External civic platforms — existing civic technology applications that maintain their own interfaces and capabilities — can integrate Civic.Social identity using authentication and credential verification protocols. These platforms do not need to become Civic.Social hubs or adopt the plugin framework; they only need to support the identity standards.

Examples include legislative engagement platforms, participatory democracy tools such as Decidim, civic organizing applications, or government websites that host their own public comment or consultation systems. By accepting standard verifiable credentials from trusted issuers for authentication and eligibility verification, these platforms allow citizens to participate using the same civic identity they use across the rest of the ecosystem — without creating yet another account.

This allows citizens to participate in civic processes across multiple civic technologies using a consistent civic identity.

The identity layer therefore acts as the connective infrastructure enabling a federated civic ecosystem to function cohesively.

---

## 14. Civic Identity Integration Requirements

To enable interoperability across the Civic.Social ecosystem, participating systems must integrate with the Civic Identity layer using a common set of functional requirements. These requirements are not intended to prescribe specific implementations but to define the capabilities needed for hubs, civic tools, and identity services to interoperate.

### Hub Integration Requirements

Civic Hubs represent communities, jurisdictions, or organizations where civic participation occurs. To participate in the Civic.Social ecosystem, hubs should support the following identity-related capabilities:

#### Authentication

- allow citizens to authenticate using a decentralized identifier
- support identity authentication flows compatible with the Civic Identity service

#### Credential Requests

- request credentials required for hub participation
- define credential rules for access to civic processes

#### Credential Verification

- verify signatures and issuer trust for presented credentials
- enforce participation rules based on credential requirements

#### Session Continuity

- maintain authenticated identity sessions across hub interactions
- allow integrated civic processes to reuse existing identity sessions when appropriate

#### Federation Compatibility

- publish civic participation events to the activity federation layer
- accept participation traffic originating from civic feeds or external links

### Civic Process Integration Requirements

Civic Processes are modular civic interactions integrated through the Civic Process Plugin Framework.

Processes must support identity verification so they can enforce participation rules defined by civic hubs or process operators.

Required capabilities include:

#### Credential Requirement Declaration

- civic processes should declare which credentials are required for participation
- requirements may allow multiple credential pathways

#### Credential Verification

- civic processes must verify presented credentials
- verification may occur through local verification libraries or through the Civic Identity verification service

#### Identity Session Compatibility

- civic processes should accept identity sessions established by hubs or dashboards
- direct access flows should still support credential presentation

#### Federation Event Publishing

- civic processes should publish participation events and updates through the federation layer

### Civic Identity Service Integration Requirements

The Civic Identity Service provides a reference identity provider for the ecosystem.

To support ecosystem interoperability, the identity service should provide the following capabilities:

#### Identity Creation

- create decentralized identifiers for citizens
- manage associated cryptographic keys

#### Authentication

- authenticate citizens using DID-based challenge-response flows
- support identity authentication for hubs, civic processes, and external civic platforms

#### Credential Issuance

- issue foundational civic credentials such as proof-of-personhood or residency credentials

#### Credential Presentation

- allow citizens to present credentials through wallet-based or managed identity flows

#### Credential Verification Support

- provide verification libraries or APIs to simplify credential validation

#### Trust Registry Access

- maintain or expose a registry of trusted credential issuers

---

## 15. Core Identity Model

The Civic.Social identity model separates identity into layers.

### Layer 1 — Decentralized Identifier

Each citizen controls a decentralized identifier conforming to the W3C DID specification.

The identifier is associated with a cryptographic key pair.

The individual controls the private key and can prove control of the identifier through cryptographic authentication.

### Layer 2 — Identity Credentials

Organizations issue credentials associated with the identifier.

Examples include:

- Proof of personhood
- Jurisdiction residency
- Voter eligibility
- Civic role credentials
- Organization membership

### Layer 3 — Credential Verification

Each civic hub, civic process, or interface defines its own credential requirements.

Examples:

A citizen assembly may require proof of residency in a city.

A civic forum may require proof of personhood.

An organizational deliberation process may require membership credentials.

Different combinations of credentials may satisfy participation requirements.

---

## 16. Credential Types

The identity system will support multiple types of verifiable credentials.

Initial categories include:

**Proof of Personhood Credentials**

Verifies that a participant represents a unique human being.

**Jurisdiction Credentials**

Verifies residency within a jurisdiction.

Examples:

- City resident
- County resident
- State resident
- Federal voter district

**Civic Role Credentials**

Verifies participation roles such as:

- Citizen assembly participant
- Facilitator
- Moderator

**Organization Membership Credentials**

Verifies membership in civic organizations.

Examples:

- Nonprofit membership
- Union membership
- Civic network participation

**Issue or Policy Credentials**

Used by advocacy organizations to recognize participation or commitments.

Examples:

Civic badges or policy pledge credentials.

---

## 17. Proof of Personhood Model

Proof of personhood is the anti-bot, anti-sybil floor for civic participation. It answers one question: is this a unique human being? It does not reveal who the person is or where they live. This makes it the lowest credential threshold in the system — sufficient for processes like open deliberation, public comment, non-binding polls, and civic forums where geographic standing is not required but bot and duplicate participation must be prevented.

Higher-trust processes such as local budget votes or citizen assemblies will typically require jurisdiction or residency credentials rather than proof of personhood alone. Proof of personhood serves as the baseline that separates "open to anyone" from "open to verified humans."

The Civic.Social identity system will support proof-of-personhood credentials that verify a decentralized identifier corresponds to a unique human participant.

Rather than relying on a single verification method, Civic.Social will support multiple verification paths.

Examples may include:

- Government identity verification
- Third-party identity verification providers
- Biometric uniqueness verification
- Existing proof-of-personhood networks

Civic hubs and civic processes may require proof-of-personhood credentials depending on the nature of the civic process.

### Pilot Position on Proof of Personhood

For the purposes of this pilot, the identity system will demonstrate the credential verification interface for proof-of-personhood — the mechanism by which a civic hub or process requests, receives, and verifies a proof-of-personhood credential — without committing to a specific proof-of-personhood provider or verification method.

The pilot may integrate with one or more existing proof-of-personhood pathways to demonstrate a working flow. The specific pathway selected for the pilot demonstration does not represent an architectural commitment. The system is designed to support multiple proof-of-personhood methods, and the credential verification interface is agnostic to the underlying verification approach.

This position reflects the current state of the proof-of-personhood landscape, in which multiple approaches are actively being developed and no single method has achieved universal adoption. The pilot focuses on building the integration layer that makes any compliant proof-of-personhood provider usable within the Civic.Social ecosystem, rather than attempting to solve the global proof-of-personhood problem.

---

## 18. Jurisdiction Credentials

Many civic processes require verifying that a participant belongs to a specific jurisdiction.

Examples include:

- municipal resident
- county resident
- state resident
- voter district

Jurisdiction credentials allow civic processes to verify eligibility without requiring citizens to reveal unnecessary personal information.

Possible issuers include:

- government agencies
- identity verification providers
- trusted civic institutions
- civic hubs themselves

---

## 19. Civic Role Credentials

Civic participation often involves roles.

Examples include:

- assembly participant
- facilitator
- moderator
- committee member
- civic researcher

These credentials can be issued by civic hubs, civic process providers, or organizations.

Role credentials allow civic processes to manage participation permissions while maintaining transparent verification.

---

## 20. Identity Verification Paths

The Civic.Social identity system will support multiple verification pathways.

Citizens may obtain credentials through different verification processes.

Examples include:

- government identity verification
- trusted third-party identity providers
- existing civic identity systems
- proof-of-personhood networks
- organization-based verification

Supporting multiple verification pathways increases accessibility while maintaining trust in civic processes.

---

## 21. Authentication and Ecosystem Access Model

Authentication allows individuals to prove control of their decentralized identifier and use their civic identity across the federated Civic.Social ecosystem.

The goal is to make a distributed civic ecosystem feel seamless to citizens even though it consists of many independent hubs, civic processes, and external civic applications.

The identity system therefore functions as a shared access layer across the ecosystem.

### Core Principle

Citizens authenticate once using their decentralized identifier and then can move fluidly across participating civic environments as long as they possess the credentials required by those environments.

This creates a user experience similar to single sign-on while preserving decentralization and the autonomy of civic hubs, civic processes, and individual participants.

### Credential-Based Access Control

Each civic hub, civic process, or civic interface defines its own participation requirements.

These requirements are expressed in terms of verifiable credentials.

Examples:

**A civic hub may require:**

- proof of personhood
- proof of residency within a specific jurisdiction

**A civic process may require:**

- organization membership
- role credentials
- jurisdiction credentials

**Participation rules may allow multiple credential paths.**

Example:

A civic forum may allow access if a participant holds *any* of the following credentials:

- city resident credential
- county resident credential
- organization membership credential

This flexible credential model allows each civic space to define access and participation rules appropriate to its context.

### Ecosystem Access Paths

Citizens may enter civic processes through multiple pathways within the ecosystem.

These pathways include:

**Direct Hub Access**

A citizen visits a Civic Hub directly and authenticates using their civic identity.

The hub verifies the credentials required for participation.

**Capability Access via Hub**

A citizen navigates from a hub interface into a civic process such as:

- citizen assemblies
- participatory budgeting
- advisory voting
- civic consultations

The process verifies the citizen's credentials through the shared identity infrastructure.

**Direct Capability Access**

Citizens may also access civic processes directly through links, embedded widgets, or external civic platforms.

In these cases the process still verifies credentials through the Civic.Social identity layer.

**Access via Civic Activity Feed or Citizen Dashboard**

Citizens may discover participation opportunities through a civic activity feed or citizen dashboard that aggregates updates from multiple civic hubs.

Selecting an action from the feed takes the citizen directly into the relevant hub or process.

Because the citizen has already authenticated using their civic identity, access can occur seamlessly without repeated sign-ins.

### Goal: Seamless Federated Civic Participation

Although the ecosystem is decentralized and federated, identity interoperability allows it to feel cohesive to citizens.

The objective is to enable a civic experience where citizens can:

- authenticate once
- discover opportunities across many civic hubs
- move between civic processes without repeated onboarding
- participate across multiple jurisdictions and organizations

This identity layer is therefore a key mechanism for turning a fragmented civic technology landscape into a coherent civic participation network.

---

## 22. Civic Identity Policy Model

Civic hubs, civic processes, and external platforms each set their own requirements for identity verification, access, and participation. The identity policy model defines how these requirements are expressed across the ecosystem.

Identity in Civic.Social is governed by three independent dimensions: assurance, disclosure, and context. These dimensions operate independently, allowing civic processes to define granular participation policies without forcing unnecessary tradeoffs between verification strength and privacy.

### Assurance

Assurance defines the level of identity verification required to participate in a civic process.

Assurance levels include:

- **None** — no identity verification required; open participation
- **Email verification** — basic confirmation of a unique communication channel
- **Proof of personhood** — verification that the participant is a unique human being
- **Residency credential** — verification of jurisdiction-based eligibility
- **Role credential** — verification of a specific civic role or organizational membership

### Disclosure

Disclosure defines what information is revealed and to whom during civic participation.

Disclosure modes include:

- **Anonymous** — no persistent identity linkage; participation is unlinkable across sessions
- **Pseudonymous** — a persistent handle allows continuity within a process without revealing real identity
- **Real identity** — optional disclosure of verified real-world identity attributes

Selective disclosure allows citizens to reveal only the specific credential attributes needed to satisfy a participation requirement — for example, proving jurisdiction residency without revealing a full address. This ensures that identity exposure is limited to the minimum necessary for each interaction.

### Context

Context is the governance layer that composes assurance and disclosure requirements into a coherent policy for a specific civic process or hub. Where assurance defines *what is verified* and disclosure defines *what is revealed*, context defines *how these dimensions are combined and enforced* within a particular civic environment.

Context governs:

- which credentials are required for each participation action
- which disclosure level is required or permitted
- which participation permissions are granted at each tier
- how identity policy is inherited or overridden across nested processes

Each civic process or hub defines its own context, and these may vary between different participation actions within the same process. A hub may define a base context that individual civic processes inherit and extend. This composability allows identity policy to be defined at the hub level and refined at the process level without requiring each process to specify requirements from scratch.

### Key Insight: High Assurance Does Not Require High Disclosure

A civic process may require verified residency credentials while still allowing anonymous or pseudonymous participation in its outputs. The dimensions of assurance and disclosure are deliberately independent.

### Standard Identity Mode Presets

Because the policy model is fully composable, Civic.Social defines standard identity mode presets that simplify configuration for common scenarios. These modes are UX abstractions of the underlying assurance and disclosure dimensions. Process designers can select a preset rather than configuring each dimension individually.

**Open** — Low assurance. No credentials required. Suitable for open consultations and public engagement processes.

**Verified Human** — Requires proof-of-personhood. Ensures that participation is limited to unique individuals without requiring identity disclosure beyond personhood. Suitable for deliberative processes and public voting on non-jurisdictional matters.

**Verified Resident** — Requires a jurisdiction credential. Ensures participation is limited to eligible residents of a specific geographic or jurisdictional area. Suitable for municipal civic participation and jurisdiction-specific voting.

**Public Profile** — Optional real identity disclosure. Citizens may choose to participate with a verified, visible public identity. This mode is opt-in and never required as a condition of participation.

Civic processes may require specific modes or allow participants to choose among available modes. Process designers may also define custom policies that go beyond these presets using the full Identity Policy Object.

---

## 23. Identity Policy Object

Civic processes and hubs may define identity requirements using a standard Identity Policy Object. This structure governs participation eligibility, visibility rules, and result stratification.

```json
{
  "assurance_requirements": ["proof_of_personhood", "resident.city"],
  "optional_credentials": ["civic_role.facilitator"],
  "disclosure_policy": {
    "public": [],
    "process": ["jurisdiction_tier"],
    "private": ["full_name", "address"]
  },
  "participation_rules": {
    "view": "open",
    "participate": "proof_of_personhood",
    "vote": "resident.city",
    "moderate": "civic_role.facilitator"
  },
  "result_stratification": ["verified_residents", "verified_humans", "open"]
}
```

**Fields:**

- **assurance_requirements** — credential types required for participation eligibility; citizens must hold at least one credential matching each entry
- **optional_credentials** — credential types that may affect participation tier or visibility without being required for basic access
- **disclosure_policy.public** — attributes visible to all participants and observers
- **disclosure_policy.process** — attributes visible only to process facilitators or operators
- **disclosure_policy.private** — attributes retained only by the participant's wallet and never transmitted to the platform
- **participation_rules** — maps each participation action (view, participate, vote, moderate) to a minimum assurance level or credential type; actions not listed inherit the process default
- **result_stratification** — an ordered list of tier labels defining how aggregate participation counts are reported; each label corresponds to a credential type or identity mode; tiers are reported as counts only and do not expose individual records

This object may be defined by civic processes or hubs and may be versioned over time as governance models evolve.

This policy model extends the existing credential-based access control architecture. The underlying DID-based identity and credential verification model remain unchanged; the policy dimensions layer additional governance capabilities on top of the existing credential access layer.

---

## 24. Result Stratification Model

Result stratification allows civic processes to lower barriers to participation while still presenting results with fine-grained distinctions based on each participant's level of identity verification. Rather than excluding participants who lack higher-level credentials, a process can accept broader participation and report results stratified by credential tier — allowing civic organizations, decision-makers, and researchers to understand participation composition without compromising individual privacy.

Example stratified result structure:

```json
{
  "verified_residents": { "count": 320 },
  "verified_humans": { "count": 800 },
  "open": { "count": 120 }
}
```

Stratification is defined in the *result_stratification* field of the Identity Policy Object for each process.

The Civic Activity Feed engagement model is extended to optionally include stratified participation metrics. These metrics report aggregate counts by credential type and verification tier. Individual participation records are not disclosed; only aggregate counts per tier are included in stratified outputs.

Stratification is optional. Processes that do not require it may omit the *result_stratification* field from their Identity Policy Object.

---

## 25. Identity Wallet Strategy

Identity wallets allow individuals to store and manage credentials.

Possible wallet implementations include:

- **Mobile identity wallets** — A mobile application storing credentials locally on the user's device.
- **Browser-based identity wallets** — A lightweight wallet integrated into a browser extension or web application.
- **Custodial identity services** — A managed service that stores credentials on behalf of users who are not comfortable managing cryptographic keys directly.

To maximize accessibility, Civic.Social may initially support both:

- non-custodial wallets for advanced users
- custodial identity services for mainstream users

Importantly, users must retain the ability to export and migrate credentials to other compatible wallets.

This prevents vendor lock-in and preserves the principles of decentralized identity.

### Wallet vs Personal Data Store

It is important to understand the distinction between the identity wallet and the Personal Data Store, as these are complementary but architecturally separate systems that serve different purposes within the Civic.Social ecosystem.

The identity wallet is primarily a credential management system. It stores verifiable credentials—proof of personhood, jurisdiction residency, civic role credentials, organizational memberships—and handles the cryptographic operations required to present and prove credentials. When a civic hub or process requests authentication, it is the wallet that signs the challenge with the citizen's DID key. When a platform requests proof of eligibility, it is the wallet that surfaces the relevant credentials and constructs a verifiable presentation. The wallet is the citizen's tool for managing their identity and proving claims about themselves.

The PDS is primarily a relationship and preference management system. It stores the citizen's social graph—their follows, hub subscriptions, community connections—along with participation preferences and user preferences that govern how they experience the ecosystem. The PDS does not handle cryptographic signing or credential presentation. Instead, it maintains the persistent state that defines the citizen's place within the ecosystem: which hubs they belong to, which organizations they are connected to, and which other citizens they have chosen to follow or connect with.

These two systems are complementary and work together to provide a complete user-controlled data layer. The wallet enables a citizen to prove who they are and what they are authorized to do. The PDS ensures that when a citizen moves to a new civic interface or application, their relationships and context move with them. Together, they ensure that a citizen's entire civic presence—both their verified identity and their accumulated relationships—remains under their control and portable across the ecosystem.

They are not the same system. A wallet without a PDS would allow a citizen to authenticate and present credentials but would not preserve their subscriptions or social graph when they switch applications. A PDS without a wallet would preserve their relationships but would not enable them to prove their identity or meet eligibility requirements. Both are necessary for the full vision of portable, user-controlled civic participation.

---

## 26. Privacy Model

The Civic.Social identity system aims to protect user privacy.

Possible privacy mechanisms include:

- **Selective disclosure of credential attributes** — Citizens may prove specific claims from their credentials without revealing all credential data. For example, a citizen might prove they reside within a jurisdiction without revealing their full address.
- **Minimal data retention by civic platforms** — Civic hubs and processes should minimize persistent storage of citizen data, retaining only information necessary for participation management and audit.

In many cases a citizen may prove statements such as:

"I reside within this jurisdiction."

without revealing full address or personal details.

The pilot will explore the appropriate balance between usability and advanced privacy technologies.

### Context-Specific Disclosure

Identity disclosure requirements may vary by context. A citizen may participate anonymously in a public consultation while participating pseudonymously in a deliberative process and providing verified residency credentials in a binding vote—all using the same underlying civic identity. The disclosure level for each interaction is determined by the Identity Policy Object defined for that process, not by a global identity setting.

### Audience-Scoped Identity Sharing

Identity attributes are disclosed selectively based on the audience and context. Attributes shared with the public, with process facilitators, and retained privately are governed by the *disclosure_policy* fields of the Identity Policy Object defined for each process. Citizens can verify what information is visible to each audience tier before choosing to participate.

### User-Controlled Visibility Per Interaction

Citizens retain control over their visibility level within the bounds set by the applicable identity policy. Where civic hubs, civic processes, or other platforms permit multiple disclosure modes, citizens may choose their preferred level of disclosure for each individual interaction. This preserves citizen autonomy while allowing each civic space to enforce minimum assurance requirements appropriate to its governance context.

---

## 27. Interoperability Standards Foundation

The Civic Identity architecture builds on widely adopted open identity standards developed through the World Wide Web Consortium (W3C). These standards form the foundation of a rapidly growing decentralized identity ecosystem that includes identity wallets, credential issuers, verification services, and large-scale identity deployments.

Decentralized identity technologies based on W3C standards are already being adopted in real-world applications. Notable deployments and initiatives include:

- **Mobile driver's licenses and digital ID programs**, including deployments such as the California Mobile Driver's License and similar initiatives in multiple U.S. states
- **The European Digital Identity Wallet (EUDI) initiative** [9], which is building a continent-scale identity wallet infrastructure for EU citizens
- **Educational credential ecosystems**, including **Open Badges** and university-issued verifiable diplomas and certificates
- **Enterprise and decentralized identity networks**, such as the **Microsoft ION DID network** built on Bitcoin
- **Workforce and professional credential systems**, including initiatives by **Jobs for the Future (JFF)** to build verifiable credential wallets for skills-based talent marketplaces, and **Credential Engine's** registry infrastructure for workforce credentials
- **Supply chain and organizational identity systems**, including the **UN Transparency Protocol (UNTP)** which uses verifiable credentials for supply chain traceability across pharmaceuticals, automotive, food, and critical minerals, and the **W3C Verifiable Supply Chain Community Group** developing VC profiles for industry verticals

These deployments demonstrate that W3C decentralized identity standards are not experimental technologies but part of an emerging global identity infrastructure. By building on these standards, Civic.Social can interoperate with existing identity wallets, credential issuers, and verification tools rather than creating a proprietary identity system.

### Utah's State-Endorsed Digital Identity (SEDI) Framework

Of particular relevance to the Civic.Social identity architecture is Utah's State-Endorsed Digital Identity (SEDI) program, established through legislation in 2025 (SB 260) and amended in 2026 (SB 275) with unanimous legislative support. SEDI is a rights-first framework that defines digital identity as something inherent to a person and endorsed by the state, rather than bestowed by the state — a philosophical alignment with Civic.Social's commitment to citizen-controlled identity. The framework builds on W3C Verifiable Credentials, ISO/IEC mobile document standards, OpenID for Verifiable Credential Issuance and Presentation (OpenID4VCI [6] / OpenID4VP [4]), and FIDO [7] authentication — the same standards foundation that the Civic.Social identity architecture adopts. SEDI requires that privacy protections be enforced through technology, with credentials supporting selective disclosure so that holders can prove only what is necessary for a given interaction.

Utah's SEDI framework represents a potential model and partnership opportunity for Civic.Social. Because Civic.Social and SEDI build on the same open standards and share the same principles of citizen control and selective disclosure, the two frameworks are naturally compatible. Designing the Civic.Social identity layer to align with the SEDI framework would allow civic identity to interoperate with state-endorsed identity infrastructure, strengthening the legitimacy and practical utility of both systems. As other states evaluate similar frameworks — which Utah's program is explicitly designed to encourage — compatibility with the SEDI model positions Civic.Social's identity architecture to integrate with an expanding network of state-level digital identity deployments. The SEDI program's emphasis on individual control, selective disclosure, and open standards makes it a natural alignment point for the Civic.Social ecosystem.

For more information, see the SEDI program overview [10].

Relevant standards include:

- W3C Decentralized Identifiers (DIDs)
- W3C Verifiable Credentials JSON-LD [3] credential formats
- OpenID for Verifiable Presentations (OpenID4VP) [4]
- SIOPv2 (Self-Issued OpenID Provider) authentication flows [5]

These standards allow identity credentials to interoperate across multiple platforms and identity providers.

---

## 28. Credential Issuers

Credentials in the Civic.Social ecosystem may be issued by multiple types of organizations.

Examples include:

- government agencies
- identity verification providers
- civic hubs
- civic process providers
- advocacy organizations

Different civic processes may trust different issuers depending on the requirements of the process.

---

## 29. Civic Identity Service Model

The Civic.Social ecosystem requires a practical identity service that citizens and civic platforms can use immediately. While the system is designed around decentralized identity standards, the ecosystem cannot rely solely on standards and hope that third parties will provide identity infrastructure.

Therefore the Civic.Social initiative intends to build a **reference Civic Identity Service** that operates as public-interest digital infrastructure for the networks.

This service will provide a complete identity experience for citizens while remaining compatible with decentralized identity standards so that alternative providers can interoperate with the ecosystem.

### Objectives of the Civic Identity Service

The Civic Identity Service will aim to provide:

- creation of decentralized identifiers (DIDs) for citizens
- authentication services for civic hubs and civic processes
- issuance of core civic credentials
- wallet infrastructure for storing and presenting credentials
- credential verification services for participating platforms

The service is intended to function as a **public-interest identity utility** supporting democratic participation.

### Core Capabilities of the Service

A production Civic Identity Service may include the following components.

**Identity Creation**

Citizens can create a decentralized civic identity associated with a DID and cryptographic key pair.

**Credential Issuance**

The service may issue foundational civic credentials such as:

- proof-of-personhood credentials
- residency and jurisdiction credentials
- basic civic participation credentials

**Wallet Management**

The service may provide a default wallet experience allowing citizens to store and present credentials.

To maintain decentralization principles, users must always be able to export credentials to external wallets.

**Authentication Infrastructure**

The service will allow civic hubs, civic processes, and external civic applications to authenticate citizens using their decentralized identity.

**Credential Verification**

Platforms can verify required credentials through standardized verification protocols.

### Open Ecosystem Principle

Although Civic.Social may operate a reference Civic Identity Service, the ecosystem is intentionally designed to allow multiple identity providers.

Any identity provider that supports the relevant decentralized identity standards should be able to interoperate with the ecosystem.

This approach ensures that:

- Civic.Social does not become a gatekeeper
- citizens can migrate identities between providers
- innovation and competition remain possible

The Civic.Social reference service exists primarily to ensure that identity infrastructure is available to the ecosystem from the outset.

### Long-Term Vision

Over time, the Civic Identity Service may evolve into a public-interest digital utility governed by a consortium of civic organizations, public institutions, and ecosystem stakeholders.

The long-term objective is to maintain an identity layer that is:

- open
- interoperable
- privacy-respecting
- resistant to commercial exploitation

This governance model aligns the operation of civic identity infrastructure with the broader democratic mission of the Civic.Social ecosystem.

---

## 30. Identity Governance Model

Initially, the Civic.Social consortium may curate a registry of trusted credential issuers.

Over time this registry could evolve into a more decentralized trust network.

**Governance questions include:**

- who can issue credentials
- how credential schemas are defined
- how trust registries are maintained

These governance structures will likely evolve as the ecosystem grows.

---

## 31. Minimum Viable Civic Identity Stack

The Civic Identity architecture builds on identity standards developed through the World Wide Web Consortium (W3C). In particular, the pilot relies on **Decentralized Identifiers (DIDs)** and **Verifiable Credentials (VCs)**, which enable portable, cryptographically verifiable identity claims across independent systems.

For the pilot phase, the goal is not to implement a fully mature global identity system but to **demonstrate a working reference implementation that proves decentralized civic identity can support trusted participation across multiple civic hubs and civic processes.**

The Minimum Viable Civic Identity Stack defines the core technical components required for this demonstration.

### 1. DID Method

The system will use **W3C Decentralized Identifiers (DIDs)** as the base identity primitive.

Each citizen controls a DID associated with a cryptographic key pair.

The DID enables:

- authentication without centralized accounts
- cryptographic proof of identifier ownership
- portable identity across platforms

Possible DID methods to evaluate:

- did:key (simple cryptographic identifiers)
- did:web (web-hosted DID documents)
- did:tdw (Trust DID Web — an evolution of did:web with verifiable history and cryptographic trust)

The pilot will begin with the most pragmatic DID method and evolve as the ecosystem matures.

### 2. Verifiable Credential Format

Credentials will follow the **W3C Verifiable Credentials Data Model.**

These credentials represent claims about a citizen that can be verified cryptographically without requiring the issuing organization to be contacted.

Example credential categories for the pilot:

**Proof of Personhood**

Verifies that the DID corresponds to a unique human participant.

**Jurisdiction Credentials**

Examples:

- resident.city
- resident.county
- resident.state

**Organization Membership**

Examples:

- civic organization membership
- nonprofit membership
- coalition participation

**Civic Role Credentials**

Examples:

- citizen assembly participant
- facilitator
- moderator

These credential schemas will be documented and published so other civic organizations can issue compatible credentials.

### 3. Credential Issuance Infrastructure

The pilot must demonstrate how organizations issue credentials to citizens.

Key functions include:

- credential creation
- cryptographic signing by the issuer
- delivery to the citizen's wallet

Potential issuers in the pilot may include:

- identity providers
- civic hubs
- civic organizations

Issuers must publish public verification keys so credentials can be validated across the ecosystem.

### 4. Credential Verification Service

Civic hubs and civic processes need a simple mechanism to verify credentials.

The pilot will implement a **credential verification service or SDK** that allows platforms to:

- request required credentials
- verify issuer signatures
- verify credential integrity
- validate expiration and revocation status

This verification layer is critical for making decentralized credentials usable in real civic applications.

### 5. Authentication Flow

Authentication will rely on cryptographic challenge-response using the citizen's DID key.

Typical flow:

1. Citizen attempts to access a hub or process
2. Platform requests authentication
3. Citizen signs a challenge with their DID key
4. Platform verifies control of the identifier

After authentication, the system requests any credentials required for participation.

This full challenge-response flow typically occurs once per session. Once a citizen has authenticated, session continuity mechanisms — similar to standard web session cookies — allow the citizen to move between hubs and processes without repeated authentication. A civic process accessed through an already-authenticated hub can reuse the existing identity session rather than requiring a new challenge-response.

This process enables **single sign-on-like behavior** across the civic ecosystem without relying on centralized identity providers.

### 6. Identity Wallet

Citizens require a mechanism to store and present credentials.

The pilot will support one or more of the following wallet models:

**Browser-based wallet**

A lightweight wallet integrated into a browser extension or web application.

**Mobile identity wallet**

A mobile application storing credentials locally on the user's device.

**Custodial identity service**

A managed service that stores credentials on behalf of users who are not comfortable managing cryptographic keys directly.

Regardless of the model, citizens must retain the ability to export and migrate credentials to other compatible wallets.

### 7. Credential Presentation Protocol

When a hub or process requires verification, the wallet presents the relevant credentials.

The pilot may use emerging standards such as:

- **OpenID Connect for Verifiable Presentations** [4]
- **SIOPv2 (Self-Issued OpenID Provider v2)** [5]

These protocols enable identity wallets to interact with web platforms using familiar authentication flows.

### 8. Trust Registry

Participating platforms must know which credential issuers they trust.

The pilot will implement a **basic trust registry** listing trusted issuers and credential schemas.

Initially this registry may be curated by the Civic.Social consortium.

Over time it could evolve into a decentralized governance model.

### 9. Developer Integration Tools

To encourage adoption, the pilot will produce integration tools including:

- credential verification libraries
- authentication APIs
- developer documentation
- example integrations with civic hubs and civic processes

These tools will allow civic technology organizations to integrate decentralized identity without needing deep expertise in identity infrastructure.

The goal of the Minimum Viable Civic Identity Stack is therefore to demonstrate that decentralized civic identity can function as a practical interoperability layer across the Civic.Social ecosystem.

---

## 32. Minimum Viable Pilot Scope

This section defines the boundary between what the pilot will build and demonstrate and what remains part of the broader architectural vision for future phases.

### What the Pilot Will Demonstrate

The pilot will demonstrate a complete end-to-end civic identity participation loop:

1. A citizen creates a decentralized civic identity (DID + key pair) through a prototype Civic Identity Service.
2. The citizen receives verifiable credentials such as a jurisdiction residency credential and potentially a proof-of-personhood credential.
3. The citizen authenticates to a Civic Hub using their decentralized identity.
4. The Civic Hub verifies the citizen's credentials and grants access to a civic process.
5. The citizen participates in the civic process (e.g., an advisory vote or consultation).
6. The citizen accesses a second civic environment (a different hub or civic process) using the same identity — without creating a new account or repeating identity verification.

This loop demonstrates the core value proposition: portable, credential-verified civic participation across independent platforms.

### What is In Scope

The pilot will produce:

- A minimum viable Civic Identity Service capable of creating DIDs, issuing credentials, and supporting authentication
- Credential schemas for jurisdiction residency and potentially proof-of-personhood
- A credential verification SDK or library for civic hubs and civic processes
- Authentication integration with at least one Civic Hub platform and one civic process
- A basic trust registry of recognized credential issuers
- Developer documentation enabling third-party integration
- A cost and operational analysis for building and running a civic identity service

### What is Explicitly Out of Scope

The following are part of the architectural vision but are not deliverables of this pilot:

- Production-scale identity infrastructure (the pilot produces a working prototype, not a production system)
- Multiple identity provider interoperability (the pilot builds the reference service; multi-provider support is a future phase)
- Full self-sovereign key custody for all users (the pilot will support managed custody with export capability; full self-custody is supported architecturally but not required for pilot participants)
- Personal Data Store implementation (the PDS is defined architecturally in this document but is not a deliverable of this pilot — it will be implemented in a subsequent phase)
- Integration with more than two civic platforms (the pilot demonstrates the pattern with a small number of integrations; broader adoption follows)

### Prototype vs. Production Intent

The pilot is intended to produce a working prototype that demonstrates the viability and value of decentralized civic identity. It is not intended to produce production-ready infrastructure. However, the architectural decisions, credential schemas, and integration patterns produced during the pilot are intended to serve as the foundation for production development. The goal is to prove the concept, validate the architecture, and produce clear guidance for scaling.

---

## 33. Pilot Phases

**Phase 1 — Identity Architecture Design**

Define identity architecture, credential schemas, and authentication flows.

**Phase 2 — Credential Issuance Prototype**

Prototype issuance of key credential types including proof-of-personhood and jurisdiction credentials.

**Phase 3 — Authentication Integration**

Enable identity authentication across civic hubs and civic processes.

**Phase 4 — Ecosystem Testing**

Demonstrate identity interoperability across multiple civic environments.

---

## 34. Expected Deliverables

The Civic Identity Pilot is intended to produce both architectural knowledge and practical infrastructure for the Civic.Social ecosystem.

Expected deliverables include:

**Identity architecture specification**

A detailed description of the decentralized civic identity architecture including authentication flows, credential models, and interoperability standards.

**Credential schema definitions**

Standard schemas for key civic credentials such as proof of personhood, jurisdiction residency, organization membership, and civic roles.

**Credential issuance reference implementation**

Prototype infrastructure demonstrating how civic organizations and identity providers can issue verifiable credentials to citizens.

**Credential verification SDK**

Developer tools allowing civic hubs and civic process providers to easily verify credentials required for participation.

**Reference Civic Identity Service Design**

A reference architecture for a production-scale Civic Identity Service that could support the broader Civic.Social ecosystem. This design will specify:

- identity provider architecture
- credential issuance services
- wallet integration strategy
- trust registry governance
- authentication and presentation protocols

The pilot may implement a lightweight prototype of this service, but at minimum it will produce a clear technical design.

**Civic Identity Cost and Operations Analysis**

The pilot will estimate the cost and operational requirements of running a shared Civic Identity Service for the ecosystem.

This analysis will explore:

- infrastructure costs
- credential issuance costs
- verification infrastructure
- security and compliance requirements
- governance and trust registry management

The goal is to determine the long-term feasibility of operating a shared civic identity layer for the network.

**Pilot deployments**

Demonstrations of identity authentication and credential verification across multiple civic hubs and civic processes.

**Developer documentation**

Documentation enabling other civic technology organizations to integrate with the Civic.Social identity infrastructure.

**Live demonstration environment**

An interactive environment where funders, ecosystem partners, and prospective civic hub operators can experience civic identity flows firsthand — including credential issuance, authentication, and participation in a civic process using verifiable credentials.

**Pilot report**

A written report documenting what was built, what was validated, key findings, and recommendations for moving toward production infrastructure. This report will also summarize lessons learned around technical feasibility, integration complexity, and ecosystem readiness.

---

## 35. Civic Identity User Journey

The Civic Identity Pilot will also define a user experience model for how citizens interact with decentralized civic identity across the ecosystem.

This user journey describes the complete civic identity experience across the ecosystem. The identity pilot will build and demonstrate the identity creation, credential issuance, authentication, and verification steps. Other steps — such as ecosystem onboarding and feed-based discovery — depend on components from other Civic.Social pilots and are included here to illustrate how civic identity fits into the broader participation experience.

Example user journey:

**1. Identity Creation**

A citizen creates a Civic Identity using a wallet or identity provider compatible with Civic.Social.

**2. Credential Issuance**

The citizen receives one or more credentials such as:

- residency credentials
- organization memberships
- proof of personhood

**3. Ecosystem Onboarding**

The citizen subscribes to civic hubs, communities, or organizations of interest.

**4. Discovery**

The citizen views updates in a civic feed or dashboard aggregating opportunities across subscribed hubs.

**5. Participation**

When the citizen selects a civic action (such as joining a consultation or assembly), the system requests any required credentials.

**6. Credential Verification**

The citizen's wallet presents the required credentials, which are verified automatically.

**7. Civic Participation**

The citizen participates in the civic process without needing to create new accounts or repeat eligibility verification.

This flow demonstrates how decentralized identity enables a seamless participation experience across a federated civic ecosystem.

### PDS and Continuity Across Applications

Throughout this journey, the citizen's Personal Data Store plays a critical role in maintaining continuity.

When the citizen subscribes to civic hubs in step 3, those subscriptions are recorded in their PDS rather than solely within the application they happen to be using. This means that if the citizen later switches to a different civic dashboard or interface, their hub subscriptions, notification preferences, and social connections travel with them because the data that defines their place in the ecosystem is not locked inside any single application.

Similarly, when the citizen follows other participants or organizations during the course of their civic engagement, these social connections are stored in their PDS. The citizen's social graph is their own, not the property of the interface they used to create those connections. If the citizen decides to move from one civic dashboard to another, or from one civic interface to a completely different one, their relationships and context move with them.

The separation between authentication and continuity becomes particularly clear in this journey. Authentication comes from the Identity Layer: the citizen's DID and credentials prove who they are and what they are authorized to do. Continuity comes from the PDS Layer: the citizen's subscriptions, preferences, and social connections ensure they do not have to start from scratch every time they use a new application.

Together, these layers ensure that the citizen's entire civic presence—both their verified identity and their accumulated relationships—remains portable and under their control.

### Social Graph Portability

Consider a citizen who has been participating in their local civic ecosystem for several months — following municipal hubs, subscribing to civic organizations, and connecting with other citizens during a participatory budgeting process. All of these relationships are stored in the citizen's PDS, not in the application they happen to be using.

If the citizen switches to a different civic dashboard or interface, the new application reads their PDS and immediately presents their existing hub subscriptions, organizational follows, and social connections. Their identity and credentials transfer through the Identity Layer. The citizen does not need to re-subscribe, re-follow, or rebuild any part of their civic network. The specific application is replaceable; the citizen's identity and relationships are not.

---

## 36. Example Civic Identity Use Cases

To guide development, the pilot will explore several practical use cases.

**Local Civic Hub Participation**

A resident of a city joins the city's Civic Hub and participates in a civic process (e.g., participatory budgeting) using a verified residency credential.

**Cross-Jurisdiction Civic Participation**

A citizen participates in multiple civic hubs such as their city, county, and state hubs using the same civic identity.

**Civic Organization Participation**

A nonprofit organization hosts a civic deliberation process that requires membership credentials issued by the organization.

**Citizen Assembly Participation**

A civic hub runs a citizen assembly requiring a verified residency credential for the city.

**External Civic Platform Integration**

A civic technology platform integrates Civic.Social identity so that citizens can participate without creating a separate account.

These use cases help demonstrate how decentralized civic identity supports flexible participation across many civic contexts.

---

## 37. Success and Validation Criteria

### Deliverable Criteria

The pilot will be considered successful upon production of:

- **Complete end-to-end identity flow** — identity creation, credential issuance, authentication, and credential verification — executed across at least two independent civic environments using synthetic or pilot participant data
- **Working prototype Civic Identity Service** — capable of creating DIDs, issuing credentials, and supporting authentication
- **Credential schemas** — that can be issued, presented, and verified by independent civic hubs and processes
- **Cross-platform authentication** — a single identity used to participate in multiple civic environments without re-onboarding
- **Credential verification SDK** — a library that a civic hub or process developer can integrate with reasonable effort
- **Developer documentation** — sufficient for a third-party civic technology platform to begin integration

### Technical Validation

The pilot will validate the following through developer-driven test flows using synthetic data:

- **Identity creation flow** — the full flow of creating a decentralized civic identity (DID + key pair) can be executed end to end through the prototype service
- **Credential issuance flow** — a credential issued by the prototype service is valid, cryptographically signed, and deliverable to a wallet
- **Credential verification** — a credential issued by the prototype service can be independently verified by a civic hub or process using the verification SDK
- **Cross-platform authentication** — a single identity can authenticate to at least two independent civic environments without re-onboarding or re-verification
- **Credential verification latency** — credential verification completes within acceptable response times for interactive civic participation
- **Interoperability** — credentials and authentication flows conform to the W3C and OpenID standards specified in the architecture, ensuring compatibility with other compliant systems

### Future Usability Evaluation

User-facing metrics — such as identity creation completion rates, onboarding drop-off points, and user experience friction — will be evaluated in subsequent phases when the prototype is stable enough for real participant testing. The pilot will document recommended usability evaluation criteria to inform that future work.

---

## 38. Estimated Development Effort

**Estimated timeline:** 9-12 months

**Estimated budget range:** $400,000 - $700,000

Budget will depend on scope of identity verification infrastructure and partner integrations.

---

## 39. Relationship to Other Civic.Social Pilots

The Civic Identity Pilot supports:

- **Civic Process Plugin Pilot** — by enabling credential verification for participation.
- **Civic Activity Feed Pilot** — by enabling identity-verified participation notifications.
- **Civic Hubs Pilot** — by enabling trusted civic participation within jurisdictional hubs.

### Relationship to the Civic Credentialing & Profiles Pilot

The Civic Identity Pilot and the Civic Credentialing & Profiles Pilot are tightly linked but address distinct layers of the ecosystem.

The Civic Identity Pilot owns the foundational infrastructure: the Citizen Node, decentralized identifiers, verifiable credential issuance and verification, authentication flows, and the reference Civic Identity Service. This pilot establishes the mechanism by which citizens prove who they are and what claims they can make about themselves.

The Civic Credentialing & Profiles Pilot builds on this foundation to create the visible trust and reputation layer. Where the Identity Pilot establishes credentials for authentication and access — proving eligibility to participate in civic hubs and processes — the Credentialing Pilot focuses on the public display of credentials as civic badges and social trust signals. Politicians, civic organizations, citizens, and other entities may choose to publicly display verifiable badges representing accomplishments, endorsements, civic participation history, or organizational affiliations. These public credential displays serve a fundamentally different purpose than the identity credentials used for access and authorization: they build social trust, signal civic engagement, and create visible accountability within the ecosystem.

These two pilots benefit from concurrent execution. Identity infrastructure must exist before credentials can be meaningfully displayed, but credential use cases also inform identity architecture decisions — particularly around credential schema design and issuer trust models. Funders considering support for either pilot should be aware that joint funding accelerates both and reduces integration risk.

---

## 40. Pilot Team Roles

**Project Lead**

Responsible for overall project coordination, ecosystem architecture, and pilot implementation.

**Technical Lead**

Responsible for identity architecture, system design, and coordination of technical development.

**Identity Infrastructure Engineers**

Responsible for implementation of decentralized identity systems, credential issuance, and verification infrastructure.

**Civic Technology Integration Engineers**

Responsible for integrating civic identity infrastructure with hubs and civic processes.

**Product and UX Design**

Responsible for citizen identity onboarding flows and wallet usability.

**Ecosystem Partnerships**

Responsible for collaboration with civic organizations, technology partners, and pilot communities.

[Specific individuals and roles to be determined.]

---

## 41. Potential Pilot Partners

Potential partners may include:

**Identity Infrastructure Partner**

[Digital Bazaar](https://digitalbazaar.com) (potential partner) or other decentralized identity infrastructure providers. Any identity infrastructure partner should be deeply acquainted with W3C decentralized identity standards and the broader credential ecosystem.

**Civic Technology Partners**

Civic process organizations and communities running civic hubs — including organizations developing civic participation tools and platforms.

**Civic Hub Partners**

The Civic.Social project is building a reference implementation of a civic hub. Pilot partners may include organizations interested in operating civic hubs using this reference implementation or their own compatible platforms.

**Civic Organizations**

Municipalities, nonprofits, and civic organizations interested in piloting civic identity infrastructure.

Final partners for each category are to be confirmed as the pilot moves into execution.

---

## 42. Estimated Budget

**Estimated total pilot cost: $400,000 - $700,000**

**Major cost categories include:**

**Identity infrastructure development**

Design and implementation of decentralized identity services, credential issuance, and authentication systems.

**Credential verification tools**

Development of verification libraries and APIs for civic hubs and civic processes.

**Integration development**

Integration with at least one civic hub platform and one civic process provider.

**Security and cryptographic engineering**

Key management, credential security, and verification infrastructure.

**Developer documentation and SDKs**

Tools enabling external civic technology platforms to integrate with Civic Identity.

**Pilot deployments and testing**

Deployment with partner civic organizations and testing of participation flows.

---

## 43. Timeline

**Estimated duration: 9-12 months**

**Phase 1 — Architecture and System Design (2-3 months)**

Finalize identity architecture, credential schemas, and integration models.

**Phase 2 — Identity Infrastructure Development (3-4 months)**

Develop decentralized identity services, credential issuance systems, and verification tools.

**Phase 3 — Ecosystem Integration (2-3 months)**

Integrate civic identity with pilot hubs and civic processes.

**Phase 4 — Demonstration, Reporting, and Handoff (2 months)**

Prepare a live demonstration environment for funders and ecosystem partners. Conduct stakeholder demos showcasing civic identity flows end-to-end. Produce a pilot report documenting findings, technical feasibility, and recommendations for production. Package deliverables — including architecture specs, credential schemas, and developer documentation — for handoff to the next phase of development.

---

## 44. Risks and Mitigations

**Adoption Risk**

**Risk:** Civic organizations may be reluctant to adopt decentralized identity infrastructure that feels unfamiliar or unproven.

**Mitigation:** Build legitimacy around the approach by communicating mainstream adoption of decentralized identity standards across industries, demonstrating clear short-term integration benefits alongside long-term architectural advantages, and providing reference implementations that simplify adoption for civic organizations.

**Identity Usability Challenges**

**Risk:** Citizens may not trust or feel confident using a new identity system, even if the underlying technology works well.

**Mitigation:** Design identity experiences so that end users do not need to understand the underlying architecture — they simply need to be able to use it and trust that it works across civic platforms. Invest in familiar onboarding flows, build trust around privacy protections, and support both wallet-based and managed identity experiences so users can participate at their comfort level.

**Fragmentation of Credential Issuers**

**Risk:** Multiple organizations may issue inconsistent or incompatible credentials.

**Mitigation:** Maintain a trust registry and publish credential standards. Provide reference schemas and issuance infrastructure.

**Technical Complexity of Decentralized Identity**

**Risk:** Decentralized identity infrastructure may be technically complex to implement and operate.

**Mitigation:** Collaborate with experienced identity infrastructure providers. Build open-source libraries and integration tools.

---

## 45. Conclusion

The Civic Identity Pilot explores how decentralized identity infrastructure can enable trusted participation across a federated civic ecosystem.

By combining open standards with a working reference implementation, the pilot aims to demonstrate how civic identity can function as digital public infrastructure for democratic participation.

This work lays the groundwork for a networked civic ecosystem where citizens can participate across jurisdictions, organizations, and civic processes using a portable civic identity.

---

## References

[1] W3C, "Decentralized Identifiers (DIDs) v1.1," W3C Candidate Recommendation, 2026. https://www.w3.org/TR/did-1.1/

[2] W3C, "Verifiable Credentials Data Model v2.0," W3C Recommendation, 2025. https://www.w3.org/TR/vc-data-model-2.0/

[3] W3C, "JSON-LD 1.1," W3C Recommendation. https://www.w3.org/TR/json-ld11/

[4] OpenID Foundation, "OpenID for Verifiable Presentations (OpenID4VP) 1.0." https://openid.net/specs/openid-4-verifiable-presentations-1_0.html

[5] OpenID Foundation, "Self-Issued OpenID Provider v2 (SIOPv2)." https://openid.net/specs/openid-connect-self-issued-v2-1_0.html

[6] OpenID Foundation, "OpenID for Verifiable Credential Issuance (OpenID4VCI) 1.0." https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html

[7] FIDO Alliance, "FIDO Authentication Standards." https://fidoalliance.org/specifications/

[8] W3C, "ActivityPub," W3C Recommendation, 2018. https://www.w3.org/TR/activitypub/

[9] European Commission, "European Digital Identity Wallet (EUDI)." https://commission.europa.eu/topics/digital-economy-and-society/european-digital-identity_en

[10] State of Utah, "State-Endorsed Digital Identity (SEDI): Protecting Liberty in a Connected World," Version 1.0, 2025. https://privacy.utah.gov/wp-content/uploads/SEDI_ProtectingLiberty.pdf

---

## 46. Open Questions for Further Design

Several architectural questions remain to be explored.

### Proof of Personhood in the Pilot

**Question:** Should proof of personhood be supported in the pilot, and if so, should it be in scope for Phase 1?

**Discussion:** There is growing momentum in the proof-of-personhood community, and bridging with that momentum could generate cross-support for Civic.Social if proof-of-personhood practitioners see a compelling civic use case for their work. The question is whether supporting proof of personhood in the pilot is worth the additional effort and cost, or whether it should be deferred to a later phase while the pilot focuses on credential-based identity flows.

### Jurisdiction Credential Issuers

**Question:** Which organizations should issue residency credentials?

**Discussion:** Jurisdiction credentials could be issued by government agencies, identity verification providers, trusted civic institutions, or civic hubs themselves. The design question is how to establish trust in jurisdiction credential issuers and whether civic hubs should be able to issue their own jurisdiction credentials.

### Identity Wallet Strategy

**Question:** Should Civic.Social build a reference identity wallet, or integrate with existing open source wallets?

**Discussion:** Open source credential wallets already exist — most notably the [EU Digital Identity Wallet (EUDI Wallet)](https://github.com/eu-digital-identity-wallet), which is being developed under an open source license and will be required for all EU member states by the end of 2026. The design question may not be whether to build a wallet from scratch, but whether to adopt and adapt an existing open source wallet for civic use cases, or simply ensure compatibility with multiple existing wallets through standard protocols.

### Privacy Model

**Question:** What level of selective disclosure should be required for initial deployments?

**Discussion:** Selective disclosure offers strong privacy guarantees while remaining practical to implement. The design question is which credential attributes require selective disclosure support in the pilot and how to balance privacy granularity with implementation complexity.

### Credential Trust Registry

**Question:** How should trusted credential issuers be defined and governed?

**Discussion:** Initially the Civic.Social consortium might curate a trust registry. Over time the registry could evolve into a decentralized governance model. The design question is how to establish and maintain trust in credential issuers at scale.

### Custodial Identity Services

**Question:** Should Civic.Social provide optional custodial identity services for non-technical users?

**Discussion:** Decentralized identity requires users to manage cryptographic keys. Some users may not be comfortable with this. Custodial services could offer key management on behalf of users while maintaining the ability to export credentials. The design question is whether to offer this service and how to ensure it remains truly optional.

### Identity Policy Context Inheritance

**Question:** Should hub-level identity policies be automatically inherited by civic processes running within that hub, with processes allowed to add requirements but not reduce them?

**Discussion:** Context governance defines how identity policy cascades through nested processes. The design question is whether civic processes should inherit hub-level policy constraints or define requirements independently.

### Open Mode and Minimum Identity Requirements

**Question:** How low should hubs and processes be able to set their identity verification requirements, and what role does proof of personhood play at the lowest tier?

**Discussion:** The identity policy model allows hubs and processes to define their own assurance requirements. The design question is how far down those requirements can go. Proof of personhood could serve as the floor for open mode — the minimum threshold that provides anti-bot and anti-sybil protection without requiring credential disclosure. Below that, hubs could theoretically allow participation with only email verification or even no identity requirement at all, though very low thresholds risk attracting automated participation. The architecture should allow hub and process operators to make this decision for themselves, while clearly communicating the tradeoffs of each threshold level.

### Pseudonymous Handle Scope

**Question:** Should pseudonymous handles in the "Pseudonymous" disclosure mode be scoped to a single process, to a single hub, or be portable across the ecosystem?

**Discussion:** Process-scoped handles maximize privacy but prevent reputation or continuity across civic spaces. Ecosystem-scoped handles enable richer participation histories but create persistent identifiers that could be used to profile behavior. The design question is which approach better serves civic participation while maintaining privacy.

### PDS Scope and Governance

**Question:** What is the appropriate scope of the Personal Data Store for subsequent phases beyond the pilot?

**Discussion:** In the pilot, the PDS is scoped to relationships, preferences, and references. Should subsequent phases expand the PDS to include additional categories of user-owned data, such as participation history or reputation signals? How should PDS data be governed—should citizens have granular control over which applications can read and write to their PDS, and should there be a consent framework governing PDS access? These questions will become increasingly important as the ecosystem grows and more applications seek to access citizen data.

### Social Graph Interoperability

**Question:** How should the citizen's social graph interact with social graphs from other federated systems?

**Discussion:** If a citizen already has connections in an ActivityPub-based social network, should those connections be importable into the Civic.Social PDS? What standards or protocols should govern cross-ecosystem social graph portability, and what are the privacy implications of making civic social connections visible across different federated networks?

---

**Version:** 0.5
**Status:** Draft
**Last updated:** April 9, 2026
**Contact:** contact@civic.social
