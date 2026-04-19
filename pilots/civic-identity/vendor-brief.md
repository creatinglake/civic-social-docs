---
status: review
last-reviewed: 2026-04-19
owners: [adam]
version: 0.1
---

# Civic Identity Pilot — Vendor Brief

Civic.Social Infrastructure Program

---

## Purpose

This brief describes the technical scope and architecture of the Civic Identity Pilot for prospective identity infrastructure vendors and technical implementation partners. It summarizes the key architectural decisions, the minimum viable identity stack, what the pilot will and will not build, and the expected collaboration model.

The full Civic Identity Pilot specification provides comprehensive detail on all sections referenced here and is available upon request.

---

## What We Are Building

The Civic Identity Pilot will design and prototype decentralized identity infrastructure that enables trusted participation across a federated ecosystem of civic hubs, civic processes, and citizen interfaces.

The pilot will demonstrate a complete end-to-end identity lifecycle:

1. A citizen creates a decentralized civic identity (DID + key pair) through a prototype Civic Identity Service.
2. The citizen receives verifiable credentials such as jurisdiction residency and potentially proof of personhood.
3. The citizen authenticates to a civic hub using their decentralized identity.
4. The civic hub verifies credentials and grants access to a civic process.
5. The citizen participates in the civic process.
6. The citizen accesses a second civic environment using the same identity — without creating a new account or repeating verification.

This loop demonstrates the core value proposition: portable, credential-verified civic participation across independent platforms.

---

## Identity Standards

The architecture is built on established W3C and OpenID standards:

- **W3C Decentralized Identifiers (DIDs)** — the base identity primitive. Each citizen controls a DID associated with a cryptographic key pair. DID methods under consideration include did:key, did:web, and did:tdw (Trust DID Web).
- **W3C Verifiable Credentials (VCs)** — portable, cryptographically verifiable claims about a citizen (personhood, residency, membership, civic roles).
- **OpenID for Verifiable Presentations (OpenID4VP)** — credential presentation protocol enabling wallets to interact with platforms using familiar authentication flows.
- **Self-Issued OpenID Provider v2 (SIOPv2)** — self-issued identity authentication.
- **OpenID for Verifiable Credential Issuance (OpenID4VCI)** — credential issuance protocol.

The pilot will begin with the most pragmatic DID method and protocol choices and evolve as the ecosystem matures.

---

## Core Identity Model

The identity model separates into three layers:

**Layer 1 — Decentralized Identifier.** Each citizen controls a DID conforming to the W3C DID specification, associated with a cryptographic key pair. The citizen proves control through cryptographic authentication.

**Layer 2 — Identity Credentials.** Organizations issue verifiable credentials associated with the citizen's DID. Initial credential types include jurisdiction residency, civic role credentials, organization membership, and possibly proof of personhood.

**Layer 3 — Credential Verification.** Each civic hub, civic process, or interface defines its own credential requirements for participation. Different combinations of credentials may satisfy different participation thresholds.

---

## The Citizen Node

The foundational identity primitive is the Citizen Node — a DID together with one or more Verifiable Credentials held in an identity wallet. The Citizen Node is defined at the protocol level, not the product level, ensuring portability across any compliant implementation.

A Citizen Node must support three operations: authenticate (prove control of the DID through challenge-response), present credentials (surface relevant credentials when a civic process requires verification), and act across systems (a single Citizen Node is sufficient for participation across any compliant civic environment).

The Citizen Node explicitly excludes profiles, feeds, and user interface elements — these are application-layer constructs, not identity primitives.

---

## Minimum Viable Civic Identity Stack

The pilot requires nine core components:

**1. DID Method.** W3C Decentralized Identifiers as the base identity primitive. Authentication without centralized accounts, cryptographic proof of identifier ownership, portable identity across platforms.

**2. Verifiable Credential Format.** W3C Verifiable Credentials Data Model. Credential schemas for proof of personhood, jurisdiction residency, organization membership, and civic roles. Schemas will be documented and published for interoperability.

**3. Credential Issuance Infrastructure.** Credential creation, cryptographic signing by the issuer, and delivery to the citizen's wallet. Issuers must publish public verification keys.

**4. Credential Verification Service.** An SDK or library enabling civic hubs and processes to request required credentials, verify issuer signatures, verify credential integrity, and validate expiration and revocation status.

**5. Authentication Flow.** Cryptographic challenge-response using the citizen's DID key, with session continuity mechanisms enabling single-sign-on-like behavior across the ecosystem.

**6. Identity Wallet.** One or more wallet models: browser-based, mobile, or custodial. Citizens must retain the ability to export and migrate credentials regardless of model.

**7. Credential Presentation Protocol.** Presentation of credentials using OpenID4VP and/or SIOPv2, enabling wallets to interact with civic platforms using familiar authentication flows.

**8. Trust Registry.** A registry of trusted credential issuers and credential schemas. Initially curated by the Civic.Social consortium, with a path toward decentralized governance.

**9. Developer Integration Tools.** Credential verification libraries, authentication APIs, developer documentation, and example integrations.

---

## Civic Identity Service Model

The pilot will produce a reference Civic Identity Service — a public-interest identity utility that provides:

- creation of decentralized identifiers (DIDs) for citizens
- authentication services for civic hubs and civic processes
- issuance of core civic credentials (personhood, jurisdiction, membership)
- wallet infrastructure for storing and presenting credentials
- credential verification services for participating platforms

This service is designed as a first implementation of an open standard. The ecosystem is intentionally designed to support multiple identity providers — any provider that supports the relevant decentralized identity standards can interoperate. The reference service exists to ensure identity infrastructure is available from the outset while maintaining a path toward a multi-provider ecosystem.

---

## Pilot Scope

### In Scope

- A minimum viable Civic Identity Service capable of creating DIDs, issuing credentials, and supporting authentication
- Credential schemas for jurisdiction residency and potentially proof of personhood
- A credential verification SDK or library for civic hubs and civic processes
- Authentication integration with at least one civic hub platform and one civic process
- A basic trust registry of recognized credential issuers
- Developer documentation enabling third-party integration
- A cost and operational analysis for building and running a civic identity service

### Explicitly Out of Scope

- Production-scale identity infrastructure (the pilot produces a working prototype, not a production system)
- Multiple identity provider interoperability (the pilot builds the reference service; multi-provider support is a future phase)
- Full self-sovereign key custody for all users (managed custody with export capability; full self-custody is supported architecturally but not required for pilot participants)
- Personal Data Store implementation (defined architecturally but delivered in a subsequent phase)
- Integration with more than two civic platforms

### Prototype vs. Production

The pilot produces a working prototype that demonstrates the viability of decentralized civic identity. Architectural decisions, credential schemas, and integration patterns are intended to serve as the foundation for production development. The goal is to prove the concept, validate the architecture, and produce clear guidance for a production system.

---

## Pilot Phases and Timeline

**Estimated duration: 9–12 months**

**Phase 1 — Architecture and System Design (2–3 months).** Finalize identity architecture, credential schemas, authentication flows, and integration models. This is the primary design collaboration phase between the project team and the vendor.

**Phase 2 — Identity Infrastructure Development (3–4 months).** Develop decentralized identity services, credential issuance systems, and verification tools.

**Phase 3 — Ecosystem Integration (2–3 months).** Integrate civic identity with pilot civic hubs and civic processes. Test end-to-end participation flows.

**Phase 4 — Demonstration, Reporting, and Handoff (2 months).** Produce live demos that stakeholders can test firsthand. Write pilot report. Package deliverables for the next phase.

---

## Estimated Budget

**Estimated total pilot cost: $400,000–$700,000**

The budget range reflects ongoing scope discussions. Major cost categories include identity infrastructure development, credential verification tools, civic hub integration, security and cryptographic engineering, developer documentation and SDKs, and pilot deployments and testing.

Some cost categories — particularly credential verification tools, SDKs, and developer documentation — may overlap with capabilities already provided by the vendor. The final budget allocation will be refined during vendor engagement.

---

## What We Expect from a Vendor Partner

The identity infrastructure vendor should bring deep expertise in W3C decentralized identity standards and the broader verifiable credential ecosystem. Specific expectations include:

- Experience implementing DID methods, verifiable credential issuance, and credential verification
- Familiarity with OpenID4VP, SIOPv2, and OpenID4VCI protocols
- Existing libraries, SDKs, or frameworks that can accelerate pilot development
- Willingness to collaborate on credential schema design tailored to civic use cases
- Ability to support both custodial and self-custody wallet models
- Understanding of trust registry architecture and credential governance

The vendor is expected to work collaboratively with the Civic.Social project team, which will lead on ecosystem architecture, civic process integration, and pilot coordination.

---

## About Civic.Social

Civic.Social is a project of the Mosaic Foundation, a Virginia non-stock corporation building open, federated infrastructure for civic participation. The project addresses fragmentation in the pro-democracy technology ecosystem by providing shared infrastructure that connects and amplifies existing civic tools rather than replacing them.

The project is led by Adam Lake, Founding Steward, who brings six years of experience in the self-sovereign identity space and fifteen years tracking decentralized social networking.

For inquiries: adam@civic.social · civic.social

---

*Civic.Social — civic.social | adam@civic.social*
*Civic Identity Pilot — Vendor Brief*
