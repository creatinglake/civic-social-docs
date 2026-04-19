---
status: review
last-reviewed: 2026-04-19
owners: [adam]
version: 0.1
---

# Civic Identity Pilot — Funder & Partner Brief

Civic.Social Infrastructure Program

---

## The Problem

The pro-democracy ecosystem is fragmented. Hundreds of civic technology organizations, civic participation platforms, and democratic innovation projects operate independently — each building its own infrastructure, its own user accounts, and its own identity systems. Citizens who want to participate must create new accounts on every platform. Organizations that want to verify participation eligibility must build or buy their own solutions. And because these systems don't interoperate, the ecosystem as a whole cannot generate the network effects that would make civic participation easier, more trustworthy, and more powerful over time.

This fragmentation has real consequences. Online civic processes struggle to distinguish real participants from automated agents — as demonstrated by incidents such as the mass fabrication of public comments during the FCC's 2017 net neutrality proceeding. Without verified identity, the outputs of civic processes — advisory votes, public consultations, deliberative results — lack the credibility needed for institutions and elected officials to act on them. And civic organizations duplicate effort building identity infrastructure that could be shared.

There is no shared identity layer for civic participation. Without one, the pro-democracy movement remains a collection of isolated tools rather than a connected ecosystem.

---

## The Opportunity

A shared civic identity layer would do for democratic participation what payment networks did for commerce and email protocols did for communication: create a common foundation that allows independent systems to interoperate and generate compounding value as the network grows.

When a citizen can authenticate once and participate across multiple civic platforms — and when a credential earned on one platform is recognized by every other — each new civic tool that joins the ecosystem makes every existing tool more valuable. Citizens gain a portable civic identity that works everywhere. Organizations gain verified participation without rebuilding infrastructure. And the ecosystem as a whole begins to function as something approaching a civic operating system: a shared layer of identity, trust, and interoperability that supports democratic participation at scale.

The Civic Identity Pilot will design and demonstrate the foundational piece of this infrastructure — decentralized identity built on established web standards (W3C Decentralized Identifiers and Verifiable Credentials) that are already being adopted by governments, major technology companies, and global standards organizations, including the EU Digital Identity Wallet initiative required for all EU member states by end of 2026.

This is not a proprietary platform. It is open, interoperable digital public infrastructure that any civic technology organization can adopt, and that becomes more powerful with every organization that does.

---

## What the Pilot Will Demonstrate

The pilot will demonstrate a complete civic participation flow in which a citizen:

- creates a civic identity (a Decentralized Identifier controlled by the citizen)
- receives verifiable credentials such as proof of personhood and jurisdiction residency
- authenticates to a civic hub, an online community space where citizens and civic organizations host and participate in democratic processes
- participates in civic processes across multiple platforms without creating new accounts

This end-to-end flow will be demonstrated across at least two independent civic environments, producing a working prototype, credential schemas, a verification SDK, developer documentation, a live demonstration environment, and a pilot report with findings and recommendations for production.

The full list of deliverables and success criteria is detailed in the full Civic Identity Pilot specification, available upon request.

---

## The Citizen Node

At the center of the architecture is the Citizen Node — the minimum viable unit of participation in the Civic.Social network. A Citizen Node is a Decentralized Identifier (DID) together with Verifiable Credentials held in an identity wallet.

It is not an app, not a profile, and not a platform account. It is a protocol-level primitive — defined by open standards, controlled by the citizen, and portable across any compliant civic environment. A citizen who creates their identity through the Civic.Social reference service can later migrate to an independent provider without losing their credentials or their ability to participate.

This design ensures no vendor lock-in. Identity travels with the citizen, not with any single application.

---

## Design Principles

**Open standards.** Built on W3C Decentralized Identifiers and Verifiable Credentials — widely adopted standards with growing global implementation.

**User control.** Citizens own their identifiers and credentials and can export or migrate to other providers at any time.

**Privacy by design.** Selective disclosure allows citizens to prove attributes like residency without revealing personal information.

**Low-friction onboarding.** A hybrid model combines simple custodial onboarding with full decentralized identity capabilities, so participation doesn't require technical sophistication.

**Multiple providers.** The reference Civic Identity Service is a first implementation of an open standard — not a centralized authority. Other providers can offer compatible services as the ecosystem grows.

---

## Theory of Change

The pilot produces working reference infrastructure and validated architecture. This establishes the technical foundation for a networked civic ecosystem where citizens participate across jurisdictions, organizations, and platforms using a single portable civic identity.

The sequence is: this pilot validates the identity architecture and produces integration tools built on open standards → civic technology platforms integrate using the SDK and documentation → citizens gain portable civic identity across participating platforms → the ecosystem grows as additional identity providers, credential issuers, and civic organizations adopt the open standard.

The pilot is designed to demonstrate feasibility, reduce integration risk for early adopters, and produce the artifacts needed to move toward production infrastructure.

---

## Timeline and Budget

**Estimated duration: 9–12 months**

**Phase 1 — Architecture and System Design (2–3 months).** Finalize identity architecture, credential schemas, and integration models.

**Phase 2 — Identity Infrastructure Development (3–4 months).** Develop decentralized identity services, credential issuance, and verification tools.

**Phase 3 — Ecosystem Integration (2–3 months).** Integrate civic identity with pilot hubs and civic processes.

**Phase 4 — Demonstration, Reporting, and Handoff (2 months).** Live demos that stakeholders and prospective partners can test firsthand, pilot report, and deliverable packaging for the next phase of development.

Budget details are included in the funding ask below.

---

## Existing Momentum

The Civic Identity Pilot builds on active relationships and infrastructure already in development:

**Identity infrastructure.** Civic.Social is in conversation with Digital Bazaar, a leading implementer of W3C decentralized identity standards, as a potential identity infrastructure partner for the pilot.

**Civic technology and pro-democracy partners.** Civic.Social has partners lined up in the civic technology and pro-democracy space who are positioned to participate as civic hub operators, civic process providers, and early integration partners.

**Broader Civic.Social program.** The identity pilot is one component of a broader infrastructure program that also includes pilots for civic hubs, civic process plugins, civic activity feeds, and civic credentialing. The identity pilot enables trusted participation across all of these. Joint funding of the identity and credentialing pilots accelerates both and reduces integration risk.

---

## What Comes Next

The pilot is designed to produce artifacts that support a clear transition to production. Phase 4 deliverables include a live demonstration environment, a pilot report with feasibility findings and production recommendations, and packaged architecture specs, schemas, and developer documentation.

After the pilot, the path forward includes hardening the reference identity service for production use, onboarding additional civic hub operators and civic technology platforms, expanding the credential issuer ecosystem, and transitioning governance of the trust registry toward a multi-stakeholder model. The open-standards foundation ensures that the infrastructure built during this pilot remains viable and extensible regardless of which organizations operate it long-term.

The pilot is designed with known risks in mind — including adoption barriers, identity usability, credential fragmentation, and technical complexity — with specific mitigations for each. These are detailed in the full Civic Identity Pilot specification.

---

## The Funding Ask

Civic.Social is seeking $400,000–$700,000 to fund the Civic Identity Pilot over 9–12 months. The budget range reflects ongoing conversations with identity infrastructure vendors and pilot partners to finalize scope. This investment will produce working identity infrastructure built on open standards, validated architecture, and the integration tools needed to launch a shared civic identity layer for the pro-democracy ecosystem.

---

## About Civic.Social

Civic.Social is a project of the Mosaic Foundation, a Virginia non-stock corporation building open, federated infrastructure for civic participation. The project addresses fragmentation in the pro-democracy technology ecosystem by providing shared infrastructure — civic identity, civic hubs, civic process plugins, and a civic activity feed — that connects and amplifies existing civic tools rather than replacing them.

The project is led by Adam Lake, Founding Steward, who brings six years of experience in the self-sovereign identity space and fifteen years tracking decentralized social networking. Civic.Social operates as a convener and infrastructure provider, building the connective layer that allows independent civic tools, governments, and organizations to interoperate.

For inquiries: adam@civic.social · civic.social

---

## For Ecosystem Partners

Civic technology platforms, civic organizations, hub operators, and standards bodies can participate in the pilot in several ways:

**As a civic hub partner** — integrate the identity layer to ensure access is restricted to verified citizens based on requirements set by the hub.

**As a civic process provider** — use the credential verification SDK to accept civic credentials from citizens participating in civic processes across the ecosystem.

**As a standards body or consultant** — the pilot builds on foundational work developed by decentralized identity standards organizations. Standards bodies and practitioners with a vested interest in seeing this work implemented in real-world civic infrastructure are welcome to consult on architecture and credential design.

The full Civic Identity Pilot specification is available upon request.

---

*Civic.Social — civic.social | adam@civic.social*
*Civic Identity Pilot — Funder & Partner Brief*
