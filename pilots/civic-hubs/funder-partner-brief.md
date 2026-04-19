---
status: review
last-reviewed: 2026-04-19
owners: [adam]
version: 0.1
---

# Civic Hubs Pilot — Funder & Partner Brief

Civic.Social Infrastructure Program

---

## The Problem

Civic participation today happens on infrastructure that communities do not control. Neighborhood groups organize on Facebook. City councils post agendas on government portals that no one visits. Civic organizations send email newsletters into the void. Citizens who want to engage with their community must navigate a dozen disconnected platforms, none of which are designed for democratic participation and none of which the community governs.

This fragmentation has real consequences. Citizens miss participation opportunities because there is no unified civic space where they can see what's happening. Communities cannot build sustained civic engagement because participation is scattered across ephemeral channels. Civic organizations duplicate infrastructure — each building its own tools, its own accounts, its own notification systems. And when civic life depends on commercial social media platforms, the incentives are misaligned: these platforms optimize for engagement and advertising revenue, not for constructive civic discourse or democratic outcomes.

The core insight is simple: civic spaces should be governed by communities, not platforms. Yet there is no open infrastructure that enables communities to operate their own digital civic spaces while remaining connected to a broader ecosystem of civic tools and processes.

---

## The Opportunity

Civic Hubs are community-operated digital civic spaces — like community centers for the digital age, but designed specifically for democratic participation. Each hub is independently governed by the community it serves: a municipality, a neighborhood, a civic organization, an issue network. Hubs host structured civic processes (advisory votes, citizen assemblies, participatory budgeting), publish civic information, and enable community interaction — all within an environment the community controls.

What makes this more than another civic technology tool is interoperability. Civic Hubs are built on open standards that allow independent hubs to discover each other, share civic activity, and enable citizens to participate across multiple communities with a single civic identity. A citizen can join their city hub, their neighborhood hub, and a housing coalition hub — and see activity from all three in a unified civic feed. Each hub remains autonomous, but the ecosystem generates network effects as more communities join.

The Civic Hubs Pilot will design, deploy, and evaluate reference hub implementations with real communities, proving the model works in practice and producing the architecture, documentation, and operational learnings needed to scale.

---

## What the Pilot Will Demonstrate

The pilot will deploy working Civic Hubs with 1–3 real communities and demonstrate:

- Communities can operate their own digital civic spaces using open hub software
- Civic processes (advisory voting, consultations) can run within hub environments with real citizen participation
- Structured participation formats produce actionable civic signals — not just conversation, but community positions, budget priorities, and policy recommendations that local officials can act on
- Multiple independent hubs can discover each other and share civic activity, proving the federation model works
- When Civic Identity integration is available, citizens can authenticate using decentralized identity, enabling participation across hubs without creating new accounts

This end-to-end demonstration will produce working hub deployments, a Civic Hub Compliance Specification defining what it means to be a compliant hub, a hub operator guide, and a pilot report with findings, recommendations, and a production roadmap.

The full list of deliverables and success criteria is detailed in the full Civic Hubs Pilot specification, available upon request.

---

## The Civic Hub

At the architectural level, a Civic Hub is the primary organizational unit of the Civic.Social ecosystem. It hosts civic processes, accepts citizen actions, emits standardized Civic Events for distribution across the ecosystem, and exposes a discovery manifest that allows other systems to find and interact with it.

At the community level, a Civic Hub is a shared digital civic space — a place where residents can follow what's happening, participate in decisions, and engage with their civic community. Different communities configure their hubs differently: a municipal hub hosts consultations and advisory votes, a neighborhood hub coordinates local projects and discussions, a civic organization hub supports policy deliberation and campaign coordination.

The hub model is designed for diversity. Anyone can create a hub. Communities govern their own civic spaces. No central authority determines who can participate or what topics are discussed. Some hubs may carry verification credentials (indicating official municipal status, for example), but hub creation remains open.

---

## Design Principles

**Community governance.** Each hub is governed by the community it serves. Moderation policies, participation rules, and feature configuration are determined locally.

**Open standards.** Hubs interoperate through shared standards for identity, events, processes, and discovery. No vendor lock-in — communities can migrate between hub software implementations without losing their governance history or social relationships.

**Structured civic participation.** Hubs prioritize structured democratic processes over open-ended social media dynamics. Advisory votes, citizen assemblies, and participatory budgeting produce actionable civic outputs.

**No engagement-maximizing algorithms.** Civic Hubs avoid the design patterns that make commercial social media toxic for civic life. Information is organized around civic relevance, not engagement metrics.

**Phased ecosystem integration.** Hubs can launch as standalone civic spaces and later integrate with Civic Identity, Civic Processes, and the Civic Activity Feed as that infrastructure becomes available.

---

## Theory of Change

The pilot produces working reference infrastructure and validated architecture. This establishes the operational foundation for a network of community-operated civic spaces connected through open standards.

The sequence is: this pilot validates the hub architecture and produces the Civic Hub Compliance Specification and hub operator guide → communities adopt hub software using these artifacts and reference implementations → hubs integrate with Civic Identity and Civic Processes as those pilots mature → the ecosystem grows as additional communities launch hubs and new hub engine implementations emerge → civic participation becomes continuous and connected rather than fragmented and episodic.

The pilot is designed to demonstrate feasibility with real communities, reduce adoption risk for future hub operators, and produce the artifacts needed to move toward production infrastructure.

---

## Timeline and Budget

**Estimated duration: 10–13 months**

**Phase 1 — Architecture Hardening and Pilot Preparation (2–3 months).** The hub architecture and a working reference implementation already exist. This phase focuses on hardening the existing architecture, drafting the Civic Hub Compliance Specification, adding persistent storage and real authentication, building the admin panel, and preparing for community deployment.

**Phase 2 — Hub Infrastructure Development (3–4 months).** Extend the hub engine for production readiness: moderation tools, community onboarding flows, discovery manifest, cross-hub event distribution, and identity adapter upgrades.

**Phase 3 — Community Deployment and Integration (3–4 months).** Deploy hubs with pilot communities. Host real civic processes with real participants. Integrate with Civic Identity when available. Demonstrate cross-hub interoperability.

**Phase 4 — Demonstration, Reporting, and Handoff (2 months).** Live demos for funders and ecosystem partners, pilot report, hub operator guide, and deliverable packaging. The pilot report will include a production roadmap — what infrastructure, funding, and partnerships are needed to move from pilot to production deployment.

Budget details are included in the funding ask below.

---

## Existing Momentum

The Civic Hubs Pilot builds on active relationships and infrastructure already in development:

**Civic.Social Hub Engine.** Civic.Social is developing its own hub engine — the Civic.Social Hub Engine — as the primary implementation for the pilot. Early development is underway, with the founding team building core infrastructure including process hosting, event emission, API endpoints, and a community interface. The pilot will use this engine for reference deployments with real communities.

**Hub engine and integration partners.** Civic.Social is in conversation with civic technology organizations — including Bonfire Networks, Decidim, and Roundabout (New Public) — as potential integration partners. The Civic Hub Compliance Specification will define what it means to be a compliant hub, opening the door for existing platforms to interoperate with the ecosystem.

**Broader Civic.Social program.** The hub pilot is one component of a broader infrastructure program that also includes pilots for Civic Identity, Civic Processes, the Civic Activity Feed, Civic Credentialing, and the Citizen Dashboard. The hub pilot provides the execution layer where citizens actually participate — making it the proving ground for the entire ecosystem. Joint funding of the hub and identity pilots accelerates both and demonstrates the end-to-end civic participation experience.

---

## What Comes Next

The pilot is designed to produce artifacts that support a clear transition to production. Phase 4 deliverables include a live demonstration environment, a pilot report with operational findings and a production roadmap, a hub operator guide, and the Civic Hub Compliance Specification.

After the pilot, the path forward includes hardening reference hub software for production use, onboarding additional communities as hub operators, expanding the hub engine ecosystem (additional platforms implementing the hub specification), integrating advanced civic processes through the plugin framework, and enabling full ActivityPub federation for real-time cross-hub communication. The open-standards foundation ensures that the infrastructure built during this pilot remains viable and extensible regardless of which organizations operate it long-term.

The pilot is designed with known risks in mind — including community adoption barriers, hub engine maturity, identity integration timing, and moderation challenges — with specific mitigations for each. These are detailed in the full Civic Hubs Pilot specification.

---

## The Funding Ask

Civic.Social is seeking $350,000–$600,000 to fund the Civic Hubs Pilot over 10–13 months. The budget range reflects ongoing conversations with hub engine partners and pilot communities to finalize scope. This investment will produce working civic hub infrastructure, validated architecture, operator documentation, and the community deployments needed to demonstrate that community-operated civic spaces are operationally viable and citizen-usable.

---

## About Civic.Social

Civic.Social is a project of the Mosaic Foundation, a Virginia non-stock corporation building open, federated infrastructure for civic participation. The project addresses fragmentation in the pro-democracy technology ecosystem by providing shared infrastructure that connects and amplifies existing tools, communities, and pro-democracy efforts rather than replacing them.

The project is led by Adam Lake, Founding Steward, who has spent over fifteen years studying the intersection of decentralized technology, civic infrastructure, and healthy online communities. Civic.Social grew out of that work — a long-term effort to design federated civic infrastructure that strengthens democratic participation rather than undermining it.

For inquiries: adam@civic.social · civic.social

---

## For Ecosystem Partners

Civic technology platforms, civic organizations, hub operators, and community groups can participate in the pilot in several ways:

**As a pilot community** — operate a Civic Hub for your community using the Civic.Social Hub Engine and contribute operational learnings to the hub operator guide.

**As a hub engine partner** — adapt your platform to implement the Civic Hub Compliance Specification, ensuring your community software can interoperate with the Civic.Social ecosystem.

**As a civic process provider** — integrate your civic participation tools (deliberation, voting, budgeting) into the hub environment through the Civic Process Plugin Framework.

**As a standards body or consultant** — the pilot builds on open standards for identity, events, and federation. Standards practitioners with a vested interest in seeing this work implemented in civic infrastructure are welcome to consult on architecture and specification design.

The full Civic Hubs Pilot specification is available upon request.

---

*Civic.Social — civic.social | adam@civic.social*
*Civic Hubs Pilot — Funder & Partner Brief*
