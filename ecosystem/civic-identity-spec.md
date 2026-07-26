---
status: draft
last-reviewed: 2026-07-26
owners: [adam]
version: 0.1
---

# Civic Identity Specification — Draft v0.1

## Lineage

This specification is promoted from the **Civic Identity Pilot** specification (`pilots/civic-identity/civic-identity-pilot-spec.md`), which remains the program and execution document for the identity pilot — its phases, deliverables, budget, partners, and validation criteria are defined there. This document carries the ecosystem-level normative content forward as one of the four canonical Civic.Social specifications (Civic Space · Civic Process · Civic Activity · Civic Identity). Where the two documents overlap, **this specification governs**.

Conformance — what a conformant implementation is at each role, and how the reference implementation phases toward it — is defined in Section 12.

---

# Notation and Conformance Language

**Notation.** The key words MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, SHOULD NOT, RECOMMENDED, MAY, and OPTIONAL in this document are to be interpreted as described in RFC 2119 and RFC 8174 when, and only when, they appear in all capitals. The same words in lower case carry their ordinary English meaning and impose no requirement.

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
- **Privacy by design.** Holders SHOULD disclose only the minimum information required to participate. Minimum disclosure is about *attributes*, not about being uncorrelatable — see Section 7.4 for what v0.1 does and does not provide.
- **Portability.** Holders MUST be able to migrate their identity and credentials between providers and wallets.
- **Open standards.** The identity layer is built on widely adopted open standards, not proprietary identity solutions.

## 1.3 Terminology

Terms used throughout this document. The canonical definitions for the wider ecosystem live in the Civic.Social Terminology Glossary; these are the identity-layer senses.

- **Sovereign Foundation** — the participant-owned identity-and-data layer beneath every interface: a sovereign node (DID + credentials) paired with a data store, held by the participant rather than by any platform.
- **Holder** — any participant that anchors to a sovereign node: a person, an entity (an accountable public role), or a community. Used in this document whenever a statement applies to all three; "citizen" is used only where the individual case is specifically meant.
- **Trust registry** — a published, machine-readable list stating which issuers a given verifier (space, process, or ecosystem body) accepts for which credential types. Trust registries do not issue anything; they record trust decisions, and each verifier chooses which registries to consult.
- **Attestation** — a signed statement by one party about another (that a role was held, that a pledge was made). A **credential** is an attestation packaged in the W3C Verifiable Credentials format, with a schema, an issuer DID, and a status entry, so that any conformant verifier can check it. Every credential is an attestation; not every attestation is a credential.
- **Identity adapter** — the replaceable component inside a Civic Space that turns an incoming request into an authenticated participant identifier. It is the seam that lets a space run stub identity in early phases (Section 12.4) and swap in full DID authentication later without rebuilding.
- **Collective DID** — a DID whose controlling keys are held on behalf of a group rather than by one person, so that the group's identity survives changes in the individuals operating it. Key control requirements are in Section 5.3.

## 1.4 What This Specification Adopts and What It Adds

Most of what an identity layer needs already exists and is well specified. This specification does not re-solve identifiers, credential formats, issuance flows, presentation flows, or device authentication. It adopts them, pins the choices where the base standards leave room, and adds only the civic-layer constructs the existing stack does not cover.

### 1.4.1 Adopted as-is

| Standard | Used for, here | v0.1 profile pin |
|---|---|---|
| **W3C DID Core 1.0** (Recommendation) | Naming every holder and every Civic Space. Identifier syntax, DID documents, verification methods, resolution. | **DID method set is not decided in v0.1.** Section 5.2 gives selection guidance (`did:key`, `did:web`, `did:webvh`); the set a conformant implementation must be able to resolve is an open question (Section 13). |
| **W3C Verifiable Credentials Data Model 2.0** | The credential and presentation data model — personhood, residency, role, membership, and issue credentials. | Securing format profiled in Section 7.1: **SD-JWT VC (`application/dc+sd-jwt`) MUST be supported**; Data Integrity with `eddsa-rdfc-2022` MAY be supported. |
| **W3C Bitstring Status List v1.0** | Revocation and suspension status for issued credentials. | Status is published at the URL in the credential's `credentialStatus` entry; see Section 7.5 for how this coexists with offline signature verification. |
| **OpenID4VCI** | Getting a credential from an issuer into a holder's wallet or managed account. | Adopted as specified; no civic-layer profile in v0.1. |
| **OpenID4VP** | Requesting and presenting credentials to a space or process at the point of participation. | Adopted as specified; the *content* of a request is derived from the Identity Policy Object (Section 8). |
| **SIOPv2** | Proving control of a DID to a relying party (the space or process being asked to trust the holder) — the authentication step itself, with no central identity provider. | Adopted as specified; see Section 6.2 for the session model built on it. |
| **FIDO / WebAuthn** | Authenticating a person to their wallet or account provider (device, passkey, biometric unlock). | Adopted as specified. This authenticates the *user to their custodian*; it is not a substitute for proving DID control to a relying party. |

Nothing in the list above is invented here. An implementer who already has a conformant wallet, issuer, or verifier has most of this specification implemented.

### 1.4.2 Added by this specification

| Added construct | Why the existing stack did not cover it |
|---|---|
| **Citizen Node** (Section 2) | The base standards define identifiers and credentials but not the minimum unit of *participation* — what an implementation must guarantee travels with a person across independent civic environments, and what it must exclude (profile, feed, platform state). |
| **Civic Personal Data Store** (Section 3) | DIDs and VCs carry attestations, not relationships. Nothing in the stack makes memberships, subscriptions, and participation pointers portable, and without them a person can move their credentials but still lose their civic life. |
| **Identity Policy Object** (Section 8) | OpenID4VP expresses a single presentation request. It does not give a space or process a durable, publishable, inheritable statement of its identity requirements that participants can read *before* deciding to participate. |
| **Result stratification** (Section 8.3) | A verifier's normal choice is admit or reject. Civic processes often need to admit broadly and report by verification tier instead, so that low barriers do not force a false claim of uniform assurance. |
| **Holder taxonomy and sovereign nodes** (Section 5) | The base standards are silent on who holds keys for a community or an office. This specification applies one node shape to persons, entities, and communities, and states the key-control expectations for the non-person cases (Section 5.3). |

---

# 2. The Citizen Node

The **Citizen Node** is the fundamental identity primitive of the ecosystem — the minimum viable unit of participation. It is a protocol-level construct, not a product feature, user interface element, or platform-specific record.

## 2.1 Composition

A Citizen Node consists of:

- A **Decentralized Identifier (DID)** conforming to **W3C DID Core 1.0**, associated with a cryptographic key pair the citizen controls
- One or more **Verifiable Credentials** conforming to the **W3C Verifiable Credentials Data Model 2.0**, held in an identity wallet — claims about the citizen (personhood, jurisdiction residency, organizational membership, civic roles) issued by trusted issuers, whose signatures a verifier can check without contacting the issuer (revocation status is a separate check — see Section 7.5)

The Citizen Node is paired with, but distinct from, the citizen's Personal Data Store (Section 3). The node carries identity; the store carries relationships. They travel together and are both citizen-controlled, but they are separate components with separate contents, and either MAY be hosted by a different provider than the other.

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
- **No platform-owned state.** A platform MUST NOT claim ownership of a citizen's identifier or credentials. These remain under the citizen's control at all times.

Because the Citizen Node carries only identity and credentials — not application state — it moves freely across the ecosystem without being tethered to any platform's data model.

---

# 3. The Personal Data Store (PDS)

> **Name collision, read this first.** "PDS" in this ecosystem means **Civic Personal Data Store**, and it is *not* AT Protocol's Personal Data Server. An atproto PDS holds a user's repository of posts and records — exactly the content this store forbids (Section 3.2). The Civic Personal Data Store holds no content: only relationships, memberships, pointers, and preferences. Where ambiguity is possible, write "Civic Personal Data Store" in full.

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
- **Platform-owned application data.** Applications MAY keep their own internal interaction data; that data belongs to the application, not the PDS.

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
- **No layer above may claim the layer below.** An application MUST NOT treat identity or PDS data as proprietary or exclusive.

---

# 5. Sovereign Nodes at Every Holder Scope

The Citizen Node pattern generalizes across the Sovereign Foundation's holder taxonomy. Every participant kind anchors to a sovereign node of the same shape — a DID plus credentials, paired with a data store that is distinct from the node (Section 3):

| Holder | Sovereign node | Paired data store |
|---|---|---|
| Person | **Citizen Node** (DID + credentials) | Personal Data Store |
| Entity (accountable public role) | **Entity Node** (DID + role credentials) | Entity Data Store |
| Community | **Community Node** (collective DID + credentials) | Community Data Store |

The holder set is open, matching the open scope taxonomy of the Civic Space Specification (§1.4): a new holder kind registers a node and data store of this shape without redesigning the identity layer.

## 5.1 Space DIDs

Civic Spaces themselves hold DIDs. Every Civic Space MUST hold a stable **space DID**, distinct from its serving URL, anchored to (or controlled by) the sovereign node of its scope. Requirements for space DIDs — URL binding via the DID document, activity source attribution, and migration survival — are defined in the **Civic Space Specification §3.5**.

## 5.2 DID Method Guidance

Implementations SHOULD start pragmatic and evolve as the ecosystem matures. Candidate methods:

- **`did:key`** — simple cryptographic identifiers; suitable for early phases and ephemeral or low-assurance contexts
- **`did:web`** — web-hosted DID documents; acceptable for holders expected to remain on a stable domain
- **`did:webvh`** (`did:web` + Verifiable History; formerly named `did:tdw`, and now developed at the Decentralized Identity Foundation) — an evolution of `did:web` with verifiable history and controller continuity

Holders that anticipate domain or provider migration — entities and spaces in particular — SHOULD prefer `did:webvh` or an equivalent method with verifiable history, so that identifier continuity survives migration.

## 5.3 Key Control for Non-Person Holders

A person's node has an obvious answer to "who holds the key": the person, or a custodian acting for them (Section 10). Communities and entities do not, and leaving it unstated is how collective identities quietly become one administrator's personal account.

**Community Nodes (collective DIDs).** A **collective DID** is a DID whose controlling keys are held on behalf of a group rather than by an individual. For v0.1 the expected arrangement is a **designated custodian**: a named account provider or named administrator holds the signing keys on the community's behalf, matching the managed-custody default of Section 10. Implementations MAY instead use an **M-of-N multisig** or a **threshold signature** arrangement — arrangements in which several named key-holders must act together before the community can sign, so that no single administrator can speak for the community alone — and SHOULD prefer one of those as the community's governance matures, because they remove the single point of capture. Whichever arrangement is used:

- The DID document MUST list the verification methods currently authorized to act for the community, so that a verifier can tell what it is trusting.
- The custody arrangement MUST be disclosed in the community's space manifest, so members can see who can speak as the community.
- The community MUST be able to rotate keys and migrate to a different custody arrangement without minting a new DID — this is the same portability guarantee as Section 11, applied to collective control.

**Entity Nodes and officeholder change.** An Entity Node is bound to an **office**, not to the person currently holding it (Civic Space Specification §1.4: entity scope is an accountable public role). When the officeholder changes, the v0.1 expectation is that the **DID persists and the keys rotate**: the outgoing officeholder's verification method is removed from the DID document, the incoming officeholder's is added, the role credential naming the outgoing officeholder is revoked, and a new role credential is issued to the incoming one. A new node is not minted, because the accountability record attached to the office — past activity, subscriptions, cross-space references — is keyed to the DID and should survive the transition, which is also why entities are steered toward methods with verifiable history (Section 5.2).

What this does not settle is whether continuity is *always* the right default: a persistent office DID means a signature history spanning multiple officeholders, and a reader who does not check the DID document's history can misattribute a predecessor's statement to a successor. Whether some transitions (a change of party, a change of institution, a contested succession) should mint a new node with a verifiable link to the old one is an **open question** (Section 13).

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

The full challenge-response flow typically occurs **once per session**. After authentication, session continuity mechanisms (comparable to standard web sessions) allow the citizen to move between spaces and processes without repeated authentication. A civic process accessed through an already-authenticated space MAY reuse the existing identity session rather than requiring a new challenge-response, but only where the space hosts the process at the same origin, which is what makes the session shared in the first place. A process reached by direct link or embedded on an external page (Section 6.3) does not share that session and MUST run its own challenge-response.

To be precise about what "single sign-on without a central identity provider" does and does not mean here: there is **no cross-origin session token and no shared login service**. Every space is its own relying party and runs its own challenge-response against the holder's DID. What produces the single-sign-on *feeling* is that the wallet holds the key and can answer a repeat challenge without prompting the holder to log in again — the repetition is absorbed by the wallet, not eliminated by a federation. This is a deliberate trade: it removes the central point of failure and surveillance that a shared IdP would introduce, at the cost of each space performing its own verification.

For the participant, the promise of Section 1.1 holds exactly as stated: they set up their identity once and are not asked to prove themselves again. What changes is only where the work happens — in their own wallet rather than on a company's login server. That is the point of the design: there is no login server to be breached, subpoenaed, or sold.

## 6.3 Access Paths

Credential verification MUST work identically regardless of how the citizen encounters a process: directly at a space, through a space into a hosted process, via a direct link or embedded process on an external page, or from a civic activity feed or dashboard. A process encountered outside any space MUST still be able to authenticate the citizen and verify credentials directly through the identity layer.

---

# 7. Credential Model

## 7.1 Credential Format

Credentials MUST conform to the **W3C Verifiable Credentials Data Model 2.0**. Issuers MUST cryptographically sign credentials and publish public verification keys so credentials can be signature-verified across the ecosystem without contacting the issuer at verification time (revocation status is a separate check — see Section 7.5).

**Securing format — a v0.1 profile decision.** The VC Data Model deliberately leaves the securing mechanism open, which means two conformant implementations can be mutually unreadable. This specification therefore profiles it:

- Implementations MUST support **SD-JWT VC**, media type `application/dc+sd-jwt`. The earlier token `application/vc+sd-jwt` was in use from July 2023 to November 2024 and was changed to avoid colliding with the W3C VC Data Model's own registration; the draft recommends accepting both while deployments transition, so an implementation SHOULD accept either on receipt and emit `application/dc+sd-jwt`.
- Implementations MAY additionally support **W3C Data Integrity** proofs with the `eddsa-rdfc-2022` cryptosuite.
- Verifiers MUST accept SD-JWT VC. A verifier that accepts only Data Integrity proofs is not conformant.
- An issuer's signing key MUST appear as a verification method in the issuer's DID document, so that key discovery is a DID resolution and not an out-of-band arrangement.

SD-JWT VC is the mandatory floor because **selective disclosure is load-bearing in this ecosystem**, not a nice-to-have: the entire disclosure model of Section 8 assumes a holder can present the jurisdiction tier from a residency credential without presenting the address in it. A format without native selective disclosure would make the Identity Policy Object's `attribute_disclosure` rules unenforceable at the wire level.

**The floor is pinned to a revision.** SD-JWT VC is an IETF draft — `draft-ietf-oauth-sd-jwt-vc` — and this specification pins **revision 17, dated 6 July 2026, which has been submitted to the IESG for publication**. Because a draft is a MUST-level requirement here, implementations MUST state and agree on the revision they target: two readers picking up "SD-JWT VC" at different dates are reading different documents and will not necessarily interoperate.

This is a **v0.1 profile choice, not a permanent commitment**. It picks one interoperable floor so that pilot implementations can actually verify each other's credentials, and it is expected to be revisited on pilot feedback — including the possibility of promoting Data Integrity to a second mandatory format if ecosystem partners standardize on it.

## 7.2 Initial Credential Registry

The initial credential categories are:

- **Proof of personhood** — verifies that a DID corresponds to a unique human being. This is the anti-bot, anti-sybil floor: the credential itself carries no name and no address, and serves as the lowest credential threshold separating "open to anyone" from "open to verified humans." It does **not** make the holder anonymous — the presentation still proves control of a DID, and that DID is correlatable across every place it is used (Section 7.4). The verification *interface* is provider-agnostic; the ecosystem supports multiple personhood verification paths (government identity verification, third-party providers, biometric uniqueness, existing personhood networks) without committing to any single method.
- **Jurisdiction / residency credentials** — verify residency within a jurisdiction (e.g., `resident.city`, `resident.county`, `resident.state`, voter district) without revealing unnecessary personal information. Possible issuers include government agencies, identity verification providers, trusted civic institutions, and civic spaces themselves.
- **Civic role credentials** — verify participation roles (assembly participant, facilitator, moderator, committee member), issued by spaces, process providers, or organizations.
- **Organizational membership credentials** — verify membership in civic organizations (nonprofits, unions, coalitions, civic networks).
- **Issue / policy credentials** — used by advocacy organizations to recognize participation or commitments (civic badges, policy pledge credentials). Display semantics for public badges are the domain of the Civic Credentialing pilot.

Different civic processes MAY trust different issuers, and participation rules MAY allow multiple credential pathways to satisfy a requirement.

## 7.3 Published Schemas

Credential schemas MUST be documented and published so that **independent issuers can issue compatible credentials**. Schema publication is a conformance requirement (Section 12), not a courtesy: interoperability of the credential layer depends on any qualifying organization being able to issue credentials that any compliant verifier can validate.

## 7.4 Correlation and Unlinkability: What v0.1 Does Not Provide

This section states plainly what the privacy model of v0.1 delivers, because the gap between "minimum disclosure" and "unlinkable" is where privacy claims usually go wrong, and a reader is entitled to know which one this is.

**v0.1 does not provide cross-context unlinkability.** The authentication model of Section 6 works by proving control of a DID. That DID is a stable identifier. Consequently:

- Presentations made by the same holder are **trivially correlatable by DID** across every space, process, and application they participate in, and across time — indefinitely.
- Any two verifiers who compare notes can link a holder's participation in one civic space to their participation in another, without breaking anything cryptographic and without the holder's involvement.
- Selective disclosure (Section 7.1) limits *which attributes* a verifier learns. It does not prevent that verifier from recognizing the same holder again, nor from being told by another verifier what that holder did elsewhere.
- The `anonymous` and `pseudonymous` disclosure settings of Section 8 are therefore **anonymity toward other participants and toward published outputs**, not toward the verifying space or its operator. A pseudonymous handle is a display choice layered over an authenticated DID; it is not an unlinkable identity.

What v0.1 does provide is real, and worth stating alongside: attribute minimization (a verifier learns the jurisdiction tier, not the address), no central identity provider that verifiers must consult — every space verifies against the holder's own key rather than against a shared login service (where a holder uses managed key custody, which Section 9.1 makes the initial default, their account provider does see each authentication it performs on their behalf; self-custody removes even that, and Section 10 sets out why managed custody is nonetheless the starting point), and no platform ownership of the identifier.

Put plainly: this design stops a stranger from learning how you voted. It does not yet stop the organisation running the vote from being able to learn it. Closing that second gap is a named open question (Section 13), not an oversight, and the honest position for v0.1 is to say so on the screen where people act.

Two candidate remedies — **pairwise / per-space DIDs** and **nullifier-based proof of personhood** (a one-time token derived from the holder's credential and the specific process, which proves "this person has not already acted here" without revealing who they are and without being recognisable in any other process) — would close part of this gap at a cost to cross-space continuity, and both are set out with their trade-offs in Section 13.

A space or process MAY use the Civic Process Specification's `anonymous` and `pseudonymous` disclosure values; those values name a publication rule and are defined there. What an operator MUST NOT do is describe v0.1 participation as anonymous or unlinkable *with respect to the verifying space* — in participant-facing text or in prose accompanying the descriptor — and it SHOULD state in both places, before the holder acts, which parties will be able to link the action to their identifier. Naming the owner and the two places makes this checkable: a reviewer reads the descriptor and the participation screen and can see whether the claim was made.

## 7.5 Revocation and Offline Verification

Two requirements in this document appear to conflict: credentials must be verifiable without contacting the issuer (Sections 2.1, 7.1), and issuers must support revocation (Section 9.2). The resolution is that these are **two separate checks against two separate things**:

- **Signature verification is offline.** A verifier resolves the issuer's DID document, obtains the verification method, and checks the credential's signature. No contact with the issuer is required, and the issuer learns nothing about the verification.
- **Revocation status is a separate lookup.** Credentials MUST carry a `credentialStatus` entry pointing to a **Bitstring Status List** published at a URL. The verifier fetches that list and checks the credential's index in it. The status list is a compressed bitstring covering many credentials at once, so fetching it reveals to its host only that *someone* checked *some* credential in that list — not which one.

Verifiers MUST check the status list for credentials that gate participation. Verifiers MAY cache a status list and SHOULD publish or honor a freshness window appropriate to the decision being made — a moderation role check tolerates a staler list than a one-time ballot eligibility check. A verifier that cannot reach the status list MUST decide by declared policy whether to fail open or fail closed; it MUST NOT silently treat an unreachable list as "not revoked".

---

# 8. Identity Policy Object

Every space and process defines its own identity requirements. Identity policy is governed by three independent dimensions:

- **Assurance** — what identity verification is required: none, email verification, proof of personhood, a residency credential, a role credential, or a combination. *What is verified.* These are **kinds, not rungs on a ladder**: a residency credential and a role credential answer different questions, and neither implies the other (see `assurance_requirements` in Section 8.1).
- **Disclosure** — what information is revealed, and to whom (anonymous → pseudonymous → real identity, with selective disclosure of individual attributes). *What is revealed.* Disclosure settings govern what other participants and published outputs see; they do not hide the holder from the verifying space (Section 7.4).
- **Context** — how assurance and disclosure compose into a policy for a specific space or process, including inheritance (a space MAY define a base policy that its processes inherit and extend). *How the dimensions combine.*

These dimensions are deliberately independent: **high assurance does not require high disclosure**. A process MAY require verified residency credentials while still allowing anonymous or pseudonymous participation in its outputs.

## 8.1 Structure

Spaces and processes express identity requirements using a standard **Identity Policy Object**:

```json
{
  "policy_id": "floyd-secret-ballot-v2",
  "assurance_requirements": ["proof_of_personhood", "resident.city"],
  "optional_credentials": ["civic_role.facilitator"],
  "attribute_disclosure": {
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

- **`policy_id`** — REQUIRED. A string naming this policy. It is what other documents reference: the Civic Process Specification's `disclosure_policy` field accepts an object of the form `{"policy_id": "<id>"}` in place of a shorthand string. Identifiers SHOULD be stable and versioned by the publisher (as in `floyd-secret-ballot-v2`), because a participant who agreed to disclosure terms agreed to a specific policy, and silently editing a policy in place breaks that agreement.
- **`assurance_requirements`** — credential types required for participation eligibility. The list is a **conjunction**: the holder MUST satisfy *every* entry. An entry MAY itself be an array, which is satisfied by **any one** member — this is how a policy expresses "personhood, plus city residency *or* county residency". Assurance levels are **not globally ordered**: `resident.city` is not "higher" than `proof_of_personhood`, it is different, and an implementation MUST NOT infer that holding one satisfies another. The only ordering defined in this specification is the explicit tier order in `result_stratification`.
- **`optional_credentials`** — credential types that may affect participation tier or visibility without being required for basic access
- **`attribute_disclosure`** — which credential attributes go where. Three buckets:
  - **`.public`** — attributes visible to all participants and observers
  - **`.process`** — attributes visible only to process facilitators or operators
  - **`.private`** — attributes retained only by the participant's wallet and never transmitted to the platform

  This field is named `attribute_disclosure`, not `disclosure_policy`, on purpose. A process descriptor's `disclosure_policy` field *names* a policy (Section 8.2); this field is the attribute-routing rule *inside* the policy so named. Giving both the same name would produce a `disclosure_policy` whose value resolves to an object with a `disclosure_policy` in it, which reads as an error even when it is not.
- **`participation_rules`** — maps each participation action (view, participate, vote, moderate) to the credential type or identity mode required to take it; each value is read with the same conjunction semantics as `assurance_requirements`, and because assurance kinds are not ordered, satisfying one action's rule does not imply satisfying another's. Actions not listed inherit the process default
- **`result_stratification`** — an ordered list of tier labels defining how aggregate participation counts are reported, most-assured tier first; each label corresponds to a credential type or identity mode; tiers are reported as counts only and MUST NOT expose individual records. The ordering is local to the policy that declares it (Section 8.3)

The Identity Policy Object MAY be versioned over time as governance models evolve.

## 8.2 Relationship to the Process Specification

The Civic Process Specification's `disclosure_policy` descriptor field references this object: a process declares one of the canonical shorthand strings (`public`, `pseudonymous`, `anonymous`, `secret`) or, for terms the shorthands do not express, an object of the form `{"policy_id": "<id>"}` naming an Identity Policy Object. The chain reads: the process's `disclosure_policy` names a policy; that policy's `attribute_disclosure` says which attributes are published, which reach facilitators, and which never leave the wallet. The policy MUST be declared before participation begins and published in the process descriptor, so participants know the disclosure terms before acting. The Civic Activity Specification §7 defines how disclosure policy constrains activity payloads.

**Eligibility is the same field at two levels.** The Process Specification's `eligibility_requirements` and this specification's `assurance_requirements` are one concept expressed at two scopes: what a participant must hold in order to be allowed to act. A process descriptor MAY give eligibility inline as its own credential list, or by reference to an Identity Policy Object — in which case the referenced object's `assurance_requirements` **is** the process's eligibility rule, with the conjunction semantics defined above.

## 8.3 Result Stratification

Result stratification allows processes to lower participation barriers while reporting results with distinctions by verification tier. Rather than excluding participants who lack higher-level credentials, a process MAY accept broader participation and report results stratified by credential tier:

```json
{
  "verified_residents": { "count": 320 },
  "verified_humans": { "count": 800 },
  "open": { "count": 120 }
}
```

**Tiers are disjoint, not nested.** Each participant is counted in **exactly one** tier: the highest tier in the declared order whose requirement they satisfy. In the example above, the 800 in `verified_humans` are personhood-verified participants who did **not** also present a city residency credential; the 320 residents are not included in that 800. Counts therefore sum to total participation — here 1,240 — and a reader can add the tiers without double-counting. An implementation that reports nested (cumulative) counts is not conformant with this field, because the same numbers would then mean something entirely different to a reader who assumed the other convention.

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
- Support **revocation** by including a `credentialStatus` entry in every issued credential and publishing a **Bitstring Status List** at the URL it names, kept current, so that verifiers can check status without asking the issuer about a specific holder (Section 7.5)
- Be listed in a **trust registry** entry so that spaces and processes can decide whether to trust its credentials

Trust decisions remain local: each space or process decides which issuers it trusts for which credential types.

---

# 10. Stewardship: The Managed Sovereign Identity Model

**Reconciling this section with the decentralization principle.** Section 1.2 states that identity MUST NOT depend on a central authority or database. This section describes a single service, run by one steward, holding DID infrastructure, credential registries, managed keys, and social graph data. Those two statements are compatible, but only if the distinction is made explicitly, so it is made here rather than left to the reader.

- **What is architecturally required** is that no central party be *necessary*: identifiers are DIDs that any resolver can verify, credentials are signed objects any verifier can check without the steward's participation, trust decisions are local to each space, and everything a holder needs to participate is exportable and re-hostable elsewhere. A conformant implementation MUST NOT introduce a dependency that only the steward can satisfy.
- **What the initial deployment does** is concentrate the *operation* of those functions in one nonprofit steward, because an ecosystem with one interoperable provider is easier to bootstrap than one with none. This is a deployment fact, not a property of the protocol.
- **The centralization is a known and real cost, not a technicality.** In particular, the steward maintains user-associated **social graph** metadata — who follows whom, who belongs to which community. That is the most sensitive dataset in the system and the one least protected by cryptography: unlike credentials, a social graph cannot be verified independently and cannot be made unlinkable by careful disclosure. Concentrating it in one operator recreates, for the duration of the transition, exactly the kind of central observer this ecosystem exists to avoid. The mitigations are governance (nonprofit, non-venture-backed control), minimization (the store holds relationships and pointers, never content — Section 3.2), and exit.
- **The exit path** is the portability contract: full export of keys, credentials, and social graph in structured form (Section 11.2) and migration to any other compliant provider (Section 11.3). Portability is what makes the transitional model transitional rather than permanent. If provider migration is never exercised by anyone, the transitional claim has failed regardless of what this document says — so migration SHOULD be demonstrated end-to-end between two independently operated providers during the pilot, rather than assumed from the presence of an export endpoint.

This specification adopts a **Managed Sovereign Identity Model with Transitional Central Stewardship**.

The objective is to preserve individual identity sovereignty without requiring every user to self-custody cryptographic keys. Key custody MAY be managed by a trusted provider (managed wallet model); self-custody MUST remain an available option; holders MUST be able to export their identity keys and credentials.

The ecosystem will begin with a single Civic Identity service implementation operated by a neutral, nonprofit steward (the "Civic Identity Steward"). This steward:

- Operates the reference Civic Identity service
- Maintains DID infrastructure and credential registries
- Maintains user-associated social graph metadata
- Provides managed key custody by default
- Exposes open, standards-based APIs

This initial central stewardship is intended to reduce complexity during early network formation, ensure coherent migration and interoperability guarantees, and build trust under nonprofit governance rather than venture-backed control.

The long-term design objective is a standards-based ecosystem of interoperable providers, in which holders migrate between providers without loss of credentials or relationships and no single operator can permanently centralize control. Over time, governance of the identity layer **may evolve** toward multi-stakeholder stewardship — for example, a consortium of civic organizations, public institutions, and ecosystem stakeholders. This evolution is a stated direction, not a scheduled commitment; the transitional model balances usability (managed custody), network coherence (a single initial provider), and long-term decentralization (provider portability).

---

# 11. Identity Portability

Portability is the load-bearing guarantee of the identity layer.

## 11.1 What Holders Retain

Every holder — person, entity, or community — MUST retain, across any application switch or provider migration:

- Their **DID**, or a successor identifier carrying a verifiable continuity link back to it (Section 11.4)
- Their **credentials**
- Their **attestations**

Space engines MAY store role mappings locally but MUST NOT control identity issuance. Role and authority bindings SHOULD be keyed to the participant's identifier (DID or portable id), not to provider-specific attributes such as email addresses, so that provider migration does not strand the authorization layer.

## 11.2 Export

- **Wallet export:** holders MUST be able to export and migrate credentials to other compatible wallets.
- **Data store export:** holders MUST be able to export their full social graph and preferences in structured form — from a Personal Data Store, an Entity Data Store, or a Community Data Store alike.
- **Key material rule:** private keys live **only** in wallets and identity providers. Space exports carry identity *references* (DIDs only) — key material MUST NOT appear in any space export (Civic Space Specification, portability contract).

## 11.3 Provider Migration

Citizens MUST be able to migrate between Citizen Account Providers. Relationship data (follows, memberships, delegations, endorsements) is logically tied to the citizen's identity record — not owned by any space engine — so that if a space engine or application is replaced, the citizen's relationships remain intact and re-bind to the new instance.

## 11.4 Open Question: DID Stability Across Provider Migration

Whether a citizen's DID remains stable across provider migration, or is re-issued with a verifiable continuity link (as `did:webvh`-class methods support), is an **open question**. It must be answered before the reference identity service mints identifiers at scale, because the answer determines whether downstream systems can key long-lived bindings (roles, delegations, participation history) to the DID itself. Until resolved, implementations SHOULD avoid designs that assume either answer irreversibly.

---

# 12. Conformance

## 12.1 Inherited Conformance

This specification builds on standards with existing conformance machinery. Implementations MUST conform to the following, as applicable to the roles the implementation claims under Section 12.3 — a standard binds an implementation only where the role it claims exercises it:

- W3C Decentralized Identifiers (DID) Core 1.0 — DID syntax, resolution, and DID document requirements
- W3C Verifiable Credentials Data Model 2.0 — credential structure, proofs, and presentation, secured per the profile in Section 7.1
- SD-JWT VC (`draft-ietf-oauth-sd-jwt-vc`, revision 17) — the mandatory securing format per Section 7.1
- W3C Bitstring Status List v1.0 — credential status publication and checking (Section 7.5)
- OpenID4VCI 1.0 (Final) and OpenID4VP 1.0 (Final) — issuance and presentation flows
- SIOPv2 (working-group draft) — self-issued authentication

Which of these binds which role: an **issuer** owes SD-JWT VC issuance and status list publication; a **wallet** owes OpenID4VP and SIOPv2 and the key control that makes proof of DID control meaningful; a **verifier** owes signature verification against the issuer's DID document and status verification against the published status list.

Conformance test suites for these underlying standards apply directly; this specification adds civic-layer requirements on top rather than redefining the base layers.

## 12.2 Civic-Layer Requirements

A conformant implementation additionally:

- Publishes credential schemas for every credential type it issues (Section 7.3)
- Supports the Identity Policy Object for expressing participation requirements (Section 8)
- Satisfies the portability guarantees of Section 11 appropriate to its role (see the conformance classes in Section 12.3)
- For Citizen Account Providers and issuers: meets the role requirements of Section 9

## 12.3 Conformance Classes

"A conformant implementation" is not one thing. Five roles implement this specification, and their obligations barely overlap — an issuer never runs an identity policy, and a space-side verifier never mints a DID. Each role conforms against its own row.

| Role | What it is | Binding sections |
|---|---|---|
| **Citizen Account Provider** | Hosts a holder's node and data store; custodies keys unless the holder self-custodies | 3, 5, 5.3, 6, 7.1, 9.1, 10, 11 |
| **Credential Issuer** | Issues verifiable credentials into the ecosystem | 7.1, 7.3, 7.5, 9.2 |
| **Wallet** | Holds credentials and performs authentication and presentation on the holder's behalf | 2.2, 3.3, 6.1, 7.1, 7.4, 11.2 |
| **Space-side verifier** | The space or process that authenticates a participant and checks their credentials at the point of participation | 5.1, 6, 7.1, 7.4, 7.5, 8, 11.1 |
| **Application / space engine** | The interface or engine a holder participates through — it reads identity and PDS data and owns neither | 2.3, 3, 4, 6.2, 8 |

A single product MAY implement several roles — a Citizen Account Provider typically also ships a wallet — in which case it conforms to the union of the rows it claims. An implementation MUST state which roles it claims; "conformant to the Civic Identity Specification" without a named role is not a meaningful claim.

## 12.4 Conformance Phasing

The reference implementation (`civic-hub`) currently uses **stub identity at a declared assurance level** through a replaceable identity adapter seam, as permitted by the Civic Space Specification §3.1 during early phases: opaque user identifiers, with the space declaring its assurance level in its identity policy. Upgrading from stub identity to full Civic Identity MUST NOT require rebuilding the space.

Convergence from stub identity to the target state defined here is the work of the **Civic Identity pilot**. Specifications in this ecosystem define the target state; reference implementations converge on them through the pilot program.

---

# 13. Open Questions

Beyond the DID-stability question (§11.4), the following remain open design areas. The first four are decisions this specification has deliberately not made; the rest are carried forward from the pilot specification (which discusses each in more depth).

- **Required DID method set.** v0.1 does not name the DID methods a conformant implementation must be able to resolve; §5.2 gives selection guidance only. This is a genuine interoperability gap: two conformant implementations can currently fail to verify each other because one cannot resolve the other's method. It should be closed before the reference service mints identifiers at scale — most likely by naming a small required resolution set rather than a required issuance method, so that holders keep method choice while verifiers keep a floor.
- **Cross-context unlinkability (§7.4).** Should the ecosystem adopt **pairwise / per-space DIDs**, so a holder presents a different identifier to each relying party and cannot be correlated by identifier alone? The cost is cross-space continuity — portable reputation, participation history, and the "authenticate once, participate everywhere" property of §1.1 all assume a stable identifier. A hybrid (stable DID for continuity-bearing contexts, pairwise DIDs for sensitive ones, holder's choice) is the likeliest answer and has not been specified.
- **Nullifier-based proof of personhood.** Should one-person-one-action be enforceable via a per-process nullifier rather than a persistent identifier, so a process can prevent double participation without being able to correlate the participant elsewhere? This is the strongest available answer to §7.4 and depends on personhood-provider capabilities the ecosystem does not control.
- **Entity node continuity across officeholders (§5.3).** v0.1 expects an office-bound DID to persist through key rotation. Whether some transitions should instead mint a new node with a verifiable link to the predecessor — and how a reader is protected from misattributing a predecessor's signed statement to a successor — is undecided.
- **Custodial optionality.** Should the ecosystem offer custodial identity services for non-technical users, and how is it ensured that custody remains genuinely optional — with export always available — rather than becoming a de facto default that recentralizes the layer?
- **Pseudonymous authority.** Should pseudonymous handles be scoped to a single process, a single space, or portable across the ecosystem? Process-scoped handles maximize privacy but prevent reputation and continuity; ecosystem-scoped handles enable richer participation histories but create persistent identifiers that could be profiled.
- **Credential-vs-capability semantics.** Credentials attest *attributes* (who you are, what you're a member of); capabilities grant *authority* (what you may do, to which resource). Where the boundary lies — and when authority should move from role/credential checks to signed, delegable, attenuable capability grants — is the subject of the **Authorization Model Note** (`ecosystem/authorization-model-note.md`), which defines the ecosystem's role-based near-term and capability-based long-term direction.

Additional open questions tracked in the pilot specification include jurisdiction credential issuance and trust, wallet strategy (build vs. adopt existing open-source wallets), selective disclosure depth, trust registry governance, identity policy context inheritance, minimum identity requirements for open modes, PDS scope and consent governance, and cross-ecosystem social graph interoperability.

---

## References

### Normative

- **W3C Decentralized Identifiers (DIDs) v1.0** — W3C Recommendation, 19 July 2022. https://www.w3.org/TR/did-core/
- **W3C Verifiable Credentials Data Model v2.0** — W3C Recommendation. https://www.w3.org/TR/vc-data-model-2.0/
- **SD-JWT-based Verifiable Credentials (SD-JWT VC)** — IETF OAuth Working Group draft `draft-ietf-oauth-sd-jwt-vc`, revision 17, 6 July 2026, submitted to the IESG for publication; media type `application/dc+sd-jwt`. The mandatory securing format per Section 7.1.
- **W3C Verifiable Credentials Data Integrity v1.0** — https://www.w3.org/TR/vc-data-integrity/ — with the **`eddsa-rdfc-2022`** cryptosuite defined in **Verifiable Credential Data Integrity EdDSA Cryptosuites v1.0**, https://www.w3.org/TR/vc-di-eddsa/. Together these are the OPTIONAL second securing format per Section 7.1.
- **W3C Bitstring Status List v1.0** — credential status publication and checking, per Section 7.5. https://www.w3.org/TR/vc-bitstring-status-list/
- **OpenID for Verifiable Credential Issuance (OpenID4VCI) 1.0** — OpenID Foundation, Final specification.
- **OpenID for Verifiable Presentations (OpenID4VP) 1.0** — OpenID Foundation, Final specification.
- **Self-Issued OpenID Provider v2 (SIOPv2)** — OpenID Foundation working-group draft, not yet Final.
- **W3C Web Authentication (WebAuthn) Level 2** and **FIDO2 / CTAP** — FIDO Alliance and W3C, for authenticating a person to their wallet or account provider (Section 6.1).
- **JSON-LD 1.1** — W3C Recommendation, where JSON-LD processing is used by the VC Data Model.
- **RFC 2119** and **RFC 8174** — requirement keywords, per Notation and Conformance Language.

**Version pinning note.** DID Core is cited at **1.0**, the published Recommendation and the version this specification targets. **DID 1.1 reached Candidate Recommendation on 5 March 2026** and is not yet a Recommendation; it is not the target of this specification, and an implementation building against it should say so explicitly rather than claiming conformance to "DIDs". The same applies to the two drafts above: SD-JWT VC is pinned to a revision (Section 7.1) and SIOPv2 can still change under an implementation, while OpenID4VCI 1.0 and OpenID4VP 1.0 are Final and cannot.

### Informative

- **Civic.Social Terminology Glossary** — canonical definitions for ecosystem-wide terms.
- Companion specifications: Civic Space, Civic Process, Civic Activity, and the Authorization Model Note.

**ActivityPub** and **ActivityStreams 2.0** are normative for the **Civic Activity Specification**, not for this one. Nothing in the identity layer depends on them, and they are omitted here to avoid implying an identity-layer obligation that does not exist.

---

**Version:** 0.1
**Status:** Draft
**Last updated:** July 26, 2026
**Contact:** contact@civic.social
