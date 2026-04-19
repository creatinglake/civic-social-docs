---
status: review
last-reviewed: 2026-04-19
owners: [adam]
version: 0.1
---

# Civic Hubs Pilot — Technical Brief

Civic.Social Infrastructure Program

---

## Executive Summary

Democratic participation increasingly depends on digital infrastructure, yet communities lack digital civic spaces they own and govern. Civic life online is scattered across inadequate channels — commercial social media platforms optimized for engagement rather than democratic outcomes, isolated government portals, email lists, and disconnected civic tools. Communities have no shared digital spaces where citizens can discover participation opportunities, engage in structured democratic processes, and organize toward collective action — spaces governed by the community itself, not by a technology company.

Civic Hubs address this gap. A Civic Hub is a community-operated digital civic space where citizens gather, participate in structured civic processes — advisory votes, deliberations, participatory budgets, public consultations — and interact within their local civic context. Each hub is independently operated by a community, jurisdiction, or organization, while interoperating with other hubs through shared identity, federation protocols, and open standards. Hubs are the central interaction environment of the Civic.Social ecosystem: the spaces where civic participation actually happens.

The Civic Hubs Pilot will design, deploy, and test Civic Hubs with real communities. It will produce a Civic Hub Compliance Specification defining what it means to be a compliant hub, deploy reference hubs using the Civic.Social Hub Engine, host real civic processes with real participants, and demonstrate that independent hubs can discover and interoperate with each other. The pilot is one component of the broader Civic.Social Infrastructure Program, which includes complementary pilots for Civic Identity, Civic Processes, the Civic Activity Feed, Civic Credentialing, and the Citizen Dashboard.

---

## Purpose

This brief describes the technical scope and architecture of the Civic Hubs Pilot for prospective technical collaborators — including developers, technical leads, implementation partners, and civic technology organizations who may contribute to the pilot's execution.

The Civic.Social Hub Engine is already under active development. This is not a request for proposals to build hub software from scratch. We are looking for technical partners who can help execute, refine, and scale the pilot — whether as contracted technical leads, development collaborators, integration partners, or civic technology platform partners exploring interoperability with the Civic.Social ecosystem.

The full Civic Hubs Pilot specification provides comprehensive detail on all sections referenced here and is available upon request.

---

## What We Are Building

Civic.Social is developing its own hub engine — the Civic.Social Hub Engine — as the primary implementation for the pilot. Early development is underway, with the project's founding team building core infrastructure using AI-assisted development tools. The pilot will use this engine for reference deployments with real communities, while also exploring interoperability with existing civic technology organizations.

The pilot will demonstrate a complete end-to-end civic hub experience:

1. A community launches a Civic Hub using the Civic.Social Hub Engine.
2. Citizens create accounts and join the hub (initially using conventional authentication, with a path to Civic Identity).
3. The hub hosts at least one structured civic process (e.g., an advisory vote or consultation).
4. Citizens participate and receive results.
5. The hub publishes Civic Events conforming to the Civic Event Specification.
6. A second hub discovers the first hub and reads its event feed, demonstrating basic federation.
7. When Civic Identity integration is available, citizens authenticate using decentralized identity (DIDs) and present Verifiable Credentials for participation.

This loop demonstrates the core value proposition: community-operated civic spaces that host real participation and interoperate through open standards.

---

## Architecture Overview

A Civic Hub is the primary organizational unit of the Civic.Social ecosystem — the community-operated space where groups gather, civic processes are hosted, and participation becomes visible. At the protocol level, a hub hosts Civic Processes, accepts user actions, emits and aggregates standardized Civic Events, and exposes a discovery manifest for federation.

The hub sits within an ecosystem of six core components: Identity (DIDs, Verifiable Credentials, authentication), Civic Hubs (community-operated spaces — this pilot), Civic Processes (modular civic participation tools), Civic Activity Feed (event aggregation and distribution), Discovery (hub and process indexing), and the Citizen Dashboard (personal civic interfaces).

Hubs integrate with Identity for authentication, host Civic Processes, publish events into the Activity Feed, and appear in Discovery. Importantly, hubs function as independent civic spaces even without the broader ecosystem — they can launch standalone and integrate incrementally.

The hub architecture follows an event-first, plugin-based, API-first design. Every action emits a standardized Civic Event — no silent state changes. Civic processes integrate through the Civic Process Plugin Framework, where each process type is a self-contained plugin registered with the hub. The hub exposes a REST API that powers both its own web interface and external clients such as citizen dashboards. Identity integration is modular and phased: hubs launch with conventional authentication (email, OAuth), add DID-based authentication when Civic Identity infrastructure is available, and support Verifiable Credential verification for gated processes. The identity adapter is designed as a replaceable module so upgrading never requires rebuilding the hub.

The full Civic Hubs Pilot specification details nine required hub components, API endpoints, event types, and integration contracts.

---

## Pilot Scope

The pilot will produce a Civic Hub Compliance Specification defining what it means to be a compliant hub in the ecosystem, reference hub deployments with 1–3 real communities using the Civic.Social Hub Engine as the primary implementation (with a potential second hub engine from a partner platform), at least one civic process demonstrating the full lifecycle, event emission conforming to the Civic Event Specification, identity integration (standalone authentication at minimum, with Civic Identity contingent on the Civic Identity Pilot), a minimum viable admin panel, a cross-hub discovery and event distribution proof of concept, a hub operator guide, and a hub engine evaluation report.

Explicitly out of scope: production-scale infrastructure, Personal Data Store implementation, advanced governance models (liquid democracy, delegative voting), real-time event delivery, and cross-hub process execution. ActivityPub federation is an open question — the Civic Event model is already designed with forward-compatible mapping to ActivityStreams, and basic ActivityPub implementation will be evaluated during Phase 1 for feasibility within the pilot timeline and budget.

The pilot produces working prototypes. Architectural decisions, API designs, and integration patterns are intended to serve as the foundation for production development.

---

## Pilot Phases and Timeline

**Estimated duration: 10–13 months**

**Phase 1 — Architecture Hardening and Pilot Preparation (2–3 months).** The hub architecture and a working reference implementation already exist. This phase focuses on hardening the existing architecture, drafting the Civic Hub Compliance Specification, adding persistent storage and real authentication, building the admin panel, and preparing for community deployment. Outreach to potential hub engine partners and technical collaborators is ongoing and may begin prior to Phase 1.

**Phase 2 — Hub Infrastructure Development (3–4 months).** Extend the hub engine for production readiness: moderation tools, community onboarding flows, discovery manifest, cross-hub event distribution, and identity adapter upgrades. Evaluate ActivityPub federation feasibility.

**Phase 3 — Community Deployment and Integration (3–4 months).** Deploy hubs with pilot communities. Host real civic processes with real participants. Integrate with Civic Identity when available. Demonstrate cross-hub interoperability.

**Phase 4 — Demonstration, Reporting, and Handoff (2 months).** Live demos for funders and ecosystem partners, pilot report, hub operator guide, and deliverable packaging. The pilot report will include a production roadmap — what infrastructure, funding, and partnerships are needed to move from pilot to production deployment.

---

## Estimated Budget

**Estimated total pilot cost: $350,000–$600,000**

Major cost categories include hub infrastructure development, hub engine adaptation, community deployment, civic process integration, identity integration, interoperability implementation, operator documentation, and pilot operations. Some cost categories will depend on the collaboration model and which technical partners join the pilot. The final budget allocation will be refined during partner engagement.

---

## Technical Collaboration

The Civic.Social project team leads ecosystem architecture, community partnerships, and pilot coordination. The founding team is actively building the Civic.Social Hub Engine and contributing to development across the pilot. We are looking for technical collaborators who can strengthen the pilot's execution in one or more of the following ways.

**Technical Leadership.** A senior technical collaborator who can guide architecture decisions, review implementation work, identify risks and bugs, and ensure the hub engine meets production-quality standards. This person would work closely with the founding team — not replacing the development effort, but sharpening it.

**Development Collaboration.** Engineers who can contribute directly to hub engine development, civic process integration, identity adapter implementation, or frontend development. The Civic.Social Hub Engine is built with Node.js, TypeScript, Express, and React.

**Platform Integration Partners.** Existing civic technology platforms — such as Roundabout (New Public), Bonfire Networks, or Decidim — may collaborate on interoperability with the Civic.Social ecosystem. The Civic Hub Compliance Specification defines what it means to be a compliant hub, and the collaboration model involves exploring how existing platforms can interoperate with or adapt to the specification.

Across all collaboration types, relevant experience includes community-facing web applications, event-driven architectures, ActivityPub or comparable federation protocols, decentralized identity (DIDs and Verifiable Credentials), data portability, and community deployment.

---

## About Civic.Social

Civic.Social is a project of the Mosaic Foundation, a Virginia non-stock corporation building open, federated infrastructure for civic participation. The project addresses fragmentation in the pro-democracy technology ecosystem by providing shared infrastructure that connects and amplifies existing tools, communities, and pro-democracy efforts rather than replacing them.

The project is led by Adam Lake, Founding Steward, who has spent over fifteen years studying the intersection of decentralized technology, civic infrastructure, and healthy online communities. Civic.Social grew out of that work — a long-term effort to design federated civic infrastructure that strengthens democratic participation rather than undermining it.

For inquiries: adam@civic.social · civic.social

---

*Civic.Social — civic.social | adam@civic.social*
*Civic Hubs Pilot — Technical Brief*
