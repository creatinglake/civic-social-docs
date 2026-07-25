---
status: draft
last-reviewed: 2026-07-03
owners: [adam]
version: 0.1
---

# Civic Identity Specification — Draft v0.1

## Lineage

This specification is promoted from the **Civic Identity Pilot** specification (`pilots/civic-identity/civic-identity-pilot-spec.md`), which remains the program and execution document for the identity pilot — its phases, deliverables, budget, partners, and validation criteria are defined there. This document carries the ecosystem-level normative content forward as one of the four canonical Civic.Social specifications (Civic Space · Civic Process · Civic Activity · Civic Identity). Where the two documents overlap, **this specification governs**.

Like the other canonical specifications, this document defines the *target state*. Reference implementations converge on it through the pilot program (see Section 12, Conformance).

---

# 1. Purpose & Scope

## 1.1 Purpose

Civic Identity is the portable, participant-owned key to the Civic.Social ecosystem — the identity thread that connects every Civic Space, Civic Process, and civic interface. Every holder — person, entity, or community — anchors to a sovereign identity node of the same shape (Section 5), and Civic Spaces themselves hold DIDs (Section 5.1). The specification elaborates the **citizen case** in the greatest depth, because individual identity carries the hardest requirements (personhood verification, privacy, provider custody): it enables a citizen to authenticate once and participate across independent civic environments, carrying their credentials and relationships with them, without any platform owning their identity. Entity and community identity reuse the same primitives with role and collective credentials respectively.

This specification defines:

- The **Citizen Node** — the fundamental identity primitive
- The **Personal Data Store (PDS)** — the user-controlled relationship and preference layer
- The three-layer separation between identity, personal data, and applications
- Sovereign identity nodes at every holder scope (person, entity, community) and Space DIDs
- Authentication protocols and the session model
- The credential model and initial credential registry
- The **Identity Policy Object** — how spaces and processes express identity requirements
- Requirements for the two identity **infrastructure roles**: Citizen Account Providers and Badge/Credential Issuers
- The stewardship model and identity portability guarantees

This specification does NOT:

- Mandate a specific wallet, provider, or verification vendor
- Define a proprietary identity system — it extends W3C Decentralized Identifiers (DIDs), W3C Verifiable Credentials (VCs), and the OpenID credential protocol family
- Define governance rules for individual spaces (each space sets its own identity policy within this framework)
- Solve the global proof-of-personhood problem (it defines the verification *interface*, provider-agnostically)

## 1.2 Design Principles

- **Self-sovereign identity.** Holders — persons, entities, and communities alike — control their own identifiers and credentials rather than relying on platform-controlled accounts.
- **Decentralization.** Identity MUST NOT depend on a central authority or database for verification.
- **Interoperability.** Credentials MUST work across civic spaces, civic processes, and external civic applications.
- **Privacy by design.** Citizens SHOULD disclose only the minimum information required to participate.
- **Portability.** Holders MUST be able to migrate their identity and credentials between providers and wallets.
- **Open standards.** The identity layer is built on widely adopted open standards, not proprietary identity solutions.

---

# 2. The Citizen Node

The **Citizen Node** is the fundamental identity primitive of the ecosystem — the minimum viable unit of participation. It is a protocol-level construct, not a product feature, user interface element, or platform-specific record.

## 2.1 Composition

A Citizen Node consists of:

- A **Decentralized Identifier (DID)** conforming to the W3C DID specification, associated with a cryptographic key pair the citizen controls
- One or more **Verifiable Credentials** conforming to the W3C Verifiable Credentials Data Model, held in an identity wallet — claims about the citizen (personhood, jurisdiction residency, organizational membership, civic roles) issued by trusted issuers and verifiable without contacting the issuer

## 2.2 Required Operations

A Citizen Node MUST support three fundamental operations:

1. **Authenticate.** The citizen MUST be able to prove control of their DID through cryptographic challenge-response to any space, process, or platform implementing this specification.
2. **Present credentials.** When a civic process requires eligibility verification, the Citizen Node MUST be able to surface the relevant credentials from the citizen's wallet as a verifiable presentation.
3. **Act across systems.** A single Citizen Node MUST be sufficient to participate in any compliant civic environment — a municipal hub, a state-level assembly, an organizational deliberation, or an external application integrated with the identity layer.

## 2.3 Explicit Exclusions

The Citizen Node deliberately excludes constructs that traditional platforms conflate with identity:

- **No profile.** Profiles are application-layer constructs. Applications MAY render profiles, differently or not at all; they are not part of the identity primitive.
- **No feed.** Feeds are constructed dynamically by applications from subscriptions and activity; they are not identity state.
- **No user interface layer.** How a citizen interacts with their identity — mobile wallet, browser extension, custodial service — is an implementation choice, not a property of the node.
- **No platform-owned state.** No platform may claim ownership of a citizen's identifier or credentials. These remain under the citizen's control at all times.

Because the Citizen Node carries only identity and credentials — not application state — it moves freely across the ecosystem without being tethered to any platform's data model.

---

# 3. The Personal Data Store (PDS)

The **Personal Data Store** is a user-controlled data layer that operates alongside the Citizen Node but is architecturally distinct from it. Where the Citizen Node handles identity (who the citizen is and what they can verifiably claim), the PDS handles the relationships, preferences, and references the citizen accumulates through participation — so that switching applications never means losing them.

## 3.1 Contents — Exactly Four Categories

The PDS stores four categories of user-owned data:

1. **Social graph** — follows, subscriptions, and connections to civic spaces, organizations, communities, and other citizens.
2. **Space memberships and subscriptions** — which civic spaces and communities the citizen has joined, with the subscription preferences governing how they receive updates.
3. **Participation pointers** — lightweight *references* to civic processes the citizen has engaged with (consultations contributed to, assemblies attended, votes cast). These are pointers, **not content**: the substance of a contribution remains with the civic process that hosted it.
4. **User preferences** — settings that govern the citizen's experience across applications: notifications, accessibility, language, display options.

## 3.2 Explicit Non-Contents

The PDS MUST NOT be treated as a store for:

- **Full posts or content contributions.** The content of participation belongs to the hosting civic process; the PDS holds at most a reference to the engagement.
- **Feeds.** Feeds are application-layer constructs assembled from subscriptions and activity; the PDS provides the subscription data feeds are built from, never the feed itself.
- **Platform-owned application data.** Applications may keep their own internal interaction data; that data belongs to the application, not the PDS.

This scoping preserves freedom of association (no platform can hold a citizen's network hostage) and prevents data-level lock-in (relational data travels with the citizen, not the application).

## 3.3 The Wallet / PDS Boundary

The identity wallet and the PDS are complementary but architecturally separate systems:

- The **wallet** is a credential management system: it stores verifiable credentials and performs the cryptographic operations of authentication and presentation.
- The **PDS** is a relationship and preference management system: it maintains the persistent state that defines the citizen's place in the ecosystem. It does not sign challenges or present credentials.

Where credentials are stored is a **deliberately unresolved implementation boundary**: credentials MAY live in the wallet, be referenced via the PDS, or both, depending on implementation architecture. The binding constraint is:

> Credentials and user state MUST both be portable and user-controlled, regardless of which component stores them.

A wallet without a PDS proves identity but loses relationships across application switches; a PDS without a wallet preserves relationships but cannot prove identity. Both are required for the full portability guarantee.

---

# 4. Three-Layer Separation: Identity / PDS / Application

The architecture enforces a strict separation of concerns:

| Layer | Contents | Responsibility | Ownership |
|---|---|---|---|
| **Identity Layer** | DID + Verifiable Credentials | Authentication and verification — *who is this person, and what can they verifiably claim?* | Citizen |
| **PDS Layer** | Social graph, memberships, participation pointers, preferences | Continuity — *what relationships and context has this person built?* | Citizen |
| **Application Layer** | Feeds, profiles, space interfaces, process UIs, dashboards | Experience — everything citizens see and interact with | Application operator |

Requirements and consequences:

- **Applications are replaceable.** Because identity and user data live in citizen-controlled layers, a citizen can switch interfaces without losing anything essential. Applications read the Identity Layer to authenticate and verify; they read the PDS to reconstruct subscriptions and connections; they write to the PDS when citizens take actions that should persist across platforms.
- **Feeds are not a primitive.** A feed is assembled by an application from PDS subscription data and space activity. Different applications MAY construct different feeds from the same PDS. No application has exclusive control over a citizen's feed.
- **Profiles are not required.** A citizen MUST be able to participate fully without ever creating a profile. Applications MAY offer profile experiences; participation capability MUST NOT depend on them.
- **No layer above may claim the layer below.** No application may treat identity or PDS data as proprietary or exclusive.

---

# 5. Sovereign Nodes at Every Holder Scope

The Citizen Node pattern generalizes across the Sovereign Foundation's holder taxonomy. Every participant kind anchors to a sovereign node of the same shape — a DID plus credentials plus a data store:

| Holder | Sovereign node | Data store |
|---|---|---|
| Person | **Citizen Node** (DID + VCs) | Personal Data Store |
| Entity (accountable public role) | **Entity Node** (DID + role credentials) | Entity Data Store |
| Community | **Community Node** (collective DID + credentials) | Community Data Store |

The holder set is open, matching the open scope taxonomy of the Civic Space Specification (§1.4): a new holder kind registers a node and data store of this shape without redesigning the identity layer.

## 5.1 Space DIDs

Civic Spaces themselves hold DIDs. Every Civic Space MUST hold a stable **space DID**, distinct from its serving URL, anchored to (or controlled by) the sovereign node of its scope. Requirements for space DIDs — URL binding via the DID document, activity source attribution, and migration survival — are defined in the **Civic Space Specification §3.5**.

## 5.2 DID Method Guidance

Implementations SHOULD start pragmatic and evolve as the ecosystem matures. Candidate methods:

- **`did:key`** — simple cryptographic identifiers; suitable for early phases and ephemeral or low-assurance contexts
- **`did:web`** — web-hosted DID documents; acceptable for holders expected to remain on a stable domain
- **`did:tdw`** (Trust DID Web) — an evolution of `did:web` with verifiable history and controller continuity

Holders that anticipate domain or provider migration — entities and spaces in particular — SHOULD prefer `did:tdw` or an equivalent method with verifiable history, so that identifier continuity survives migration.

---

# 6. Authentication

## 6.1 Protocols

The identity layer adopts the OpenID credential protocol family, named per protocol function:

- **OpenID4VCI** (OpenID for Verifiable Credential Issuance) — credential issuance
- **OpenID4VP** (OpenID for Verifiable Presentations) — credential presentation
- **SIOPv2** (Self-Issued OpenID Provider v2) — self-issued authentication
- **FIDO-compatible authenticators** — supported via the identity provider for user-facing authentication

Wallet-based authentication MUST be supported. These protocols allow identity wallets to interact with web platforms using familiar authentication flows while remaining fully decentralized.

## 6.2 Challenge-Response Session Model

Authentication rests on cryptographic challenge-response using the citizen's DID key:

1. Citizen attempts to access a space or process.
2. The platform requests authentication.
3. The citizen signs a challenge with their DID key (via wallet or identity provider).
4. The platform verifies control of the identifier.
5. The platform requests any credentials required for participation; the wallet presents them; the platform verifies signatures and issuer trust.

The full challenge-response flow typically occurs **once per session**. After authentication, session continuity mechanisms (comparable to standard web sessions) allow the citizen to move between spaces and processes without repeated authentication. A civic process accessed through an already-authenticated space MAY reuse the existing identity session rather than requiring a new challenge-response. This yields single-sign-on-like behavior across the ecosystem without a centralized identity provider.

## 6.3 Access Paths

Credential verification MUST work identically regardless of how the citizen encounters a process: directly at a space, through a space into a hosted process, via a direct link or embedded process on an external page, or from a civic activity feed or dashboard. A process encountered outside any space MUST still be able to authenticate the citizen and verify credentials directly through the identity layer.

---

# 7. Credential Model

## 7.1 Credential Format

Credentials MUST conform to the **W3C Verifiable Credentials Data Model**. Issuers MUST cryptographically sign credentials and publish public verification keys so credentials can be validated across the ecosystem without contacting the issuer.

## 7.2 Initial Credential Registry

The initial credential categories are:

- **Proof of personhood** — verifies that a DID corresponds to a unique human being. This is the anti-bot, anti-sybil floor: it reveals neither who the person is nor where they live, and serves as the lowest credential threshold separating "open to anyone" from "open to verified humans." The verification *interface* is provider-agnostic; the ecosystem supports multiple personhood verification paths (government identity verification, third-party providers, biometric uniqueness, existing personhood networks) without committing to any single method.
- **Jurisdiction / residency credentials** — verify residency within a jurisdiction (e.g., `resident.city`, `resident.county`, `resident.state`, voter district) without revealing unnecessary personal information. Possible issuers include government agencies, identity verification providers, trusted civic institutions, and civic spaces themselves.
- **Civic role credentials** — verify participation roles (assembly participant, facilitator, moderator, committee member), issued by spaces, process providers, or organizations.
- **Organizational membership credentials** — verify membership in civic organizations (nonprofits, unions, coalitions, civic networks).
- **Issue / policy credentials** — used by advocacy organizations to recognize participation or commitments (civic badges, policy pledge credentials). Display semantics for public badges are the domain of the Civic Credentialing pilot.

Different civic processes MAY trust different issuers, and participation rules MAY allow multiple credential pathways to satisfy a requirement.

## 7.3 Published Schemas

Credential schemas MUST be documented and published so that **independent issuers can issue compatible credentials**. Schema publication is a conformance requirement (Section 12), not a courtesy: interoperability of the credential layer depends on any qualifying organization being able to issue credentials that any compliant verifier can validate.

---

# 8. Identity Policy Object

Every space and process defines its own identity requirements. Identity policy is governed by three independent dimensions:

- **Assurance** — the level of identity verification required (none → email verification → proof of personhood → residency credential → role credential). *What is verified.*
- **Disclosure** — what information is revealed, and to whom (anonymous → pseudonymous → real identity, with selective disclosure of individual attributes). *What is revealed.*
- **Context** — how assurance and disclosure compose into a policy for a specific space or process, including inheritance (a space MAY define a base policy that its processes inherit and extend). *How the dimensions combine.*

These dimensions are deliberately independent: **high assurance does not require high disclosure**. A process may require verified residency credentials while still allowing anonymous or pseudonymous participation in its outputs.

## 8.1 Structure

Spaces and processes express identity requirements using a standard **Identity Policy Object**:

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

- **`assurance_requirements`** — credential types required for participation eligibility; citizens must hold at least one credential matching each entry
- **`optional_credentials`** — credential types that may affect participation tier or visibility without being required for basic access
- **`disclosure_policy.public`** — attributes visible to all participants and observers
- **`disclosure_policy.process`** — attributes visible only to process facilitators or operators
- **`disclosure_policy.private`** — attributes retained only by the participant's wallet and never transmitted to the platform
- **`participation_rules`** — maps each participation action (view, participate, vote, moderate) to a minimum assurance level or credential type; actions not listed inherit the process default
- **`result_stratification`** — an ordered list of tier labels defining how aggregate participation counts are reported; each label corresponds to a credential type or identity mode; tiers are reported as counts only and MUST NOT expose individual records

The Identity Policy Object MAY be versioned over time as governance models evolve.

## 8.2 Relationship to the Process Specification

The Civic Process Specification's `disclosure_policy` descriptor field references this object: a process declares `secret`, `on_the_record`, or a **named policy from the Identity Policy Object**. The policy MUST be declared before participation begins and published in the process descriptor, so participants know the disclosure terms before acting. The Civic Activity Specification §7 defines how disclosure policy constrains activity payloads.

## 8.3 Result Stratification

Result stratification allows processes to lower participation barriers while reporting results with distinctions by verification tier. Rather than excluding participants who lack higher-level credentials, a process MAY accept broader participation and report results stratified by credential tier:

```json
{
  "verified_residents": { "count": 320 },
  "verified_humans": { "count": 800 },
  "open": { "count": 120 }
}
```

Only aggregate counts per tier are included; individual participation records MUST NOT be disclosed through stratified outputs. Stratification is optional; processes that do not require it omit the `result_stratification` field.

---

# 9. Infrastructure Roles

Two ecosystem participants serve the identity foundation rather than hosting processes at a scope. They are **service providers governed by this specification, not Civic Spaces** (Civic Space Specification §1.5).

## 9.1 Citizen Account Provider

A Citizen Account Provider hosts a citizen's foundation — the Citizen Node and Personal Data Store — on the citizen's behalf. The model is federated, like an email provider for civic identity.

A conformant Citizen Account Provider MUST:

- Create and maintain DIDs and associated cryptographic keys for citizens
- Support DID-based challenge-response authentication for spaces, processes, and external platforms
- Support credential storage and presentation (wallet-based or managed flows)
- Provide **managed key custody by default**, with **self-custody available** as an option
- Provide **full export** of identity keys, credentials, and PDS contents in structured form
- Support **provider migration**: citizens MUST be able to move to another compliant provider without losing credentials or relationships
- Expose open, standards-based APIs (the protocols of Section 6)

A Citizen Account Provider MUST NOT bind a citizen's participation capability to its own continued operation: everything required to participate must be exportable.

## 9.2 Badge / Credential Issuer

A Badge/Credential Issuer is a third-party organization issuing verifiable credentials into the ecosystem (pledges, endorsements, attestations of office, participation records).

A conformant issuer MUST:

- Hold an **issuer DID** and publish public verification keys
- Issue credentials conforming to the published schemas (Section 7.3), or publish schemas for new credential types it introduces
- Publish its **issuance criteria** so verifiers and citizens can evaluate what a credential attests
- Support **revocation**, such that verifiers can check credential revocation status
- Be listed in a **trust registry** entry so that spaces and processes can decide whether to trust its credentials

Trust decisions remain local: each space or process decides which issuers it trusts for which credential types.

---

# 10. Stewardship: The Managed Sovereign Identity Model

This specification adopts a **Managed Sovereign Identity Model with Transitional Central Stewardship**.

The objective is to preserve individual identity sovereignty without requiring every user to self-custody cryptographic keys. Key custody MAY be managed by a trusted provider (managed wallet model); self-custody MUST remain an available option; users MUST be able to export their identity keys and credentials.

The ecosystem will begin with a single Civic Identity service implementation operated by a neutral, nonprofit steward (the "Civic Identity Steward"). This steward:

- Operates the reference Civic Identity service
- Maintains DID infrastructure and credential registries
- Maintains user-associated social graph metadata
- Provides managed key custody by default
- Exposes open, standards-based APIs

This initial central stewardship is intended to reduce complexity during early network formation, ensure coherent migration and interoperability guarantees, and build trust under nonprofit governance rather than venture-backed control.

However, the architecture MUST be designed from inception to allow:

- Export of identity keys and credentials
- Export of full social graph data
- Migration to an alternative compliant identity provider
- Emergence of interoperable competing providers

The long-term design objective is a standards-based ecosystem of interoperable providers, in which users migrate between providers without loss of credentials or relationships and no single operator can permanently centralize control. Over time, governance of the identity layer **may evolve** toward multi-stakeholder stewardship — for example, a consortium of civic organizations, public institutions, and ecosystem stakeholders. This evolution is a stated direction, not a scheduled commitment; the transitional model balances usability (managed custody), network coherence (a single initial provider), and long-term decentralization (provider portability).

---

# 11. Identity Portability

Portability is the load-bearing guarantee of the identity layer.

## 11.1 What Citizens Retain

Users MUST retain, across any application switch or provider migration:

- Their **DID**
- Their **credentials**
- Their **attestations**

Space engines MAY store role mappings locally but MUST NOT control identity issuance. Role and authority bindings SHOULD be keyed to the participant's identifier (DID or portable id), not to provider-specific attributes such as email addresses, so that provider migration does not strand the authorization layer.

## 11.2 Export

- **Wallet export:** citizens MUST be able to export and migrate credentials to other compatible wallets.
- **PDS export:** citizens MUST be able to export their full social graph and preferences in structured form.
- **Key material rule:** private keys live **only** in wallets and identity providers. Space exports carry identity *references* (DIDs only) — key material MUST NOT appear in any space export (Civic Space Specification, portability contract).

## 11.3 Provider Migration

Citizens MUST be able to migrate between Citizen Account Providers. Relationship data (follows, memberships, delegations, endorsements) is logically tied to the citizen's identity record — not owned by any space engine — so that if a space engine or application is replaced, the citizen's relationships remain intact and re-bind to the new instance.

## 11.4 Open Question: DID Stability Across Provider Migration

Whether a citizen's DID remains stable across provider migration, or is re-issued with a verifiable continuity link (as `did:tdw`-class methods support), is an **open question**. It must be answered before the reference identity service mints identifiers at scale, because the answer determines whether downstream systems can key long-lived bindings (roles, delegations, participation history) to the DID itself. Until resolved, implementations SHOULD avoid designs that assume either answer irreversibly.

---

# 12. Conformance

## 12.1 Inherited Conformance

This specification builds on standards with existing conformance machinery. Implementations MUST conform to:

- W3C Decentralized Identifiers (DIDs) — DID syntax, resolution, and DID document requirements
- W3C Verifiable Credentials Data Model — credential structure, proofs, and presentation
- OpenID4VCI / OpenID4VP / SIOPv2 — issuance, presentation, and self-issued authentication flows

Conformance test suites for these underlying standards apply directly; this specification adds civic-layer requirements on top rather than redefining the base layers.

## 12.2 Civic-Layer Requirements

A conformant implementation additionally:

- Publishes credential schemas for every credential type it issues (Section 7.3)
- Supports the Identity Policy Object for expressing participation requirements (Section 8)
- Satisfies the portability guarantees of Section 11 appropriate to its role (provider, issuer, or space-side verifier)
- For Citizen Account Providers and issuers: meets the role requirements of Section 9

## 12.3 Conformance Phasing

The reference implementation (`civic-hub`) currently uses **stub identity at a declared assurance level** through a replaceable identity adapter seam, as permitted by the Civic Space Specification §3.1 during early phases: opaque user identifiers, with the space declaring its assurance level in its identity policy. Upgrading from stub identity to full Civic Identity must not require rebuilding the space.

Convergence from stub identity to the target state defined here is the work of the **Civic Identity pilot**. Specifications in this ecosystem define the target state; reference implementations converge on them through the pilot program.

---

# 13. Open Questions

Beyond the DID-stability question (§11.4), the following remain open design areas, carried forward from the pilot specification (which discusses each in more depth):

- **Custodial optionality.** Should the ecosystem offer custodial identity services for non-technical users, and how is it ensured that custody remains genuinely optional — with export always available — rather than becoming a de facto default that recentralizes the layer?
- **Pseudonymous authority.** Should pseudonymous handles be scoped to a single process, a single space, or portable across the ecosystem? Process-scoped handles maximize privacy but prevent reputation and continuity; ecosystem-scoped handles enable richer participation histories but create persistent identifiers that could be profiled.
- **Credential-vs-capability semantics.** Credentials attest *attributes* (who you are, what you're a member of); capabilities grant *authority* (what you may do, to which resource). Where the boundary lies — and when authority should move from role/credential checks to signed, delegable, attenuable capability grants — is the subject of the **Authorization Model Note** (`ecosystem/authorization-model-note.md`), which defines the ecosystem's role-based near-term and capability-based long-term direction.

Additional open questions tracked in the pilot specification include jurisdiction credential issuance and trust, wallet strategy (build vs. adopt existing open-source wallets), selective disclosure depth, trust registry governance, identity policy context inheritance, minimum identity requirements for open modes, PDS scope and consent governance, and cross-ecosystem social graph interoperability.

---

## References

Normative references are those of the pilot specification: W3C DIDs v1.1, W3C Verifiable Credentials Data Model v2.0, JSON-LD 1.1, OpenID4VP, SIOPv2, OpenID4VCI, FIDO Alliance authentication standards, and ActivityPub. See the reference list in `pilots/civic-identity/civic-identity-pilot-spec.md`.

---

**Version:** 0.1
**Status:** Draft
**Last updated:** July 3, 2026
**Contact:** contact@civic.social
