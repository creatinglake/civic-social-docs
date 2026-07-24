---
status: draft
last-reviewed: 2026-07-03
owners: [adam]
version: 0.1
---

# Civic Credentialing Pilot

Civic.Social Infrastructure Program

> **Status and governance.** This specification is a **working draft published for community discussion** — not a final standard. The credential schemas, the mutual-consent protocol, and the issuer requirements defined here are contributed by Civic.Social as starting points, with the expectation that they evolve in the open alongside the W3C Verifiable Credentials community and adjacent credentialing efforts (Open Badges 3.0 and the broader 1EdTech/W3C convergence among them). Breaking changes between pre-1.0 versions are expected and welcomed.

> **Terminology.** **Civic Credentialing** is the canonical name for this layer (see the [Terminology Glossary](../../canon/terminology.md)). A **badge** is the publicly-displayed type of credential — every badge is a verifiable credential, but not every credential a citizen holds is displayed as a badge.

---

## Table of Contents

### [How to Read This Document](#how-to-read-this-document-1)

### Executive Overview
1. [Executive Summary](#1-executive-summary)
2. [Purpose of the Pilot](#2-purpose-of-the-pilot)
3. [Relationship to the Civic.Social Pilot Program](#3-relationship-to-the-civicsocial-pilot-program)
4. [Strategic Importance](#4-strategic-importance)

### Strategic Context
5. [What is a Civic Credential](#5-what-is-a-civic-credential)
6. [The Badge / Credential Issuer — an Infrastructure Role](#6-the-badge--credential-issuer--an-infrastructure-role)
7. [Credential Categories](#7-credential-categories)

### Credentialing Architecture
8. [The Mutual-Consent Display Model](#8-the-mutual-consent-display-model)
9. [Credential Lifecycle and Activity Emission](#9-credential-lifecycle-and-activity-emission)
10. [Issuer Requirements](#10-issuer-requirements)
11. [Credential Schemas](#11-credential-schemas)
12. [Display Surfaces Across Space Scopes](#12-display-surfaces-across-space-scopes)
13. [Participation Credentials — Automation from Civic Activities](#13-participation-credentials--automation-from-civic-activities)

### Pilot Implementation
14. [Minimum Viable Pilot Scope](#14-minimum-viable-pilot-scope)
15. [Pilot Phases and Timeline](#15-pilot-phases-and-timeline)
16. [Pilot Demonstration Scenarios](#16-pilot-demonstration-scenarios)
17. [Success and Validation Criteria](#17-success-and-validation-criteria)
18. [Expected Deliverables](#18-expected-deliverables)

### Ecosystem and Partnerships
19. [Dependencies on the Civic Identity Pilot](#19-dependencies-on-the-civic-identity-pilot)
20. [Relationship to Other Civic.Social Pilots](#20-relationship-to-other-civicsocial-pilots)
21. [Potential Pilot Partners](#21-potential-pilot-partners)

### Pilot Plan
22. [Risks and Mitigations](#22-risks-and-mitigations)
23. [Open Questions for Further Design](#23-open-questions-for-further-design)

### Conclusion
24. [Conclusion](#24-conclusion)

---

<a id="how-to-read-this-document-1"></a>
## How to Read This Document

This document is the canonical specification for the Civic Credentialing Pilot. It is written for multiple audiences.

**Funders and program evaluators** should focus on the Executive Overview (sections 1–4), the Success and Validation Criteria (section 17), and the Pilot Plan (sections 22–23). These explain why a recognition layer matters, how the pilot is measured, and where the honest tensions are.

**Prospective issuers** — advocacy organizations, civic institutions, reform coalitions — should focus on the Credential Categories (section 7), the Mutual-Consent Display Model (section 8), and the Issuer Requirements (section 10). Together these define what it takes to issue credentials into the ecosystem and what protections govern how those credentials appear.

**Technical implementers** should focus on the Credentialing Architecture (sections 8–13) and the Expected Deliverables (section 18). These define the lifecycle, the activity contract, the schema publication model, and the display components the pilot will build.

This pilot specification refers throughout to companion documents:

- **[Civic Identity Pilot](../civic-identity/civic-identity-pilot-spec.md)** — the identity foundation this pilot builds on: DIDs, Verifiable Credentials, wallets, the credential verification interface, and the trust registry. This pilot is downstream of that work and does not redefine it (see section 19).
- **[Civic Space Specification](../../ecosystem/civic-space-spec.md)** — the host contract for the spaces where badges are displayed, including the Badge / Credential Object (§4.13) and the infrastructure-role taxonomy (§1.5).
- **[Civic Activity Specification](../../ecosystem/civic-activity-spec.md)** — the distribution layer through which credential lifecycle events become visible across the ecosystem.
- **[Terminology Glossary](../../canon/terminology.md)** — canonical vocabulary for all Civic.Social documents.

**Conformance phasing.** As with every specification in this ecosystem, this document defines the *target state*. Reference implementations converge on the target through the pilot program; where the pilot ships something narrower than the specification describes, the gap is named explicitly rather than papered over.

---

<a id="1-executive-summary"></a>
## 1. Executive Summary

The Civic Credentialing Pilot will design, prototype, and validate the **recognition layer** of the Civic.Social ecosystem: verifiable civic badges and the display infrastructure that makes civic recognition visible, portable, and trustworthy.

Civic recognition today is either invisible or unverifiable. A representative signs a reform pledge, and the record of that commitment lives on one organization's website, in one format, with no way to verify it later or carry it anywhere else. A citizen participates in a citizen assembly, and nothing durable marks that participation. An organization endorses a candidate, and the endorsement is a press release, not a checkable claim. The signals that should let citizens assess credibility — who committed to what, who participated in what, who stands with whom — are fragmented across platforms that do not interoperate and cannot be independently verified.

The pilot addresses this with three moves:

- **W3C Verifiable Credentials as the substrate.** Every civic badge is a verifiable credential issued against the credential model established by the Civic Identity Pilot. Credential schemas are published openly, so any organization — inside or outside the Civic.Social ecosystem — can issue compatible credentials without permission or bespoke integration.
- **The mutual-consent display model as the structural protection.** An issuer issues; a badge appears on a public space only when the subject accepts. Either side can revoke, revocations are visible, and the offer itself is part of the public record even when display is declined. This single mechanism prevents adversarial labeling while keeping issuer behavior auditable — the two failure modes that have historically made public labeling systems either weapons or noise.
- **Verify-on-render display surfaces at every space scope.** Badge display components for entity-scoped Representative Spaces (the accountability surface), individual-scoped Citizen Dashboards (personal recognition), and community-scoped Civic Hubs (member roles and recognition) — all consuming credentials by cryptographic verification, never by trusting the space that displays them.

The pilot also delivers the automation bridge between participation and recognition: **participation credentials issued automatically from civic process outcomes**, triggered by the `civic.process.*` activities that processes already emit. When a citizen completes a deliberation or a vote closes, the recognition follows from the activity record — no manual issuance required.

The pilot proves the recognition layer the way the Civic Process Plugin Pilot proves the participation layer: with a reference implementation (a reference issuer service), published contracts (the civic credential schema set and the mutual-consent protocol), and measurable validation (an independent organization issuing a compatible credential from the published schemas alone).

---

<a id="2-purpose-of-the-pilot"></a>
## 2. Purpose of the Pilot

The Civic Credentialing Pilot defines what a civic credential is in the Civic.Social ecosystem, demonstrates the full issue → offer → accept → display → revoke lifecycle end to end, and proves that recognition can be made visible without being made weaponizable.

The pilot is the bridge between two layers that already exist in the program. The Civic Identity Pilot establishes the substrate — DIDs, verifiable credentials, wallets, verification. The Civic Activity Feed makes participation visible. This pilot activates the layer between them: it turns credentials from private cryptographic objects into **public civic meaning** — badges on a representative's profile, recognition on a citizen's dashboard, roles in a community hub — while preserving the verifiability and subject control that the identity layer guarantees.

The pilot is explicitly *not* an identity pilot in disguise. It does not build DID infrastructure, wallets, or verification protocols; it consumes them (section 19). Its own contribution is everything above the credential itself: the issuer contract, the consent protocol, the display components, and the automation that connects recognition to participation.

---

<a id="3-relationship-to-the-civicsocial-pilot-program"></a>
## 3. Relationship to the Civic.Social Pilot Program

The Civic Credentialing Pilot is one component of the broader Civic.Social Infrastructure Program, operating alongside complementary pilots:

**Civic Identity Pilot** — defines the DID/VC infrastructure, wallet strategy, credential verification interface, and trust registry that this pilot builds on. This pilot is the most directly downstream consumer of that work (section 19).

**Civic Process Plugin Pilot** — defines the participation layer whose outcomes this pilot converts into participation credentials. Process plugins emit the `civic.process.*` activities that trigger automated credential issuance.

**Civic Activity Feed Pilot** — defines the aggregation and distribution layer through which credential lifecycle activities (issuance, acceptance, revocation) become visible in feeds.

**Civic Hubs Pilot** — defines the community-scoped spaces where member recognition and role badges are displayed.

As with the rest of the program, there is a preferred sequencing but no hard gate: the pilot can run against the Civic Identity Pilot's reference infrastructure where it exists, and against a minimal stub issuer-and-verifier where it does not, provided the stub honors the same interfaces.

---

<a id="4-strategic-importance"></a>
## 4. Strategic Importance

Every other layer of the Civic.Social ecosystem answers a question of *action*: where participation happens (spaces), what participation is (processes), how participation travels (activities), who is participating (identity). The credentialing layer answers the question of *record*: what has this person, organization, or officeholder verifiably done and committed to?

That question matters most where power is held. The entity-scoped Representative Space is designed around a deliberate asymmetry: entities control their voice, but they do not control the accountability data displayed alongside it. Civic credentials are a core part of that accountability data. A pledge badge on a representative's space is a third-party-issued, cryptographically verifiable, revocable public claim — not a self-description. When the issuing organization concludes the commitment has been broken, it revokes, and the revocation is itself visible. This is the difference between a profile and a record.

The same layer serves recognition in the other direction. Civic participation is chronically under-recognized: people show up to assemblies, deliberations, and consultations, and nothing durable acknowledges it. Participation credentials — issued automatically from process outcomes, held in the citizen's own wallet, displayed at the citizen's own discretion — give participation a portable, verifiable record that outlives any single platform.

Without the mutual-consent protection, none of this is safe to build. A public labeling system where issuers can unilaterally pin claims on subjects is an adversarial-labeling machine; a system where subjects can silently scrub their record is an accountability theater. The mutual-consent display model (section 8) is the structural design that makes the recognition layer worth having — and it is the pilot's most important contribution to the broader credentialing field.

---

<a id="5-what-is-a-civic-credential"></a>
## 5. What is a Civic Credential

A **civic credential** is a W3C Verifiable Credential making a civic claim: a commitment made, an affiliation held, a participation completed, a role occupied. It is issued by an identified issuer (a DID) about an identified subject (a DID), against a published schema and published issuance criteria, with checkable revocation status.

A **badge** is the publicly-displayed type of credential. The distinction matters:

- **Possession** is governed by the identity layer. A credential in a citizen's wallet is the citizen's to hold and to present via selective disclosure — proving a claim to a specific verifier without making it public. Nothing in this pilot changes that.
- **Display** is governed by this pilot's mutual-consent protocol. A credential becomes a badge when the subject consents to its public display on a Civic Space. Display-consent applies to *public display on spaces*, not to possession or to selective presentation.

Credentials conform to the Badge / Credential Object of the Civic Space Specification (§4.13): issuer DID, recipient DID, criteria reference, and status (active / revoked), compatible with Verifiable Credential standards. This pilot extends that minimal object model with the display-state machine (section 8) and the schema set (section 11); it does not redefine the VC substrate, which belongs to the Civic Identity Pilot's credential model.

---

<a id="6-the-badge--credential-issuer--an-infrastructure-role"></a>
## 6. The Badge / Credential Issuer — an Infrastructure Role

The **Badge / Credential Issuer** is an **infrastructure role**, not a Civic Space. This distinction, established in the Civic Space Specification (§1.5), is load-bearing for the whole design.

A Civic Space hosts processes at a scope — community, individual, or entity — and carries the full space contract: sovereign foundation, portability, plugin hosting, discovery. An issuer does none of that. An issuer is a **service provider that issues verifiable credentials into the ecosystem**: pledges, endorsements, attestations of office, participation records. Its obligations live in the identity layer's issuer requirements (section 10), not in the space contract.

Keeping issuers out of the space taxonomy has practical consequences the pilot depends on:

- **Any organization can be an issuer without operating a space.** An advocacy organization with a DID, a published schema, and a status endpoint can issue credentials into the ecosystem without running hub infrastructure. This keeps the barrier to issuing low and the issuer population diverse.
- **Spaces display; issuers attest.** A Representative Space displays badges but never issues the accountability credentials shown on it. The separation is what makes the display trustworthy: the space cannot mint its own accountability record.
- **Issuer trust is registry-governed, not federation-governed.** Whether a verifier trusts an issuer is a question for the trust registry (curated by Civic.Social initially — see section 10), not a question of which spaces federate with which.

An organization may of course operate both a Civic Space *and* an issuer service — a civic hub that issues facilitator role credentials, for example. Architecturally these remain two roles with two contracts, even when one operator holds both.

---

<a id="7-credential-categories"></a>
## 7. Credential Categories

The pilot's initial civic schema set covers four categories. The first three are pilot scope; the fourth is named and explicitly deferred.

### 7.1 Political Commitment

Credentials attesting that a subject — typically an entity: an officeholder or candidate — has made a public commitment: signed a reform pledge, endorsed a policy position, committed to a structural reform. Examples: *Signed [a reform] Pledge*, *Supports [an anti-corruption act]*, *Committed to Participatory Budgeting*.

These are the highest-stakes category. They are claims about promises, they attach to people who hold or seek power, and they are exactly where both adversarial labeling (an issuer pinning an unwanted label on an opponent) and accountability erosion (a subject quietly shedding an inconvenient commitment) will be attempted. The mutual-consent model (section 8) exists primarily for this category. Every political commitment credential MUST carry an evidence reference (the public record of the commitment) and a criteria reference (what the issuer requires before issuing).

### 7.2 Organizational Affiliation

Credentials attesting membership in, or endorsement by, a named organization: union membership, coalition participation, an organization's endorsement of a candidate. The claim is about a relationship between two parties, which is why both parties hold revocation rights: the organization can revoke when the relationship ends, and the subject can remove the badge when they no longer wish to display the affiliation.

### 7.3 Civic Participation

Credentials recording engagement in civic processes: participated in a citizen assembly, completed a deliberation, voted in a participatory budgeting round, served as a facilitator. These are the volume category, and they are distinctive in two ways. First, they can be **issued automatically from process outcomes** via the activity layer (section 13) — recognition follows from the participation record rather than from manual issuer action. Second, their subjects are typically individuals, so they live primarily in the citizen's wallet under the possession rules (section 5), displayed publicly only when the citizen chooses.

Participation credentials record *that* participation occurred, never *what* the participant said or how they voted. The credential inherits the disclosure constraints of the process it derives from: a credential derived from a secret-ballot vote attests participation only.

### 7.4 Civic Reputation — Named and Deferred

Composite or evaluative credentials issued by third-party evaluators: responsiveness scores, engagement grades, aggregate assessments built from underlying records. This category is real — the ecosystem's accountability vision includes evaluative layers like a civic grade — and it is **explicitly out of scope for this pilot**. Composite scoring raises methodological questions (who defines the metric, how is it contested, how does it avoid laundering editorial judgment as cryptographic fact) that the primary-credential categories do not. The pilot names the category so the schema namespace reserves room for it, and defers it until the primary layer it would aggregate over exists and has been observed in use.

---

<a id="8-the-mutual-consent-display-model"></a>
## 8. The Mutual-Consent Display Model

The mutual-consent display model is the pilot's central protocol contribution: the structural protection that makes public civic labeling safe enough to build. It applies to the **public display of credentials on Civic Spaces** — primarily entity-scoped spaces, where the stakes are highest — and it rests on one rule:

> **An issuer can always issue. A badge only displays when the subject accepts. Either side can end the display. Every step is on the record.**

### 8.1 The Display-State Machine

1. **Offer.** The issuer issues a credential naming the subject and offers it for display. The offer is recorded and the subject is notified. From this moment the offer is part of the public record of the issuer's behavior — visible in the credential activity stream (section 9) — even if the subject never accepts.
2. **Accept / Decline.** The subject accepts (the credential displays as a badge on the relevant space) or declines (it does not display — but the offer and the decline remain in the activity record). A subject may also simply not respond; an unanswered offer never displays.
3. **Display.** While accepted and unrevoked, the badge renders on the space's display surface with issuer attribution, criteria reference, evidence link, and live verification (section 12).
4. **Revoke (issuer-side).** The issuer revokes — a pledge broken, a membership lapsed. The badge stops displaying, and the revocation is visible in the record. Revocation is not deletion: the history that the credential was issued, accepted, and later revoked remains reconstructible from the activity stream.
5. **Remove (subject-side).** The subject withdraws display consent — an affiliation they no longer wish to show. The badge stops displaying, and the removal is likewise visible in the record.

### 8.2 What the Model Prevents — in Both Directions

**Against adversarial labeling:** no issuer can make a badge appear on a subject's space unilaterally. A hostile organization can *offer* a derogatory credential, but it will never display without acceptance — the subject's space cannot be defaced by issuance alone.

**Against accountability erosion:** the offer, the decline, the revocation, and the removal are all visible events. A representative who declines every accountability-relevant offer, or who removes a pledge badge the week before breaking the pledge, has created a public record of exactly that. Subjects control what displays; they do not control the record of what was offered and what happened to it.

This symmetry is the design. Display is consensual; history is not editable.

### 8.3 Scope of the Model

Display-consent governs public display on spaces. It does **not** govern:

- **Possession.** Credentials held in a citizen's wallet are the citizen's, full stop.
- **Selective disclosure.** A citizen presenting a credential privately to a specific verifier (proving assembly participation to apply for a facilitator role, say) is exercising the identity layer's presentation flows, which require no public display consent.
- **The issuer's own records.** An issuer may publish its own list of credentials it has issued and revoked — that is the issuer speaking about its own attestations, on its own surfaces, governed by the transparency expectations in section 10.

---

<a id="9-credential-lifecycle-and-activity-emission"></a>
## 9. Credential Lifecycle and Activity Emission

Every transition in the display-state machine emits a **Civic Activity** through the standard activity layer, so recognition — and issuer behavior — is visible in feeds the same way participation is. There is no side channel: the credential record is reconstructible from the activity stream, in keeping with the ecosystem's no-silent-state-changes principle.

The pilot defines the credential activity types under the extension namespace convention (`civic.<domain>.<verb>`) of the Civic Activity Specification (§4.5):

- `civic.credential.offered` — issuer has issued a credential and offered it for display
- `civic.credential.accepted` — subject accepted; badge now displays
- `civic.credential.declined` — subject declined display
- `civic.credential.revoked` — issuer revoked the credential
- `civic.credential.removed` — subject withdrew display consent

On the wire these follow Civic Activity Specification v0.1: the type field is `event_type` and the transport endpoint is `GET /events` (the ratified v0.1 compatibility baseline; the `activity_type` / `GET /activities` rename arrives with v0.2).

Two payload rules are binding:

- **Activities carry references, not credential contents.** A credential activity's `data` payload identifies the credential (id, schema, issuer DID, subject DID, status endpoint) — it never embeds the full credential or subject attributes beyond what the display state already makes public. Verification happens against the credential and its status endpoint, not against the activity.
- **Visibility follows the display state and the subject scope.** Offer/accept/revoke activities concerning entity-scoped public badges are `public` — that visibility *is* the auditability. Activities concerning individually-held participation credentials default to `restricted` (notification to the subject), becoming public only if the citizen opts into public display. Recognition of officeholders is public by design; recognition of citizens is public by choice.

Credential activities are notification and audit infrastructure first, feed content second: feed-worthiness policy (which credential activities surface in which lenses) belongs to the Civic Activity Feed's classification layer, not to this pilot.

---

<a id="10-issuer-requirements"></a>
## 10. Issuer Requirements

The issuer requirements are the infrastructure-role contract: what an organization must satisfy to issue civic credentials that the ecosystem's display surfaces will render. The pilot formalizes five requirements:

1. **Issuer DID.** The issuer holds a stable DID and signs every credential with it. Anonymous issuance is not supported: attestation without attributable authorship is rumor, not recognition.
2. **Published issuance criteria.** Every credential type the issuer offers must reference publicly-published criteria — what the issuer requires before issuing, in human-readable form. A pledge badge references the pledge text and signing process; a membership badge references what membership means. Criteria are what let a citizen viewing a badge answer "what did earning this actually require?"
3. **Revocation support.** Every issued credential carries a status reference, and the issuer maintains a checkable status endpoint (or status-list mechanism) that verifiers and display components can poll. An issuer that cannot revoke cannot issue commitments — a pledge badge that survives a broken pledge is worse than no badge.
4. **Activity emission.** The issuer emits the `civic.credential.*` activities of section 9 for every issuance, acceptance, revocation, and removal it is party to, through the standard activity layer. This is what makes issuer behavior auditable and recognition visible in feeds.
5. **Trust registry entry.** The issuer is listed in the ecosystem's trust registry, with its DID, its credential types, and its criteria references.

**On the trust registry's governance.** The registry is **curated by Civic.Social initially**, as an extension of the identity governance posture established in the Civic Identity Pilot: a curated registry of trusted credential issuers that **may evolve toward more decentralized, multi-stakeholder governance** as the ecosystem grows. The pilot deliberately does not define decentralization triggers or binding governance transitions — that question needs broader ecosystem input than a pilot can convene, and pretending otherwise would be false precision. What the pilot does commit to is honesty about the tension this creates (section 22): whoever curates the registry holds real gatekeeping power over who can make displayable civic claims, and the pilot documents that power rather than obscuring it. Registry listing gates *default display trust*, not issuance itself — anyone can issue schema-compatible VCs; verifiers and spaces decide what they render.

---

<a id="11-credential-schemas"></a>
## 11. Credential Schemas

The pilot publishes the **initial civic credential schema set** — machine-readable schemas for the three in-scope categories (political commitment, organizational affiliation, civic participation), issued against the Civic Identity Pilot's credential model and compatible with the W3C Verifiable Credentials data model.

The schemas are published openly, with documentation and worked examples, under an explicit design goal: **an independent organization should be able to issue a compatible civic credential from the published schemas alone** — no partnership agreement, no bespoke integration, no conversation with Civic.Social required. That property is a named success criterion (section 17), because it is the difference between an open credentialing layer and a platform with an API.

Each schema defines, at minimum: the credential category and type, the claim structure, the required criteria reference, the required evidence reference (for commitment credentials), the jurisdiction field, and the status/revocation reference. Schema versioning follows the ecosystem's contract-versioning discipline: schemas are version-tagged, and display components declare which schema versions they render.

Alignment with existing credential vocabularies — Open Badges 3.0 in particular, which is itself converging on the W3C VC data model — is an explicit design consideration: where an existing open vocabulary already expresses what a civic schema needs, the schema should extend rather than duplicate it. The depth of that alignment is an open question (section 23) the schema-drafting phase will resolve concretely.

---

<a id="12-display-surfaces-across-space-scopes"></a>
## 12. Display Surfaces Across Space Scopes

Badges display on Civic Spaces, and every space scope has a display surface with its own character:

- **Entity scope — Representative Space: the accountability surface.** Badges on a representative's or candidate's space are the flagship use: third-party-issued commitment and endorsement records displayed alongside — but never controlled by — the entity's own voice. The entity accepts or removes; it cannot edit badge content, alter criteria, or scrub the offer/revocation history. Candidates get the same surface as incumbents, letting citizens compare commitment records across a ballot.
- **Individual scope — Citizen Dashboard: the personal recognition surface.** The citizen's own console shows the credentials they hold (private, from the wallet) and lets them choose which display publicly as badges on their profile. Default private; public by opt-in.
- **Community scope — Civic Hub: the roles and recognition surface.** Hubs display member role badges (facilitator, moderator, assembly alum) and community recognition, integrating credential display into membership surfaces.

One discipline binds all three: **display components consume credentials by verification, not by trust in the displaying space.** A badge component embedded in any space — or on an external website — renders a badge only after verifying the credential's signature against the issuer's DID, checking the issuer against the trust registry, and checking revocation status. The space hosting the component contributes pixels, not truth. This is what makes the components safely embeddable anywhere: a compromised or dishonest space cannot forge a verified badge, only decline to show one.

Verification results may be cached, but staleness is bounded: a revoked credential must disappear from every conforming display surface within the bounded revocation-propagation window defined by the mutual-consent protocol specification (the pilot's working bound: 24 hours worst-case, minutes in the common case). The bound is a conformance requirement, not an aspiration — it is tested in the validation criteria.

---

<a id="13-participation-credentials--automation-from-civic-activities"></a>
## 13. Participation Credentials — Automation from Civic Activities

Manual issuance does not scale to participation recognition, and it should not have to: the ecosystem already produces a signed, structured record of participation — the activity stream. The pilot builds the bridge that turns that record into credentials.

The **participation-credential automation service** subscribes to the activity layer and issues civic participation credentials when configured process outcomes occur. The trigger vocabulary is the standard `civic.process.*` activity set: a `civic.process.ended` or `civic.process.result_published` activity for an assembly can trigger participation credentials for its participants; a `civic.process.action_taken` for a facilitation role can trigger a role credential.

Design rules:

- **Issuance policy is declared, not implied.** A process (or the space hosting it) opts into participation credentialing by declaring which activities trigger which credential schemas for which participant roles. No process produces credentials by surprise.
- **The credential inherits the process's disclosure policy.** Participation credentials attest participation, never ballot content or statement text (section 7.3).
- **The automation service is itself an issuer.** It satisfies every issuer requirement of section 10 — its own DID, published criteria (the declared issuance policy), revocation support, activity emission, registry entry. Automation changes who pushes the button, not the contract.
- **Delivery respects possession rules.** Auto-issued credentials are offered to the citizen's wallet; the citizen is notified and accepts into possession. Public display remains a separate, later, optional choice.

This is the piece that closes the loop the program has been building toward: participation (processes) → visibility (activities) → recognition (credentials) — each layer consuming the previous one's standard contract, no bespoke integration anywhere in the chain.

---

<a id="14-minimum-viable-pilot-scope"></a>
## 14. Minimum Viable Pilot Scope

### What the Pilot Will Demonstrate

The end-to-end credentialing lifecycle:

1. An issuer registers: DID, credential types, published criteria, status endpoint, trust registry entry.
2. The issuer issues a political-commitment credential to an entity and offers it for display. A `civic.credential.offered` activity is emitted.
3. The entity accepts; the badge renders on its Representative Space via the badge display component, verify-on-render. `civic.credential.accepted` is emitted.
4. A citizen viewing the space inspects the badge: issuer, criteria, evidence, live verification status.
5. The issuer later revokes; the badge disappears from display within the bounded window; the revocation is visible in the record.
6. In parallel: a civic process completes, the automation service issues participation credentials to participants' wallets from the `civic.process.*` activity record, and one participant opts into public display on their dashboard.
7. An independent organization, working only from the published schemas and onboarding documentation, issues a compatible credential that the display components verify and render.

### What is In Scope

- **Reference issuer service** — the issue / offer / accept / revoke lifecycle, the credential status endpoint, and activity emission (section 18).
- **The initial civic credential schema set** — political commitment, organizational affiliation, civic participation — published with documentation and examples.
- **The mutual-consent protocol specification** — the display-state machine, activity contract, and revocation-propagation bounds, published as a standalone protocol document.
- **Badge display components** — embeddable, verify-on-render, for all three space scopes.
- **Participation-credential automation** — the activity-triggered issuance service with declared issuance policies.
- **Issuer onboarding documentation** — everything an independent organization needs to become a conforming issuer.
- **Trust registry integration** — consuming the identity layer's registry, with the curated-initially governance posture documented honestly.

### What is Explicitly Out of Scope

- **Civic Reputation composite scoring.** Named and deferred (section 7.4). No grades, scores, or evaluative aggregates in this pilot.
- **Cross-ecosystem badge import.** Rendering credentials issued in other ecosystems (legacy Open Badges 2.x, platform-specific badge systems) is future work; schema *alignment* with Open Badges 3.0 is in scope, import pipelines are not.
- **Zero-knowledge presentations beyond selective disclosure.** The pilot supports the identity layer's selective-disclosure presentations as they exist; ZK proof systems beyond that are the identity layer's future roadmap, not this pilot's.
- **Operating a production trust-registry governance body.** The pilot documents the curated posture and the open governance question; it does not convene or operate a multi-stakeholder registry authority.
- **Profile systems generally.** Spaces and their profile surfaces are the Civic Space and Representative Space work; this pilot delivers the badge components those surfaces embed, not the surfaces themselves.

---

<a id="15-pilot-phases-and-timeline"></a>
## 15. Pilot Phases and Timeline

**Indicative duration: 5–8 months**, moderated by what already exists: the activity layer is live (`GET /events` is in production), representative-space surfaces are running, and the Civic Identity Pilot defines the credential substrate. The pilot's work is the credentialing layer proper, not its foundations.

### Phase 1 — Protocol and Schema Drafting (1.5–2 months)

Draft the mutual-consent protocol specification (state machine, activity types, revocation-propagation bounds) and the initial civic credential schema set. Resolve the Open Badges 3.0 alignment question concretely per schema. Validate schema drafts against the Civic Identity Pilot's credential model and the Civic Space Specification's Badge / Credential Object.

Key deliverables: mutual-consent protocol spec draft, civic credential schema set draft, `civic.credential.*` activity type definitions.

### Phase 2 — Reference Issuer and Display Components (2–3 months)

Build the reference issuer service (issue / offer / accept / revoke lifecycle, status endpoint, activity emission) and the badge display components (embeddable, verify-on-render, revocation-aware caching). Integrate the trust registry lookup. Demonstrate the manual lifecycle end to end on an entity-scoped surface.

Key deliverables: reference issuer service, badge display components for the entity and individual scopes, working manual lifecycle demonstration.

### Phase 3 — Participation Automation and Multi-Scope Display (1–1.5 months)

Build the participation-credential automation service against the live `civic.process.*` activity stream, with declared issuance policies. Extend display to the community scope (hub member roles/recognition). Validate wallet delivery and the possession-vs-display separation for citizen-held credentials.

Key deliverables: automation service, community-scope display, end-to-end participation-recognition loop.

### Phase 4 — Independent Issuer Validation and Publication (1–1.5 months)

The decisive phase: engage at least one independent organization to issue a compatible credential from the published schemas and onboarding documentation alone, and iterate the schemas and documentation based on where that attempt struggles. Run the adversarial and revocation validation scenarios (section 17). Publish the protocol spec, schema set, and onboarding documentation as community-draft artifacts, with a public CHANGELOG.

Key deliverables: independent-issuer proof, validation results, published v0.1 artifact set, pilot report.

---

<a id="16-pilot-demonstration-scenarios"></a>
## 16. Pilot Demonstration Scenarios

### Scenario A — The Pledge Badge (Entity Scope, Full Lifecycle)

A reform organization registers as an issuer and issues a *Signed [Reform] Pledge* credential to a county supervisor, offering it for display. The supervisor accepts; the badge renders on their Representative Space with issuer attribution, criteria, and evidence links. A citizen inspects and verifies it. Eighteen months later the organization concludes the commitment was broken and revokes; the badge disappears from display within the bounded window, and the issued → accepted → revoked history remains visible in the activity record. This scenario validates the full mutual-consent lifecycle on the accountability surface.

### Scenario B — The Adversarial Offer

A hostile organization issues a derogatory credential naming a candidate and offers it for display. The offer is recorded and the candidate is notified; the candidate declines. The badge never renders on the candidate's space — but the offer and decline are visible in the credential activity record, so the issuer's behavior is auditable and the candidate's surface is untouched. This scenario validates the structural protection in both directions.

### Scenario C — Participation Recognition (Automation)

A civic hub runs a citizen assembly whose descriptor declares participation credentialing. The process closes; `civic.process.ended` triggers the automation service; participation credentials are offered to each participant's wallet. One participant accepts possession and additionally opts into public display on their dashboard; another accepts possession and keeps it private, later presenting it via selective disclosure to qualify as a facilitator. This scenario validates automation, possession-vs-display, and the disclosure inheritance rules.

### Scenario D — The Independent Issuer

An organization with no prior Civic.Social relationship takes the published schema set and onboarding documentation, and — without bespoke support — issues a schema-compatible organizational-affiliation credential that the badge display components verify and render. This scenario validates the open-layer claim: the schemas, not the platform, are the integration surface.

---

<a id="17-success-and-validation-criteria"></a>
## 17. Success and Validation Criteria

### Deliverable Criteria

The pilot is successful upon production of the artifacts in section 18, headlined by the mutual-consent protocol specification, the published schema set, the reference issuer service, and the badge display components.

### Technical Validation — Measurable

- **Independent issuance.** At least one independent organization issues a compatible civic credential from the published schemas and onboarding documentation alone, and that credential verifies and renders in the pilot's display components without code changes on either side.
- **No display without acceptance.** Across all validation runs, including deliberately adversarial issuance (Scenario B), zero credentials render on any subject's space without a recorded acceptance. This criterion admits no exceptions.
- **Bounded revocation propagation.** A revoked credential disappears from every conforming display surface within the protocol's bounded window (working bound: 24 hours worst-case), measured, not asserted.
- **Auditability of the record.** For every credential in the pilot, the full offer / accept / decline / revoke / remove history is reconstructible from the `civic.credential.*` activity stream via `GET /events` alone.
- **Verify-on-render.** A deliberately tampered or forged credential presented to a display component is refused; a display component hosted on an untrusted page still renders only verifiable badges.
- **Automation correctness.** Every auto-issued participation credential corresponds to a declared issuance policy and a real triggering `civic.process.*` activity; no credential discloses protected process content.
- **Issuer conformance.** Every pilot issuer (reference, automation, independent) satisfies all five issuer requirements of section 10, checked against a published conformance checklist.

### Future Usability Evaluation

Citizen-facing questions — do badge displays change how citizens assess representatives; do participation credentials feel meaningful; what display defaults do citizens actually choose — are observed qualitatively during the pilot and documented as inputs to production design, not gated as pilot success criteria.

---

<a id="18-expected-deliverables"></a>
## 18. Expected Deliverables

- **Mutual-Consent Protocol Specification (v0.1).** The display-state machine, the `civic.credential.*` activity contract, revocation-propagation bounds, and the possession/display boundary. The pilot's primary protocol artifact.
- **Civic Credential Schema Set (v0.1).** Published, versioned schemas for political commitment, organizational affiliation, and civic participation credentials, with documentation and worked examples, issued against the Civic Identity Pilot's credential model.
- **Reference issuer service.** Working implementation of the issue / offer / accept / revoke lifecycle with credential status endpoint and standard activity emission.
- **Badge display components.** Embeddable, verify-on-render components for entity-, individual-, and community-scoped display surfaces, with revocation-aware caching.
- **Participation-credential automation service.** Activity-triggered issuance from `civic.process.*` outcomes under declared issuance policies, delivering to citizen wallets.
- **Issuer onboarding documentation.** The complete path from "organization with a claim to make" to "conforming issuer in the trust registry," including the issuer conformance checklist.
- **Independent-issuer proof.** At least one credential issued by an outside organization from the published artifacts alone, documented as a case study.
- **Trust registry integration and governance note.** Working registry consumption, plus an honest written account of the curated-initially posture, the gatekeeping tension, and the open governance question.
- **Pilot report.** What was built, what was validated, what the mutual-consent model survived and where it strained, and recommendations for v0.2.

---

<a id="19-dependencies-on-the-civic-identity-pilot"></a>
## 19. Dependencies on the Civic Identity Pilot

This pilot is the most directly downstream pilot in the program: it builds on the Civic Identity Pilot's foundation and does not duplicate it. Specifically, it requires the following identity-pilot deliverables (or conforming stand-ins):

- **Credential schema definitions and the credential model** — the base VC issuance model this pilot's civic schemas extend. *Required.*
- **Credential issuance reference implementation** — the issuance infrastructure the reference issuer service builds on. *Required, stubbable:* a minimal VC issuance library honoring the same interfaces suffices for early phases.
- **Credential verification SDK** — the verification tooling the badge display components call for verify-on-render. *Required.*
- **Trust registry** (from the Reference Civic Identity Service design) — the issuer registry this pilot consumes and extends with issuer-requirement entries. *Required, stubbable:* a curated flat registry file honoring the lookup interface suffices for the pilot.
- **Wallet infrastructure** — for citizen possession of participation credentials and selective-disclosure presentation. *Required for Scenario C; stubbable* with the identity pilot's managed-wallet prototype.

Preferred sequencing: Civic Identity ahead or in parallel. Hard dependency: partial — unlike most pilot pairings in the program, this pilot cannot ship its full scope against pure stubs, because "verifiable" is the product. The stub paths above keep early phases unblocked, but Phase 4 validation requires real verification infrastructure.

---

<a id="20-relationship-to-other-civicsocial-pilots"></a>
## 20. Relationship to Other Civic.Social Pilots

**Civic Process Plugin Pilot** — the upstream source of participation records. Its `civic.process.*` activities are this pilot's automation triggers; its processes are where participation credentials are earned. Preferred sequencing: process pilot ahead (it substantially is — the process layer is live). Hard dependency: none for manual credentialing; the automation service needs a live process emitting activities, which already exists.

**Civic Activity Feed Pilot** — the distribution layer that makes recognition visible. This pilot emits `civic.credential.*` activities into the shared stream; the feed pilot decides how they surface in lenses. Preferred sequencing: parallel. Hard dependency: none — activities flow to the existing `GET /events` endpoint regardless.

**Civic Hubs Pilot** — provides the community-scoped display surface and a natural issuer of role credentials. Preferred sequencing: parallel. Hard dependency: none.

**Representative Space work** — provides the entity-scoped accountability surface where the flagship scenarios run. The representative-space service already operating in the ecosystem is the natural host for Scenarios A and B.

---

<a id="21-potential-pilot-partners"></a>
## 21. Potential Pilot Partners

Three categories of partner matter:

- **Issuer partners** — reform and advocacy organizations with real commitments to attest: pledge campaigns, anti-corruption coalitions, bridging organizations, and civic-credentialing networks already building issuer taxonomies. One of these is the natural candidate for the independent-issuer validation (Scenario D).
- **Display partners** — the spaces where badges appear: the representative-space deployment already running in the ecosystem, and any Civic Hub community from the hubs pilot.
- **Standards partners** — the W3C Verifiable Credentials community and the 1EdTech Open Badges 3.0 effort, for schema-alignment conversations; the identity-standards advisors already engaged through the Civic Identity Pilot.

---

<a id="22-risks-and-mitigations"></a>
## 22. Risks and Mitigations

**Risk: adversarial and derogatory labeling.** The most serious risk of any public labeling system: issuers weaponizing credentials to attack subjects. *Mitigation: this is what the mutual-consent display model exists for — no display without acceptance, structurally enforced and validated under deliberate adversarial testing (Scenario B, section 17). Residual risk: the offer record itself could be spammed as a harassment vector; see the spam risk below and the open question on offer-record visibility (section 23).*

**Risk: trust-registry gatekeeping.** Named honestly: whoever curates the trust registry decides which issuers' credentials display by default, and that is real power over civic speech. Curated-by-Civic.Social-initially is the pragmatic starting posture, not a resolved governance design. *Mitigation: keep the registry's scope narrow (default display trust, not issuance rights — anyone can issue schema-compatible VCs, and verifiers can trust issuers directly); publish admission criteria and decisions transparently; document the tension and the may-evolve governance posture in the deliverables rather than claiming it solved; carry the long-term governance question forward as an open question requiring broader ecosystem input.*

**Risk: credential spam.** Low-cost issuance invites volume abuse — issuers blanketing subjects with offers, or automation policies minting meaningless participation credentials until recognition is noise. *Mitigation: offers require registry-listed issuers before they generate subject notifications; issuance-rate observation is part of registry curation; automation requires declared, published issuance policies; and the feed layer's classification keeps credential activities from flooding lenses. The pilot treats observed spam patterns as primary input to v0.2 rate-limiting design.*

**Risk: revocation infrastructure is the weakest link.** A badge that outlives its truth damages the whole layer's credibility, and revocation checking is historically the least-reliably-implemented part of VC systems. *Mitigation: revocation support is a hard issuer requirement, propagation is bounded and measured (not aspirational), and display components fail toward non-display when status cannot be checked within the staleness bound.*

**Risk: recognition drifts toward reputation scoring.** The pressure to aggregate — "just add a score" — is constant, and composite scoring imported prematurely would poison the primary layer's neutrality. *Mitigation: Civic Reputation is named and explicitly deferred (section 7.4); the pilot's scope discipline holds the line at primary credentials, and the pilot report documents the deferral rationale for whoever takes up the reputation question later.*

**Risk: schema fragmentation.** If the civic schemas diverge needlessly from adjacent open vocabularies, "compatible credential" becomes compatible-with-us-only. *Mitigation: the Open Badges 3.0 / VC data-model alignment analysis is a Phase 1 task with a concrete per-schema resolution, and the independent-issuer validation is the empirical test of whether the schemas are genuinely implementable from outside.*

---

<a id="23-open-questions-for-further-design"></a>
## 23. Open Questions for Further Design

**How visible should declined offers be, and for how long?** The offer being part of the public record is essential to issuer auditability — but an aggressive issuer could exploit permanent, prominent decline records as a harassment surface. Where declined-offer visibility sits (fully public, discoverable-but-not-surfaced, time-decayed) is the pilot's most delicate protocol calibration, and adversarial testing should drive it.

**What are the trust registry's admission criteria?** "Curated by Civic.Social initially" defers the governance question but not the operational one: the pilot must actually admit issuers against some published criteria. What those criteria are — and how contested admissions or removals are handled even in the curated phase — needs definition in Phase 1.

**How deep should Open Badges 3.0 alignment go?** Extend OB3 achievement vocabulary directly, map to it, or merely remain data-model compatible? Each schema in the initial set may warrant a different answer.

**What is the right revocation mechanism at scale?** The pilot's per-issuer status endpoint is simple and testable; status lists (bitstring status list style) scale better and leak less traffic metadata to issuers about where badges are being viewed. The pilot should ship the simple mechanism and document the migration path.

**How do contradictory credentials display?** Two issuers can make opposing claims about the same subject — an endorsement and a broken-pledge revocation from rival organizations. The display surfaces need a posture (chronological neutrality, issuer-attribution prominence, no adjudication) that the pilot will draft but real usage must validate.

**Do institutional bodies accept credentials differently than individuals?** Entity-scoped spaces include institutional bodies (a board, a commission) where "the subject accepts" means an authorized officer acting for the body. The authorization semantics for collective acceptance are inherited from the space's own role model, but the edge cases (contested authority, leadership transitions mid-offer) need working through.

**When does the trust registry's governance evolve, and into what?** Deliberately unanswered here. The registry may evolve toward multi-stakeholder governance as the ecosystem grows; the shape, the participants, and the timing need broader ecosystem input than this pilot can convene. The pilot's obligation is to document the question honestly and hand forward the operational experience that makes the eventual answer better informed.

---

<a id="24-conclusion"></a>
## 24. Conclusion

The Civic Credentialing Pilot delivers the recognition layer of the Civic.Social ecosystem: verifiable civic badges built on W3C Verifiable Credentials, issued by an open population of infrastructure-role issuers, displayed on every scope of Civic Space by verification rather than trust, and governed by a mutual-consent protocol that makes public civic labeling safe in both directions — no display without acceptance, no editing of history.

Its deliverables serve the two audiences every pilot in this program serves. For the Civic.Social ecosystem, it activates the layers already built: identity gains public meaning, participation gains durable recognition, activity feeds gain a recognition stream, and the Representative Space gains the third-party accountability record that makes its neutrality asymmetry real. For the broader field, it publishes a schema set any organization can issue against and a consent protocol any credentialing system can adopt — a concrete answer to the question of how public labeling systems avoid becoming either weapons or noise.

The strategic bet is that recognition is infrastructure. Commitments, affiliations, and participation are civic facts, and civic facts deserve the same properties this ecosystem demands of every other layer: verifiable by anyone, controlled by no single party, portable across every surface, and auditable end to end. The pilot is how that bet gets tested at the scale where it matters first — one issuer, one representative, one badge, one revocation, all on the record.

---

*Civic.Social — civic.social | contact@civic.social*
