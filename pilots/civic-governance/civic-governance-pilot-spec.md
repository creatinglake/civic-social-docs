---
status: draft
last-reviewed: 2026-07-12
owners: [adam]
version: 0.1
---

# Civic.Social Governance Pilot

Civic.Social Infrastructure Program

> **Status and governance.** This specification is a **working draft published for community discussion** — not a final standard, and emphatically not a settled constitution. It is also the one document in this program with an unavoidable recursion: it proposes the process by which the Civic.Social specifications will be governed, and it was written by the party that currently governs them. That is a conflict of interest, not a credential. The document is offered as a first draft precisely so that the people it would bind can rewrite it before it binds anyone. Breaking changes between pre-1.0 versions are expected and welcomed.

> **Terminology.** "Governance" is overloaded in this corpus and the overload is load-bearing, so it is worth being exact. **Hub governance** is how a community governs itself inside a Civic Space (moderation, membership, local decision-making) — that belongs to the [Civic Space Specification](../../specs/civic-space-spec.md). **Governance communities** are the core product concept: communities that participate in structured civic processes rather than conversation. **Ecosystem governance** — the subject of this pilot — is how the *specifications, registries, and conformance machinery themselves* are governed: who may change them, by what process, funded how, with what right of appeal and what right of exit. Where this document says "governance" without qualification, it means ecosystem governance.

> **Scope.** This pilot is branded Civic.Social and its immediate object is the civic layer. But the governance *instruments* it produces — the process document, the IPR policy, the non-powers charter, the funding policy, the standing model — are written to be **vertical-agnostic and adoptable by Mosaic Foundation** for any future vertical (section 12). Civic.Social is the first instantiation, not the only intended one. Mosaic's own corporate governance — board composition, bylaws, 501(c)(3) compliance — is a matter for the Foundation's directors and is **out of scope** (section 27).

---

## Table of Contents

### [How to Read This Document](#how-to-read-this-document-1)

### Executive Overview
1. [Executive Summary](#1-executive-summary)
2. [Purpose of the Pilot](#2-purpose-of-the-pilot)
3. [Relationship to the Civic.Social Pilot Program](#3-relationship-to-the-civicsocial-pilot-program)
4. [Strategic Importance — The Concentration Problem](#4-strategic-importance--the-concentration-problem)

### First Principles
5. [Inherited Premises — What the Ecosystem Design Already Decides](#5-inherited-premises)
6. [The Incentive Problem](#6-the-incentive-problem)
7. [Ten Requirements for Governing a Civic Commons](#7-ten-requirements)
8. [Precedents — What We Take and What We Refuse](#8-precedents)

### Governance Architecture
9. [What Must Be Governed — The Inventory](#9-what-must-be-governed)
10. [Steward, Conform, Represent — The Three Functions](#10-steward-conform-represent)
11. [The Four Zones and the Placement Test](#11-the-four-zones-and-the-placement-test)
12. [The Two-Tier Model — Mosaic Instruments, Civic.Social Working Group](#12-the-two-tier-model)
13. [Standing — What Replaces Membership](#13-standing--what-replaces-membership)
14. [Decision-Making, Objection, and Appeal](#14-decision-making-objection-and-appeal)
15. [Specification Lifecycle and Versioning](#15-specification-lifecycle-and-versioning)
16. [Contribution and IPR](#16-contribution-and-ipr)
17. [Registries, Conformance, and the Gate](#17-registries-conformance-and-the-gate)
18. [The Economics of the Commons — How Do We Pay for Neutrality?](#18-the-economics-of-the-commons)
19. [Divestiture — Dated, Not Aspirational](#19-divestiture--dated-not-aspirational)
20. [The Governance Community — Stated Intention, Named Preconditions](#20-the-governance-community)

### Pilot Implementation
21. [Minimum Viable Pilot Scope](#21-minimum-viable-pilot-scope)
22. [Pilot Phases and Timeline](#22-pilot-phases-and-timeline)
23. [Pilot Demonstration Scenarios](#23-pilot-demonstration-scenarios)
24. [Success and Validation Criteria](#24-success-and-validation-criteria)
25. [Expected Deliverables](#25-expected-deliverables)

### Ecosystem and Partnerships
26. [The Governance Questions This Pilot Inherits](#26-the-governance-questions-this-pilot-inherits)
27. [Relationship to Mosaic Foundation](#27-relationship-to-mosaic-foundation)
28. [Potential Pilot Partners](#28-potential-pilot-partners)

### Pilot Plan
29. [Risks and Mitigations](#29-risks-and-mitigations)
30. [Open Questions for Further Design](#30-open-questions-for-further-design)

### Conclusion
31. [Conclusion](#31-conclusion)

---

<a id="how-to-read-this-document-1"></a>
## How to Read This Document

This document is the canonical specification for the Civic.Social Governance Pilot. It is written for multiple audiences.

**Funders and program evaluators** should focus on the Executive Overview (sections 1–4), the incentive analysis (sections 6–7), the economics of the commons (section 18), and the Success and Validation Criteria (section 24). These explain why the governance question is the load-bearing question for this program, what would constitute failure, and what a funder's own money does and does not buy.

**Prospective participants** — implementers, civic organizations, jurisdictions, adjacent standards efforts — should focus on Standing (section 13), Decision-Making (section 14), Contribution and IPR (section 16), and Divestiture (section 19). Together these define what it takes to have a real voice here, what that voice can and cannot be overridden by, and what commitments constrain the current steward.

**Adjacent standards bodies** — W3C, DIF, 1EdTech, the Social Web Foundation, the DDS Working Group, Metagov — should focus on the Four Zones and the Placement Test (section 11), which set out how Civic.Social decides what it standardizes, what it profiles, what it adopts unchanged, and what it hands off.

This pilot specification refers throughout to companion documents:

- **[Civic Space Specification](../../specs/civic-space-spec.md)**, **[Civic Process Specification](../../specs/civic-process-spec.md)**, **[Civic Activity Specification](../../specs/civic-activity-spec.md)**, and **[Civic Identity Specification](../../specs/civic-identity-spec.md)** — the four canonical specifications whose governance this pilot designs.
- **[Contributing](../../CONTRIBUTING.md)** — the current, provisional contribution process, which this pilot is expected to supersede.
- **[Program Overview](../../canon/program-overview.md)** and **[Terminology Glossary](../../canon/terminology.md)** — canonical program framing and vocabulary.

**A note on what this document is not.** It is not the ecosystem design. Who participates in the Civic.Social ecosystem, what roles they occupy, and why an open marketplace of independent providers beats a single consolidated container — those questions are settled elsewhere, and this document treats their answers as **inherited constraints** rather than open questions (section 5). What this document adds is the layer that ecosystem design has so far assumed and never specified: the rules under which the shared standards actually change hands.

---

<a id="1-executive-summary"></a>
## 1. Executive Summary

The Civic.Social Governance Pilot will design, draft, and stress-test the **governance layer** of the ecosystem: the process by which the civic specifications, registries, and conformance machinery are changed, the incentive structure that keeps that process aligned with the common good, and the economic model that pays for it without corrupting it.

Six pilots precede this one. Every one of them defers the same question. The Civic Process Plugin Pilot asks which venue should govern the long-term evolution of its specification and who issues hosting certifications. The Civic Credentialing Pilot names trust-registry curation as "real power over civic speech" and leaves its governance "deliberately unanswered." The Civic Identity Pilot lists three governance questions and concludes that the structures "will likely evolve as the ecosystem grows." Each defers with the same reasoning: *this needs broader ecosystem input than a pilot can convene.*

**This is the pilot that convenes it.**

The pilot proceeds from a specific diagnosis: **governance failures in standards bodies are almost never failures of the process document. They are failures of incentive design.** Process is downstream of who pays, who staffs, who decides, and who is free to leave. A consortium funded by revenue-tiered dues will, over time, produce standards shaped by its largest dues payers — not because anyone is corrupt, but because that is what the incentive gradient does to institutions over a decade. Accordingly, this pilot leads with incentives (sections 6–7), derives its process from them, and treats every governance instrument as falsifiable against them.

Three commitments define the design:

- **Standing is earned, never purchased.** There is no membership tier, because there is no membership. Standing accrues to those who ship conforming implementations, operate spaces, maintain specifications, or are demonstrably affected by them — and the last category is stipended, not merely invited. Money buys no votes; funders are published; grants may not be earmarked to specification outcomes.
- **The steward owns no network and takes no cut.** No rent on interoperability, in any form: registry listing, conformance marks, and certification are free or cost-recovery only. The steward's reference implementations exist to prove the standards, not to corner the market, and the **Steward Non-Powers Charter** enumerates — in a form deliberately hard to amend — the powers the steward disclaims.
- **Exit is real, and divestiture is dated.** The specifications, the test suites, and the decision record are licensed and archived such that any party can reconstitute the whole standard without the steward's cooperation. The credible possibility of a fork is what disciplines a steward that would otherwise face no competition. Where this program currently promises that governance "may evolve toward multi-stakeholder structures," the pilot replaces that with **triggers and dates**.

The pilot's decisive validation is not a document. It is a **decision that goes against Civic.Social's own stated preference and holds** (section 24). Until that has happened at least once, in public, the neutrality claim is unfalsified and therefore worth nothing.

---

<a id="2-purpose-of-the-pilot"></a>
## 2. Purpose of the Pilot

The pilot exists to answer four questions that the rest of the program cannot answer for itself:

1. **Who decides what the civic specifications say** — and how are they prevented from being decided by whoever has the most money, the most staff time, or the founding position?
2. **Where does each specification belong** — authored here, profiled here, adopted unchanged from an upstream body, or handed off entirely to an adjacent one? Section 11 answers this with a repeatable test rather than case-by-case judgment.
3. **How is the commons funded** without the funding mechanism becoming the capture mechanism? (Section 18.)
4. **How does the current steward give power away** on a schedule it does not control? (Sections 19–20.)

The pilot is explicitly *not* an attempt to stand up a governance body in its first phase. Convening a live, multi-stakeholder standards organization requires participants who do not yet exist in sufficient number, and funding that does not yet exist at all. Pretending otherwise would produce governance theater — the most common failure mode in this space, and the one that is hardest to reverse once its vocabulary has been adopted. What the pilot produces is the **constitution and the instruments**, tested against real decisions on real specifications, together with an honest account of the preconditions under which the current steward hands them over.

---

<a id="3-relationship-to-the-civicsocial-pilot-program"></a>
## 3. Relationship to the Civic.Social Pilot Program

The Civic.Social Governance Pilot is the seventh component of the Civic.Social Infrastructure Program, and it is structurally different from the other six. Each of those pilots proves a *layer* of the architecture — spaces, processes, activity, identity, credentialing, the citizen interface. This pilot proves the *conditions under which those layers can be trusted by parties who did not build them.*

Its relationship to each is therefore inverted. The other six pilots hand this one their unresolved governance questions (catalogued in section 26); this one hands back the process by which those questions get answered, and the instruments — standing, objection, appeal, registry policy, conformance policy — that each layer's registry and certification machinery plugs into.

There is a preferred sequencing but no hard gate. The pilot can begin at any time, because its first phase operates on specifications that already exist. It becomes *more* valuable, not less, the longer it is deferred — and correspondingly more expensive, because governance retrofitted onto an ecosystem with entrenched participants is harder than governance designed while the participant list is still short. That asymmetry is the argument for doing it now, while Civic.Social still has the standing to constrain itself voluntarily.

---

<a id="4-strategic-importance--the-concentration-problem"></a>
## 4. Strategic Importance — The Concentration Problem

Civic.Social currently holds four powers simultaneously:

1. It **authors** the four canonical specifications.
2. It **operates** the reference implementations.
3. It **curates** the trust registry that determines whose credentials display by default.
4. It would **issue** the conformance marks and hosting certifications that determine who counts as compliant.

Any one of these is defensible in a young project. Held together, indefinitely, by a single organization, they are precisely the concentration this program's own strategic literature argues against when someone else is doing it. The critique Civic.Social makes of consolidated civic platforms — that they recreate a single point of control, capture value centrally, and limit who can build — applies with equal force to a standards steward that authors the standard, ships the leading implementation, keeps the list of who is trusted, and hands out the certificates.

The honest position is that **Civic.Social is not currently a neutral steward. It is a founder with good intentions**, which is a different and much less durable thing. Good intentions are not an institutional property; they do not survive founder transitions, funding pressure, or a decade of incentive gradient. What survives is structure.

This matters beyond principle, for three practical reasons:

**Governments will not build on infrastructure a single private party controls.** A jurisdiction adopting a civic identity layer is making a multi-decade commitment on behalf of residents. The question its counsel will ask is not "is this good software" but "what happens to us if this organization changes its mind, changes hands, or ceases to exist." A published constitution, an IPR regime, and a fork-ready archive are the only answers that survive that meeting.

**Serious implementers will not invest against a spec they cannot influence.** The civic-tech field is small and has been burned. An organization asked to rebuild its participation tooling against the Civic Process contract will want to know how it gets a change landed, and what happens when Civic.Social disagrees with it.

**Funders are increasingly asking the capture question directly.** The pitch for this program is that a neutral commons succeeds precisely where a single company cannot. That claim is only as good as the mechanism behind it — and at present there is no mechanism, only an assertion.

---

<a id="5-inherited-premises"></a>
## 5. Inherited Premises — What the Ecosystem Design Already Decides

The Civic.Social ecosystem design is settled and published. This pilot does not reopen it. But several of its commitments are not merely aesthetic — each one imposes a hard constraint on what a legitimate governance design can look like. Stating the derivation explicitly is what prevents the governance layer from quietly contradicting the ecosystem layer, which is the usual way these things go wrong.

| Inherited premise | Governance requirement it imposes |
|---|---|
| "Coordinated through **interoperability, not enrollment**." | There can be **no membership to sell**. Standing must derive from contribution and effect, never from a fee. |
| "**No fixed membership list**, single adoption path, or predefined end state." | The process must work for participants who never formally join, and must not privilege a roster. |
| The steward "does not moderate civic spaces, adjudicate disputes between participants, or control who may participate." | These disclaimers must become an **enumerated, hard-to-amend instrument**, not brand copy. |
| The steward is "accountable to the broader democracy-renewal community rather than controlled by a single central entity." | Accountability requires a mechanism: published decisions, standing to object, a real appeal, and a real exit. |
| Reference implementations exist "to prove the standards and show real-world momentum, **not to corner the market**." | The steward must be structurally barred from competing for deployments against ecosystem providers. |
| "The steward **owns no network and takes no cut**." | No revenue may be extracted from the act of being interoperable — no listing fees, no certification rent, no toll on the gate. |
| "Collaboration without consolidation" — value is captured by a marketplace of independent providers. | Enough money must return to the commons to sustain it **without** routing through the commons' own gatekeeping functions. This is the central tension of section 18. |
| Interoperability "should sit *below* any business model, so every model can flourish on top." | Governance of the substrate must be insulated from the commercial interests of anyone operating on top of it — including the steward. |

Two premises deserve emphasis because they do most of the work in what follows.

**There is no membership.** This is the single sharpest structural divergence from the consortium model, and it is not an accident of style — it falls directly out of "interoperability, not enrollment." A body with no membership cannot sell membership tiers, and therefore cannot develop the revenue dependency that tiered membership produces. That closes off the most common capture channel in standards work. It also closes off the most reliable funding channel in standards work, which is the problem section 18 has to solve rather than dodge.

**The steward is a lightweight enabler, not an authority.** W3C never owned a website. The Linux Foundation never shipped a distribution that competed with Red Hat. The Internet Engineering Task Force has no power whatsoever over how anyone uses TCP/IP. The pattern across every successful open-infrastructure steward is that its power over the ecosystem is close to nil, and that this is *the source* of its legitimacy rather than a limitation on it. A steward that can be ignored, forked, or routed around — and is not, because people find it useful — is the only kind worth trusting.

---

<a id="6-the-incentive-problem"></a>
## 6. The Incentive Problem

Standards bodies do not typically fail because their process documents are badly drafted. They fail because the process sits downstream of an incentive structure that pulls in a different direction, and over a long enough period the incentive structure wins. What follows is a taxonomy of the ways that happens, stated as fairly as we can manage — because each of these institutions is, on balance, a success, and the failure modes are the price they paid rather than proof they were wrong.

**F1 — Revenue-tiered membership produces funder-shaped staff.**
W3C membership fees scale with organizational revenue, ranging from roughly US$1,000 to US$77,000 annually. The intent is progressive and defensible: large firms can pay more, so they do, and small organizations get in cheaply. The effect, compounded over years, is that a large share of the consortium's operating revenue arrives from a small number of very large technology companies — and staff, chairs, and process capacity become structurally responsive to them, without anybody deciding to be captured. Critics, including the Electronic Frontier Foundation during the Encrypted Media Extensions dispute, have argued the result is a body that accommodates incumbent business models against the objections of its own participants. **This is the failure mode Civic.Social must design against first, because it is the one that arrives disguised as fiscal responsibility.**

**F2 — But the absence of money is not a solution; it is a different failure.**
Those dues fund a professional staff, a royalty-free patent policy, and the multi-year attention that makes standards actually converge. The Social Web Foundation — the nonprofit stewarding ActivityPub, and the closest analogue to what Civic.Social proposes — reported on the order of **$95,000 in donations** in its 2025 financial report, and has since taken a $200,000 Interledger grant specifically to *research sustainable revenue and governance models for decentralized social platforms*. That is the honest state of the art: the people furthest along this path consider the funding question unsolved and are being funded to study it. **We must offer a better funding model, not merely a purer one, or we will simply have traded capture for neglect.**

**F3 — Founder and steward capture.** The concentration problem of section 4. The party that authors the spec, ships the implementation, curates the registry, and issues the certificates does not need bad intentions to distort the ecosystem; it needs only to keep making locally reasonable decisions.

**F4 — Funder capture.** Philanthropy substitutes cleanly for dues, and carries the same dynamic in different clothes. Foundations have programmatic agendas, reporting requirements, and theories of change. A steward dependent on two grantmakers is as capturable as one dependent on three platform companies — arguably more so, because the dependence is less visible and the funders are presumed benign.

**F5 — Rent on the gate.** Every gatekeeping function is monetizable: the trust registry, the conformance mark, the hosting certification. The moment any of them carries a fee, the steward acquires a financial interest in the gate being narrow, and in there being more gates. This is how neutral stewards become vendors, and it happens gradually enough that no single step looks like the mistake.

**F6 — The reference implementation drifts into a product.** The steward ships reference code to prove the spec. The code is good. Deployments ask for support. Support becomes revenue. Revenue becomes dependence. The steward is now the largest vendor in a market it also regulates, and every ecosystem provider is competing against the body that writes the rules.

**F7 — Merit-as-incumbency: time is dues in disguise.** The Apache and IETF traditions — individual participation, no corporate seats, rough consensus — are genuinely superior on the capture axis. But "whoever shows up" advantages whoever can afford to show up to everything: salaried standards professionals, funded by employers with a commercial stake. A community organization in Floyd County has the same formal rights and a fraction of the hours. Nobody sold them out; they were simply outlasted. **A governance system that does not treat participation cost as a design variable has quietly adopted a means test.**

Each of the ten requirements in section 7 exists to counter one or more of these.

---

<a id="7-ten-requirements"></a>
## 7. Ten Requirements for Governing a Civic Commons

These are the first-principles requirements against which every governance instrument this pilot produces will be tested. They are stated as requirements, not aspirations: a proposed rule that cannot name which requirement it serves is decoration, and a proposed rule that violates one is a defect regardless of how convenient it is.

**R1 — Incentives aligned with the common good.** The structure must make the aligned behavior the *easy* behavior, not the virtuous one. Any rule that relies on the steward's restraint rather than the steward's constraints has failed this requirement. *(Counters F3, F5, F6.)*

**R2 — Inclusive, but efficient.** Legitimacy requires that affected parties can participate; usefulness requires that decisions actually ship. A process that lets any objection stall any change indefinitely is not more democratic — it advantages the incumbent, since the status quo needs no consensus to persist. Every decision path must have a bounded clock and a named decider of last resort. *(Counters F7.)*

**R3 — Standing is earned, never purchased.** No membership tier, no fee-based seat, no pay-to-participate. Voice derives from contribution, implementation, operation, or demonstrable effect. *(Counters F1.)*

**R4 — Money buys no votes.** Funding and decision rights are structurally separated — the Linux Foundation's core insight, and the one thing worth taking from the consortium model wholesale. Funders are published. Grants may not be earmarked to specification outcomes. Nobody's contribution, however large, converts into a technical vote. *(Counters F1, F4.)*

**R5 — No rent on interoperability.** Registry listing, conformance testing, and certification are free or cost-recovery only, permanently. The steward derives no revenue from any gate it controls. *(Counters F5.)*

**R6 — Every gate is narrow, published, appealable, and cheap.** Where a gatekeeping function is genuinely necessary — and the trust registry probably is — its criteria are published in advance, its decisions are published with reasons, its rejections are appealable to a body that does not report to the steward, and its cost of entry is near zero. *(Counters F3, F5.)*

**R7 — Exit must be real.** Specifications, test suites, schemas, and the full decision record are openly licensed and independently archived, such that any party can reconstitute the standard and its conformance machinery without the steward's cooperation or permission. The credible possibility of a fork is the only discipline a monopolist steward actually faces. *(Counters F3, F4, F6.)*

**R8 — Participation cost is a governance variable.** The process is async-first, low-bandwidth, and documented in plain language; and where participation cost would otherwise select for well-resourced incumbents, affected-party participants are **compensated**. Civic.Social, of all organizations, cannot coherently argue that citizens deserve stipends to sit on citizens' assemblies and then run its own governance on unpaid volunteer evenings. *(Counters F7.)*

**R9 — Neutrality must be falsifiable.** The steward publishes the decisions that went against it. A neutrality claim that has never been tested against the steward's own preference is not evidence of neutrality; it is evidence that nothing has been at stake yet. *(Counters F3.)*

**R10 — Sustainable without capture.** Enough money must flow back to the commons to maintain it — the specifications, the test suites, the registries, the editorial labor — through channels that create no claim on its decisions. This requirement is in direct tension with R3, R4, and R5, deliberately. Section 18 is where that tension is worked rather than wished away. *(Counters F2.)*

---

<a id="8-precedents"></a>
## 8. Precedents — What We Take and What We Refuse

The open-infrastructure steward is not a novel institution. It is a well-tested one, and the variation among successful examples is mostly variation in **who pays and what the money buys**.

| Body | Who pays | What money buys | Who decides technically | What we take | What we refuse |
|---|---|---|---|---|---|
| **W3C** | Revenue-tiered member dues (~$1K–$77K) | Membership, Advisory Committee representation | Working Groups; chairs appointed by staff | Royalty-free patent policy; formal objection as a real instrument; the Community Group → Working Group maturity ladder; horizontal review | **Revenue-tiered dues.** The concentration of revenue in a handful of incumbents is the capture channel we most need to close (F1). |
| **IETF** | No membership; meeting fees and sponsorship | Nothing structural | Individuals, by rough consensus, in open working groups | No membership at all; individual (not organizational) participation; "rough consensus and running code" as a bias toward implemented reality | Travel-and-attendance-heavy participation, which is a means test in practice (F7). |
| **Linux Foundation / JDF** | Tiered corporate dues | **Board seats — but not technical votes** | Maintainer-elected Technical Steering Committees | **The separation itself: the money layer and the technical layer do not touch.** This is the single most important structural idea in this table. Also: contributors keep copyright (DCO, no assignment); a legal home and IPR regime available without incorporating a new body. | Corporate-dues-funded boards as the *only* funding channel. We take the firewall, not the till. |
| **Apache** | Donations | Nothing | Project Management Committees; individual merit | Individual merit over corporate seats; "community over code" | Merit-by-tenure, which quietly advantages whoever can be present for years (F7). |
| **CNCF / OpenJS** | LF dues | Board seats | Maintainer-led TSC; sandbox → incubating → graduated | The **maturity ladder** as a way to let immature specs exist without either blessing or killing them | Same as LF. |
| **Decidim / Metadecidim** | Public contracts; association members | Association membership (individual, not corporate) | Metadecidim community: General Assembly, elected Coordination Committee, open Thematic Committees | **The two things this program most needs.** (1) *Divestiture that actually happened*: Barcelona City Council transferred the Decidim trademark and codebase to an independent Decidim Association in 2019 — the founding institution gave the asset away. (2) *A civic project governing itself participatorily*, which is the direct precedent for section 20. | Nothing substantial. This is the closest thing to a model, and it should be studied properly rather than admired from a distance. |
| **AT Protocol / Bluesky** | Venture capital | Ownership of the company | Company-authored specs, now moving to IETF: an ATP working group was chartered in early 2026, and the PLC directory is being spun out as an independent organization | The **staged divestiture path**: build with a company's speed, then hand the substrate to a neutral body. It is a fairer precedent than the "vendor-captured protocol" caricature, and it is roughly the path Civic.Social is on. | Divestiture on the company's own schedule, with no prior commitment. This is exactly what section 19 replaces with dated triggers. |
| **Social Web Foundation** | Philanthropy (~$95K donations, 2025 report; $200K Interledger grant) | Nothing | Specs shepherded to W3C | The honest posture: they treat sustainable funding and governance as an *unsolved research question*, and got funded to work on it. A natural collaborator, not a competitor. | Nothing to refuse — this is a cautionary case about scale, not about design. |
| **NIIS (X-Road), DHIS2 / HISP, MOSIP** | Government and institutional funding | Nothing (public accountability instead) | Small professional steward + implementer network | **Public-infrastructure funding as an alternative to dues** — the most promising channel for a civic commons, and the least explored in civic tech. Also: a certified provider market with the steward taking no cut. | Dependence on one or two national governments, which is its own concentration. |
| **Digital Public Goods Alliance** | Philanthropy and UN backing | Nothing | Standard (9 indicators) + public registry | The **lightest possible lever**: define a standard, certify against it, publish a registry, own nothing. Proof that a steward can be very small and still matter. | — |

**The synthesis.** Take the Linux Foundation's firewall between money and technical votes. Take IETF's refusal of membership. Take W3C's royalty-free patent policy and its formal-objection mechanism. Take CNCF's maturity ladder. Take Decidim's proof that divestiture is a thing institutions actually do. Take the public-infrastructure funding channel that NIIS and DHIS2 demonstrate and that civic tech has barely touched. And refuse the one thing every consortium in this table has in common — **a revenue line that scales with the wealth of the parties whose behavior the standard constrains.**

---

<a id="9-what-must-be-governed"></a>
## 9. What Must Be Governed — The Inventory

No document in this corpus currently enumerates what is actually under governance. That absence is itself a governance failure: you cannot constrain the exercise of powers you have not listed. The inventory below is a first attempt, and **refining it is a named deliverable** (section 25) rather than a settled precondition — the boundary of what Civic.Social should govern is one of the things this pilot is meant to discover.

**Normative specifications** — the four canonical specs ([Space](../../specs/civic-space-spec.md), [Process](../../specs/civic-process-spec.md), [Activity](../../specs/civic-activity-spec.md), [Identity](../../specs/civic-identity-spec.md)) and their companions (the [plugin architecture](../../ecosystem/civic-plugin-architecture-spec.md), the [discovery layer](../../ecosystem/discovery-layer-spec.md), the [authorization model note](../../ecosystem/authorization-model-note.md)).

**Extensible registries** — the activity type registry, the capability class registry, the credential schema set, the plugin trust-tier definitions, the space-scope taxonomy. These are the parts that grow continuously, which makes them the parts where governance is felt daily rather than annually.

**Trust and admission registries** — the credential issuer trust registry. This is the sharpest instance: whoever curates it holds real power over whose civic claims display by default, and the Credentialing Pilot says so plainly.

**Conformance machinery** — the test suites, the conformance criteria, the plugin development harness, and any conformance mark or hosting certification issued against them.

**Reference implementations** — the civic hub, the citizen dashboard, the representative space, the reference issuer service. Governed here only in one respect: the constraints that keep them reference implementations rather than products (F6, R5).

**The corpus itself** — this documentation repository, its canon, its terminology, and the decision record. Whoever controls the terminology controls a surprising amount of the architecture.

**The governance instruments** — recursively, the process document and charter themselves, and the rules for amending them. Amendment thresholds for the Non-Powers Charter must be higher than for ordinary specification changes; a disclaimer that can be revoked by the party that made it is not a disclaimer.

---

<a id="10-steward-conform-represent"></a>
## 10. Steward, Conform, Represent — The Three Functions

The governance role decomposes into three functions with genuinely different characters, and conflating them is how stewards accumulate powers nobody granted them.

**Steward.** Maintain the civic-layer specifications and registries: adjudicate changes, publish versions, keep the record. This is the function most people mean by "governance," and it is the smallest of the three in practice.

**Conform.** Maintain the machinery that makes "compatible" a *checkable claim* rather than a marketing one — test suites, conformance criteria, published results. This is the function that gives an open standard teeth, and the one most often skipped. It is also, per R5, the function most dangerous to monetize.

**Represent.** Carry civic use cases *upstream* into the bodies that own the foundational standards — W3C, DIF, IETF, 1EdTech, the Social Web Foundation — and *sideways* into adjacent civic efforts. This is the function that keeps the ecosystem from drifting into a private dialect. It is proactive work: implementation reports, errata, use-case submissions, and showing up.

**On whether Civic.Social is a standards body.** It is, and the document should say so rather than hedging. The distinction that matters is not *whether* we standardize but *what*:

- **Adopt unchanged.** DIDs, Verifiable Credentials, ActivityPub, OIDC/SIOP, Open Badges 3.0. These are hardened, well-governed, and not ours to re-litigate. We consume them and we do not fork them.
- **Profile.** How those standards are *used together* in a civic ecosystem: the DID login profile across independent spaces, civic credential schemas, identity assurance and disclosure policy composed across hubs, how a space advertises its membership requirements and login capabilities, activity vocabulary extensions over ActivityPub. This is real standardization work — it is simply not standardization *from scratch*. The Mosaic architecture notes already name this exactly, as "Mosaic Profile 0.1."
- **Author.** And here the hedging should stop: **the Civic Space specification is genuinely new.** A scoped host primitive with a sovereign identity-and-data foundation, a plugin contract, a portability contract, and a discovery manifest is not defined by any upstream body. Neither is the Civic Process lifecycle-and-plugin contract. These are original standards, and Civic.Social is their standards body.

The discipline, then, is not to deny that we author standards. It is to author **as few as possible**, push work upstream whenever upstream will take it, and hold the rest under a governance regime good enough that governments and competitors can build on it.

---

<a id="11-the-four-zones-and-the-placement-test"></a>
## 11. The Four Zones and the Placement Test

Where a given specification *belongs* is currently decided case by case, which is why it feels complicated. It should be decided by a published test, applied consistently, with the answers written down and revisited on a schedule.

### 11.1 The Four Zones

**Zone 1 — Upstream** (W3C, DIF, IETF, 1EdTech, Social Web Foundation). Foundational standards we depend on and do not control. Our role: **implementer and advocate.** We contribute use cases, implementation experience, and errata; we never fork. Instruments: a **named liaison per body**, and a public **upstream register** recording what we depend on, at which version, with which open asks outstanding.

**Zone 2 — Adjacent peers** (DDS Working Group, Metagov IDT, DelibTech Network, Decidim). Overlapping civic-layer work, governed elsewhere. Our role: **align or hand off.** Handoff is an explicitly good outcome, not a defeat — if a specification's implementers, expertise, and momentum live in another venue, that venue is where it belongs, and Civic.Social's published posture is already to support whichever venue has the most relevant participants and the lowest coordination overhead. This pilot converts that posture from a sentiment into a procedure.

**Zone 3 — Mosaic Foundation** (the steward). The legal home and the vertical-agnostic instruments: process document, IPR regime, non-powers charter, funding and independence policy, standing model, appeals, divestiture triggers, conformance program design.

**Zone 4 — Civic.Social Working Group** (the vertical). The four civic specifications, their registries, the conformance suite, the reference implementations.

### 11.2 The Placement Test

For any specification, registry, or schema, ask in order:

1. **Is the primitive civic-specific, or general?** If a non-civic system would want it unchanged, it probably belongs upstream, not here.
2. **Does an upstream body already own it, or plausibly want to?** If yes, the default is adopt-or-contribute, not author. The burden of proof is on authoring.
3. **Where are the implementers?** Standards live where the people who implement them live. If the implementers of a thing are mostly in another venue, that venue should hold it, whatever the org chart says.
4. **Whose IPR and licensing regime is safer for adopters?** A government's counsel cares about this more than about elegance.
5. **What is the coherence cost of handing it off?** Some specs are load-bearing for the architecture and cannot be governed at arm's length without the whole stack drifting. Name that cost honestly rather than using it as a pretext to keep everything.

**Default rule: adopt over profile, profile over author, hand off over hoard.** Where the test is ambiguous, the tie goes to the option that gives Civic.Social *less* control. That default is not modesty; it is R1 — making the aligned choice the structurally easy one.

The output is a **placement register**: every governed artifact from section 9, its zone, the reasoning, and the date it was last reviewed. Published, and re-run annually.

---

<a id="12-the-two-tier-model"></a>
## 12. The Two-Tier Model — Mosaic Instruments, Civic.Social Working Group

Civic.Social is Mosaic Foundation's first vertical, not its only intended one, and the technical substrate they share — decentralized identifiers, personal data stores, spaces, hosts, verifiable credentials, federated activity — is the same substrate. Governing them under two separate regimes would duplicate every instrument and fork the vocabulary, producing exactly the fragmentation this program exists to cure.

The resolution is the pattern the Linux Foundation and Apache have both proven: **foundation-level policy, project-level technical governance.**

**Mosaic tier — the instruments.** Vertical-agnostic, written once, inherited by each vertical: the Process Document, the IPR and licensing policy, the **Steward Non-Powers Charter**, the Funding and Independence Policy, the standing model, the appeals path, the divestiture triggers, and the conformance program design.

**Civic.Social tier — a chartered working group.** Applies those instruments to the four civic specifications and their registries. Its own participants, its own domain expertise, its own decisions, its own chairs and editors. It does not re-derive the constitution; it operates under it.

This gives three things at once. The pilot stays **branded Civic.Social**, because the civic layer is where the real decisions and the real implementers are. The instruments are **reusable**, so a future Mosaic vertical inherits a working constitution on day one instead of reinventing it badly. And there is **no terminology fork**, because there is only one rulebook.

It also clarifies what "Mosaic governance" means and does not mean. Mosaic is the *steward of the instruments and the legal home*. It is not a super-working-group that adjudicates civic technical decisions from above. A Mosaic board that could overrule the Civic.Social Working Group on the shape of the Activity spec would have reintroduced, at the foundation layer, precisely the concentration this design exists to prevent.

---

<a id="13-standing--what-replaces-membership"></a>
## 13. Standing — What Replaces Membership

If there is no membership (R3, and the inherited premise of interoperability-not-enrollment), then something else must determine whose voice carries weight. The proposal is that **standing is earned in one of four ways**, and that the fourth is the one that makes this a civic institution rather than a technical one.

**Implementer standing.** You ship a conforming implementation — a space engine, a plugin, an issuer service, a client. Standing follows working code, which is IETF's oldest and best instinct: the people who have to build the thing should have privileged voice about the thing.

**Operator standing.** You run a Civic Space in production — a hub, a dashboard, a representative space — with real participants. Operators bear the consequences of specification decisions that implementers can merely ship around.

**Editorial standing.** You maintain a specification, a registry, or the test suite. Sustained maintenance labor is a contribution, and the people doing it need enough authority to keep the corpus coherent.

**Affected-party standing — stipended.** Communities, jurisdictions, and citizens who are subject to these systems without building them. This is the category every standards body says it wants and none of them get, for a reason that is not mysterious: **showing up is expensive, and unpaid participation selects for the well-resourced** (F7). Civic.Social cannot coherently argue that citizens deserve compensation for serving on assemblies and then staff its own governance with unpaid evenings. Affected-party seats should be **compensated**, and their selection may be **sortition-informed** — the same instrument this program advocates everywhere else.

To be clear about the risk: stipends create a patronage hazard, since the steward pays the people who are supposed to hold it accountable. Mitigations exist — fixed terms, published selection, stipends funded from a ring-fenced pool with the amount set in advance, no stipend contingent on any position taken — and the pilot should test them rather than assume them. But the alternative is not neutrality; the alternative is a governance body composed of people whose employers can spare them, which is a means test with better manners.

**What standing is not.** It is not purchasable, not transferable, not granted by the steward as a favor, and not lost for disagreeing with the steward. Standing is published, with its basis, so that anyone can check the roll and contest it.

---

<a id="14-decision-making-objection-and-appeal"></a>
## 14. Decision-Making, Objection, and Appeal

**Rough consensus, not voting.** The IETF instinct is correct: counting votes invites the question of who gets to vote, which invites the question of who can buy a vote, and the whole capture apparatus follows from there. Consensus is determined by chairs on the substance of the arguments, not the headcount, and it explicitly does not require unanimity.

**Bounded clocks (R2).** Every decision path carries a published clock. Silence is not a veto; an unrebutted objection is not automatically fatal; and a change that has met its consensus bar and its clock ships. A process where any participant can stall any change indefinitely is not a democratic process — it is a status-quo machine, and the status quo is the steward's own draft.

**Formal objection.** W3C's most exportable instrument. A participant with standing may lodge a formal objection: it must be substantive, it must be published, it must receive a published reasoned response, and it cannot be quietly absorbed. Objections are part of the permanent record whether or not they prevail.

**Appeal to a body that does not report to the steward.** Whatever shape it eventually takes — an elected technical committee, a rotating panel, an external ombudsperson — the appeal path's defining property is that Civic.Social does not appoint it, cannot dissolve it, and can lose in front of it. Until that body exists, the appeal path is **the fork** (R7), and the pilot should say so plainly rather than pretending an internal review is an appeal.

**Publication of decisions that went against the steward (R9).** Every decision record notes where Civic.Social's stated preference was overruled. If that list is empty after a year of real activity, the correct inference is not that we were right every time.

---

<a id="15-specification-lifecycle-and-versioning"></a>
## 15. Specification Lifecycle and Versioning

A maturity ladder, in the CNCF and W3C spirit, so that immature work can exist openly without being either blessed or killed:

**Working Draft** → the current state of every civic spec. Published for discussion; breaking changes expected; nobody should build production infrastructure against it without knowing that.

**Community Draft** → discussed in the open with participants who have standing; changes tracked in a public CHANGELOG; a conformance suite exists.

**Candidate** → feature-frozen; requires **independent implementation evidence** — at least two implementations, at least one of them not by Civic.Social — before it can advance. This is the requirement that makes the ladder honest, and it is the one Civic.Social currently could not satisfy for any of its four specs.

**Ratified** → stable; breaking changes require a major version and a migration path; the specification is expected to be safe for a jurisdiction to build against for years.

**Versioning discipline.** Pre-1.0, breaking changes between minor versions are expected and welcomed — the current published posture, which should be preserved rather than tidied away. Post-1.0, breaking changes require a major version, a migration path, and a deprecation window long enough for public institutions to move, which is longer than software teams like.

---

<a id="16-contribution-and-ipr"></a>
## 16. Contribution and IPR

Unglamorous, and the thing a government's counsel reads first.

**Copyright stays with contributors.** The Linux Foundation model — a Developer Certificate of Origin, not a copyright assignment. Pooling contribution does not require surrendering ownership, and requiring assignment is a needless barrier that has sunk more open-infrastructure efforts than any technical decision.

**Documents CC-BY; code permissively licensed; specifications royalty-free.** The specifications must carry an explicit royalty-free patent commitment from contributors — W3C's most valuable single instrument, and the reason its standards are safe to implement. Without it, an implementer is one patent claim away from having built on sand.

**Two-track contribution**, extending what CONTRIBUTING.md already sketches: documentation fixes go straight to pull request; substantive specification changes get discussed in the working group before a PR, because they bind everyone downstream.

**The decision record is a public artifact**, archived independently of any Civic.Social-controlled infrastructure (R7). A standard you cannot reconstruct the reasoning for is a standard you cannot safely fork.

---

<a id="17-registries-conformance-and-the-gate"></a>
## 17. Registries, Conformance, and the Gate

This is where governance stops being abstract, because these are the functions that can tell someone *no*.

**Extensible registries** — activity types, capability classes, credential schemas, trust tiers — should be governed with a **low bar and a published bar**. Registration is not endorsement. The failure mode to avoid is a registry whose curator becomes the arbiter of what civic activity is legitimate; the correct posture is that the registry documents what exists, with a clear namespace convention, and reserves refusal for genuine collisions and abuse.

**The trust registry is the hard case**, and the Credentialing Pilot already says so: whoever curates it holds real power over whose civic claims display by default. Three constraints, from R5 and R6:

- **Narrow scope.** Registry listing gates *default display trust*, never issuance itself. Anyone can issue schema-compatible credentials; verifiers and spaces remain free to trust issuers directly. The registry is a convenience, not a permission system, and every design decision should preserve that.
- **Published criteria, published decisions, real appeal.** Admissions and removals are published with reasons. Removal — the power to un-trust an issuer — is the sharpest instrument in the ecosystem and needs the strongest appeal path.
- **Free, permanently.** No listing fee, ever. The moment the registry has a revenue line, the steward has an interest in the gate (F5).

**Conformance** is the function that gives the standard teeth: machine-runnable test suites, published results, and a conformance mark that means something. It must also be free or cost-recovery only. A certification program that funds the steward is a certification program that will grow, and it will grow in the direction of more mandatory certification.

**Who certifies long-term** — the Process Pilot's open question — gets its answer from R6 and section 19: initially the steward, under published criteria, with a dated commitment to move issuance to a body it does not control. Independent certifiers should be permitted from the outset, not eventually.

---

<a id="18-the-economics-of-the-commons"></a>
## 18. The Economics of the Commons — How Do We Pay for Neutrality?

This is the hardest section in the document, and the one with the least prior art. It is included here — rather than deferred to a finance document — because **funding structure is governance structure**, and a governance design that does not specify its funding model has specified nothing (R10 versus R3, R4, R5).

**The tension, stated plainly.** The ecosystem design requires that value be captured by a marketplace of independent providers, and that the steward own no network and take no cut. But a steward with no revenue maintains nothing: specifications rot, test suites break, registries go stale, and the commons dies of neglect rather than capture. The Social Web Foundation's roughly $95,000 in reported donations is not a criticism of that organization; it is a measurement of how hard this is. **We need enough money flowing back into the commons to sustain it, arriving through channels that create no claim on its decisions.**

### 18.1 Channels, with their capture profiles

| Channel | Capture profile | Posture |
|---|---|---|
| **Revenue-tiered membership dues** | The F1 channel. Reliable, professionalizing, and it bends the institution toward its largest payers over a decade. | **Refused.** This is the structural line this program is drawing, and the reason to be precise about R3 and R4. |
| **Unrestricted philanthropy** | Low per-grant capture, high concentration risk. Two funders is a dependency. | **Primary near-term channel**, with a diversification floor and a published funder register. |
| **Restricted / programmatic grants** | Moderate. Funders have theories of change; earmarking to specification outcomes is the danger. | **Permitted with a firewall**: no grant may condition support on a specification outcome, a registry decision, or a conformance result. Published, including the conditions. |
| **Public-infrastructure funding** (the NIIS, DHIS2, MOSIP channel) | Moderate; risk is dependence on one or two governments. Underexplored in civic tech and structurally well-matched: this *is* public infrastructure. | **The most promising medium-term channel**, and a named pilot investigation. |
| **Sponsorship with explicitly zero governance rights** | Low, *if and only if* the firewall of R4 is real and visible. The LF insight applies: money may buy a logo and a board seat on business matters, never a technical vote. | **Permitted, with the firewall published alongside the sponsor list.** |
| **Certification, registry, or conformance fees** | The F5 channel. Directly monetizes the gate. | **Refused, permanently.** Written into the Non-Powers Charter, where it is hard to amend. |
| **Support and services revenue on reference implementations** | The F6 channel. Reliable revenue, and it turns the steward into the largest vendor in a market it also regulates. | **Refused for the steward.** Explicitly *encouraged* for ecosystem providers — that is the marketplace working as designed. |
| **Fiscal hosting / shared services** (e.g. hosting the standards work at Linux Foundation or the Joint Development Foundation) | Low. Provides legal home and IPR regime without incorporating a new body. | **Actively evaluate.** May be the cheapest credible path to a neutral IPR regime, and it is a real option rather than a hypothetical one. |

### 18.2 The independence rules that follow

- **A diversification floor.** No single funder above a published share of operating revenue. Setting that number honestly — it will be embarrassing at first — is a pilot task, not a pilot assumption.
- **A published funder register**, including amounts, terms, and any conditions.
- **No earmarking to outcomes.** Funding may support the *work*; it may never condition on the *result*.
- **A money/decision firewall**, and the discipline of publishing it: what a funder buys is stated as explicitly as what it does not.
- **A sunset requirement** on restricted grants, so that programmatic dependence has an expiry.

**Deliverable, not conclusion.** This section frames the problem and fixes the constraints. The **Mosaic Economic Framework v0.1** — the actual revenue model, with numbers — is a named deliverable of this pilot (section 25). Committing to a funding model before testing it against real funders would be exactly the false precision this program criticizes elsewhere.

---

<a id="19-divestiture--dated-not-aspirational"></a>
## 19. Divestiture — Dated, Not Aspirational

Across the corpus, the current promise is that governance "may evolve toward more decentralized, multi-stakeholder governance as the ecosystem grows." That sentence appears, in one form or another, in the Identity Pilot, the Credentialing Pilot, and the Process Pilot. It is a sincere intention and it is worth nothing, because it has no trigger, no date, no threshold, and no consequence for missing it. Every founder who has ever retained control has said a version of it.

The pilot replaces it with **triggers**: conditions which, when met, oblige the steward to transfer a specific power to a specific body within a specific window. Candidate triggers, to be fixed in Phase 1:

- **On independent implementation.** When N independent implementations of a specification exist, editorial control of that specification transfers to a working group in which Civic.Social holds no more than a fixed minority of chairs.
- **On registry scale.** When the trust registry exceeds N issuers, or when the first contested removal occurs, admission authority transfers to a multi-stakeholder panel with a published appeal path.
- **On certification demand.** Before any conformance mark becomes load-bearing for procurement, issuance moves to a body Civic.Social does not control.
- **On time.** A backstop date, independent of every other trigger. If none of the above have fired by then, the steward publishes an explanation — and that explanation is itself subject to formal objection.

Decidim is the precedent worth naming: Barcelona City Council, the founding institution, transferred the trademark and the codebase to an independent association. AT Protocol is the live case: a company-authored protocol moving its core specifications into IETF working-group process and spinning its identity directory into an independent organization. Both show that divestiture is a thing institutions actually do. Neither did it because a document told them to — which is the honest limit of this section, and the reason the triggers must be public, so that failing to honor them is visible.

---

<a id="20-the-governance-community"></a>
## 20. The Governance Community — Stated Intention, Named Preconditions

The end state this pilot is aiming at, stated as an intention rather than a plan:

> **The specifications for civic governance should eventually be governed by a governance community running on Civic.Social itself.** Specification proposals as Civic Processes. Standing as verifiable credentials. Decisions as Civic Activities, on the public record, reconstructible from the activity stream. Deliberation on the deliberation plugins. The ecosystem governing the standards that constitute it.

This is not decoration. It would be the most rigorous possible test of the specifications — a governance community is exactly the primitive this program claims to have built — and no other standards body in the world is in a position to attempt it.

**And it cannot be built now.** Civic.Social does not currently have the bandwidth, the funding, or the participants. Building it prematurely would produce a governance community of one, which is worse than an honest interim arrangement because it would *look* like legitimacy. The pilot therefore states the intention and names the preconditions:

- **Participants.** Enough independent implementers, operators, and affected-party representatives that the community is not a mirror.
- **Funding.** Stipends for affected-party standing (R8), because a governance community that only salaried professionals can afford to join is not the thing we are describing.
- **Specification stability.** The specs must be at a stage where decisions are about *evolution* rather than *invention*. Committee-designed foundations are a well-known failure mode; the early architecture needs a small number of people with a coherent vision, and the honest thing to say is that this is currently that phase.
- **A rehearsal.** At least one full specification change run end-to-end through Civic Processes in a low-stakes setting, so that the machinery is known to work before anything consequential depends on it.

Until those are met, governance operates as a **stewarded working draft under a published constitution** — which is to say, exactly what this document is trying to be. The intention above is what the constitution is *for*, and the divestiture triggers of section 19 are the mechanism by which it stops being a promise.

---

<a id="21-minimum-viable-pilot-scope"></a>
## 21. Minimum Viable Pilot Scope

### What the Pilot Will Demonstrate

1. A published constitution — process document, non-powers charter, IPR policy, standing model, funding policy — with each rule traceable to one of the ten requirements.
2. The placement test applied to every artifact in the inventory, with the answers published, including at least one artifact placed *outside* Civic.Social.
3. At least one substantive specification change proposed by an outside party and landed through the process.
4. At least one contested decision that resolves **against** Civic.Social's stated preference, published as such.
5. A fork drill: an independent party reconstitutes a specification and its conformance suite from public artifacts alone, without Civic.Social's cooperation.
6. A first-draft economic framework for the commons, tested against actual funder conversations rather than authored in a vacuum.

### What is In Scope

- The governance instruments (section 25), written vertical-agnostic at the Mosaic tier.
- The Civic.Social Working Group charter, at the vertical tier.
- The placement test, the placement register, and the upstream register.
- Registry and conformance governance policy, including admission criteria and appeal.
- The divestiture triggers.
- The governance-community design note: intention, preconditions, rehearsal design.
- The Mosaic Economic Framework v0.1.

### What is Explicitly Out of Scope

- **Operating a live standards organization.** The pilot writes the constitution and tests it against real decisions; it does not stand up a permanent body with staff, seats, and a budget. That is the follow-on undertaking the pilot is meant to make fundable.
- **Building the governance community.** Named as intention with preconditions (section 20); not built.
- **Mosaic Foundation's corporate governance.** Board composition, bylaws, 501(c)(3) compliance, conflict-of-interest policy. The Foundation's directors own this; the pilot only names the seam (section 27).
- **Reopening the ecosystem design.** Inherited, not re-litigated (section 5).
- **Resolving where every specification ultimately lives.** The pilot delivers the *test*; applying it is ongoing work, and some answers will only be knowable once the adjacent venues have matured.

---

<a id="22-pilot-phases-and-timeline"></a>
## 22. Pilot Phases and Timeline

**Indicative duration: 6–9 months.** Unusually, most of the raw material already exists: six pilot specs full of deferred governance questions, a published ecosystem design, and a strategic literature that has already done the precedent analysis. The work is consolidation, derivation, and — the hard part — testing.

### Phase 1 — Constitution Drafting (2–3 months)

Draft the Mosaic-tier instruments: process document, Steward Non-Powers Charter, IPR and licensing policy, standing model, funding and independence policy, appeals path, divestiture triggers. Fix the numbers the triggers depend on. Complete the inventory (section 9) and the placement register (section 11). Legal review of the IPR regime, including a serious evaluation of Linux Foundation / Joint Development Foundation hosting as an alternative to standing up bespoke machinery.

Key deliverables: constitution draft, inventory, placement register, IPR opinion.

### Phase 2 — Upstream and Adjacent Engagement (2–3 months, overlapping)

Open the liaison relationships: W3C (Verifiable Credentials, Social Web), DIF, 1EdTech, the Social Web Foundation, the DDS Working Group, Metagov's IDT cohort, the DelibTech Network, the Decidim Association. Publish the upstream register. Run the placement test *in public* on at least one contested artifact, and — if the test says so — hand something off. Compare notes with the Social Web Foundation specifically, whose funding-and-governance research overlaps directly with section 18.

Key deliverables: upstream register, liaison relationships, at least one published placement decision that reduces Civic.Social's scope.

### Phase 3 — Stress-Testing the Constitution (2 months)

Run the demonstration scenarios of section 23 against live specifications. Land an outside contributor's substantive change. Run a formal objection to a published resolution. Run a contested registry admission. Run the fork drill. Where the process breaks — and it will — fix the constitution rather than the record.

Key deliverables: validation results, constitution v0.2, published decision record including the decisions that went against the steward.

### Phase 4 — Economics and Publication (1–2 months)

Draft the Mosaic Economic Framework against real funder conversations. Publish the constitution, the registers, and the pilot report. Scope the follow-on: what it would cost, and what would have to be true, to convene the working group for real.

Key deliverables: Mosaic Economic Framework v0.1, published v0.1 governance artifact set, pilot report, follow-on scope.

---

<a id="23-pilot-demonstration-scenarios"></a>
## 23. Pilot Demonstration Scenarios

### Scenario A — The Outside Contributor

An organization with no prior Civic.Social relationship proposes a substantive change to the Civic Activity Specification — a new activity type, or a change to the visibility model. Working only from the published process, they find the venue, gain standing, argue the case, and land the change. This validates that the process is legible and usable from outside, which is the minimum bar for calling it open.

### Scenario B — The Decision That Goes Against Us

A change with real architectural consequence is proposed, and Civic.Social's editors oppose it. It attracts consensus anyway. A formal objection is lodged, published, reasoned, and resolved — **against the steward's stated preference** — and the specification changes accordingly, with the record published as such. *This is the decisive scenario.* Every other artifact in this pilot is preparatory to it. Until it has happened, neutrality is a claim; after it has happened, it is a fact with a URL.

### Scenario C — The Contested Registry Decision

A credential issuer is refused admission to the trust registry, or an admitted issuer is removed. The criteria were published in advance; the decision is published with reasons; the affected party appeals; the appeal is heard by a body that does not report to Civic.Social. This validates the sharpest gate in the ecosystem — and whether "narrow, published, appealable, cheap" survives contact with a party that is genuinely angry.

### Scenario D — The Handoff

The placement test is run in public on a contested artifact — the deliberation-adjacent portions of the process specification are the natural candidate, given the DDS Working Group's overlapping scope. Either the artifact is handed off to the adjacent venue, or a reasoned public decision not to hand it off is published against the test's criteria. Validates that Zone 2 is a real zone and not a diplomatic gesture.

### Scenario E — The Fork Drill

An independent party reconstitutes a full specification, its conformance suite, its schemas, and its decision record from public artifacts alone — no Civic.Social cooperation, no private repositories, no undocumented tribal knowledge. Whatever they cannot reconstruct is, by definition, a lever the steward is still holding. Validates R7, and it is the only exit right that means anything.

---

<a id="24-success-and-validation-criteria"></a>
## 24. Success and Validation Criteria

### Deliverable Criteria

The pilot succeeds on production of the artifacts in section 25, headlined by the constitution, the placement register, the divestiture triggers, and the economic framework.

### Validation — Measurable

- **The legitimacy test.** At least one substantive decision resolves against Civic.Social's stated preference, is implemented, and is published as such (Scenario B). **This criterion admits no substitute.** A pilot that produces a beautiful constitution and never loses an argument has demonstrated nothing.
- **Outside contribution.** At least one substantive specification change originates outside Civic.Social and lands through the published process without bespoke assistance (Scenario A).
- **Scope reduction.** At least one governed artifact is placed outside Civic.Social by the placement test — adopted from upstream, or handed to an adjacent venue — and the decision is published (Scenario D).
- **Forkability.** An independent party reconstitutes a specification and its conformance suite from public artifacts alone (Scenario E). Every gap found is logged and closed.
- **Gate discipline.** Every registry and conformance decision made during the pilot is published with reasons against criteria published *beforehand*; every refusal is appealable; no fee is charged at any gate.
- **Funding transparency.** Every funder is published with amounts and conditions; no grant conditions on a specification outcome; the diversification floor is published, even where it is currently breached.
- **Traceability.** Every rule in the constitution names the requirement (R1–R10) it serves. Rules that serve none are removed.

### Qualitative Evaluation

Whether participants experience the process as legible, fair, and worth their time; whether affected-party stipends attract genuine participation or merely credential the already-engaged; whether adjacent venues treat the placement test as good-faith or as territorial. Observed and documented, not gated.

---

<a id="25-expected-deliverables"></a>
## 25. Expected Deliverables

- **Civic.Social Governance Charter (v0.1).** Purpose, scope, the ten requirements, and the amendment rules — including a higher amendment threshold for the Non-Powers Charter than for ordinary changes.
- **Process Document (v0.1).** Standing, consensus, clocks, formal objection, appeal, the specification maturity ladder, and versioning.
- **Steward Non-Powers Charter.** The enumerated, deliberately hard-to-amend list of powers the steward disclaims: will not moderate civic spaces, will not adjudicate disputes between participants, will not gate who may participate in the ecosystem, will not charge for interoperability at any gate, will not compete for deployments against ecosystem providers. The pilot's most quotable artifact, and its most falsifiable.
- **IPR and Licensing Policy.** DCO over CLA; CC-BY documents; permissive code; royalty-free patent commitment on specifications.
- **Standing and Participation Model.** The four standing paths, the stipend design for affected parties, the published standing roll, and the anti-patronage safeguards.
- **Funding and Independence Policy.** Diversification floor, funder register, no-earmarking rule, money/decision firewall, restricted-grant sunset.
- **Mosaic Economic Framework (v0.1).** How the commons is funded without corrupting it: channels, capture profiles, and a proposed model tested against real funder conversations.
- **Placement Test and Placement Register.** The decision procedure, plus every governed artifact's zone, reasoning, and review date.
- **Upstream Register and liaison relationships.** What we depend on, at what version, with what open asks — and a named human per upstream body.
- **Registry and Conformance Governance Policy.** Admission criteria, publication requirements, appeal path, permanent no-fee commitment, and the path to independent certifiers.
- **Divestiture Triggers.** Dated and thresholded commitments transferring specific powers to specific bodies.
- **Governance Community Design Note.** The intention, the preconditions, and the rehearsal design (section 20).
- **Governance sections merged into the four canonical specifications**, replacing the current "may evolve as the ecosystem grows" language with references to the constitution.
- **Incentive Audit (v1).** For every governance function: who benefits, who could capture it, and what prevents them. Repeated annually, not once.
- **Pilot report.** What the constitution survived, where it broke, and what the steward learned about giving power away while it still had a choice.

---

<a id="26-the-governance-questions-this-pilot-inherits"></a>
## 26. The Governance Questions This Pilot Inherits

Every deferral in the program, and where it is answered.

| Source | Deferred question | Answered in |
|---|---|---|
| [Civic Process Pilot](../civic-process/civic-process-pilot-spec.md) §34 | Which community-group venue should govern the long-term evolution of the plugin specification? | §11 (placement test), §12 (two-tier model) |
| [Civic Process Pilot](../civic-process/civic-process-pilot-spec.md) §34 | Who issues hosting certifications long-term? | §17, §19 |
| [Civic Credentialing Pilot](../civic-credentialing/civic-credentialing-pilot-spec.md) §10, §22 | Who curates the trust registry, and what constrains that gatekeeping power? | §17, §6 (F5), §7 (R5, R6) |
| [Civic Credentialing Pilot](../civic-credentialing/civic-credentialing-pilot-spec.md) §23 | What are the trust registry's admission criteria? | §17 |
| [Civic Credentialing Pilot](../civic-credentialing/civic-credentialing-pilot-spec.md) §23 | When does registry governance evolve, and into what? | §19 (triggers, not "may evolve") |
| [Civic Identity Pilot](../civic-identity/civic-identity-pilot-spec.md) §30 | Who can issue credentials; how are schemas defined; how are trust registries maintained? | §17, §14 |
| [Civic Space Specification](../../specs/civic-space-spec.md) §11 | Specification stewardship and versioning. | §15, §16 |
| [CONTRIBUTING.md](../../CONTRIBUTING.md) | What is the long-term governance venue for each spec? | §11, §12 |
| Positioning material | "Stewarded as a nonprofit public-benefit entity supported by a coalition." What does that mean structurally? | §12, §13, §18 |
| Program-wide | Where does the money come from, and what does it buy? | §18 |

---

<a id="27-relationship-to-mosaic-foundation"></a>
## 27. Relationship to Mosaic Foundation

Mosaic Foundation is the nonprofit whose mission is open, interoperable public digital infrastructure built on open standards. Civic.Social is its first vertical, not its whole mission. The technical substrate is shared, which is why the governance instruments are written at the Mosaic tier (section 12).

The seam is worth stating precisely, because conflating these is how foundations accidentally become authorities:

**Mosaic's board governs Mosaic** — incorporation, bylaws, conflict-of-interest policy, 501(c)(3) compliance, budget, officers. That work belongs to the directors and is out of scope for this pilot. This document depends on the Foundation existing and being legitimate; it does not tell the Foundation how to run itself.

**Mosaic stewards the instruments** — the constitution, the IPR regime, the non-powers charter, the funding policy. Vertical-agnostic, adopted by each vertical.

**The Civic.Social Working Group governs the civic specifications** — under those instruments, with its own participants and chairs.

**And the boundary that matters most:** a Mosaic board with the power to overrule the working group on the substance of a specification would have reintroduced, at the foundation layer, exactly the concentration this design exists to prevent. The Foundation holds the constitution. It does not hold the pen.

---

<a id="28-potential-pilot-partners"></a>
## 28. Potential Pilot Partners

**Upstream standards bodies** — W3C (Verifiable Credentials, Social Web), DIF, 1EdTech (Open Badges 3.0), and the IETF, for liaison relationships and the upstream register.

**Adjacent civic-layer venues** — the DDS Working Group, Metagov's Interoperable Deliberative Tools cohort, the DelibTech Network (DemocracyNext and the AI & Democracy Foundation), and the **Decidim Association**, whose Metadecidim community is the closest existing model for both the divestiture and the governance-community questions and should be studied properly rather than cited from a distance.

**The Social Web Foundation** — the most direct peer, working the identical funding-and-governance problem from the ActivityPub side, and currently grant-funded to research it. Collaboration here is more valuable than a liaison.

**Institutional stewards** — NIIS (X-Road), the HISP centre at the University of Oslo (DHIS2), MOSIP, and the Digital Public Goods Alliance, for the public-infrastructure funding channel that section 18 identifies as the most promising and least explored.

**Governance and legal partners** — the Linux Foundation / Joint Development Foundation, for a serious evaluation of hosted standards infrastructure; open-source legal counsel for the IPR regime; and academic governance researchers (Metagov's institutional-design community among them) as critical readers of the constitution.

**Funders** — specifically those willing to fund governance work, which is a small population, because governance produces no demo. That difficulty is itself a finding, and section 18 should report it honestly.

---

<a id="29-risks-and-mitigations"></a>
## 29. Risks and Mitigations

**Risk: the pilot is run by the party it is meant to constrain.** The foundational conflict. Civic.Social is writing the rules that will bind Civic.Social, at a moment when no other party has the standing to object. *Mitigation: none is sufficient, and the document should not pretend otherwise. The partial mitigations are structural — publish the draft early, invite adversarial review from the parties it disadvantages, and make the pilot's success criterion an outcome the steward cannot fake (Scenario B, section 23). The best available answer is that the constraints are written now, voluntarily, while the steward still finds it easy to be generous.*

**Risk: governance theater.** Producing a beautiful constitution that no one operates under, and calling it legitimacy. The most common outcome in this field, and the hardest to reverse once the vocabulary is adopted. *Mitigation: every success criterion in section 24 requires an outcome rather than a document, and the decisive one requires losing an argument. If the pilot ends with no outside contribution, no scope reduction, and no decision against the steward, it has failed — regardless of how good the artifacts look.*

**Risk: process overhead kills the thing it protects.** Standards process is heavy, the team is small, and a young architecture needs the ability to change fast. Premature process is how promising specs die of committee. *Mitigation: R2's bounded clocks; the maturity ladder, which keeps early specs explicitly in a phase where the steward may move quickly; and staging the constitution so the heaviest requirements attach at Candidate rather than Working Draft. Section 20 says plainly that the current phase is one where a small group with a coherent vision is the right architecture — the honesty about that is what earns the right to say it.*

**Risk: legitimacy without contributors.** Standing models, stipends, and appeal panels presuppose participants. Right now there are very few. A governance body of one is not neutral; it is a mirror with better paperwork. *Mitigation: sequence legitimacy against the implementer population — the divestiture triggers are threshold-based rather than date-based for exactly this reason. And do not simulate a community: an honest stewarded draft is more credible than a hollow assembly.*

**Risk: funder capture (F4).** The near-term funding channel is philanthropy, and the population of funders willing to fund governance is small — which means concentration is likely by default. *Mitigation: the diversification floor, the published funder register, the no-earmarking rule, and the firewall — all published even while the floor is being breached, because a breached floor that is visible is a different thing from one that is hidden.*

**Risk: the reference implementation becomes a product (F6).** The steward ships good code; deployments want support; support becomes revenue; the steward becomes the market's largest vendor and its regulator. *Mitigation: the Non-Powers Charter bars the steward from competing for deployments against ecosystem providers, and section 18 refuses support-and-services revenue for the steward specifically while encouraging it for everyone else. This is a real cost — it forecloses the most natural revenue line available — and it is the price of the neutrality claim.*

**Risk: venue fragmentation.** Specs scattered across a Civic.Social WG, the DDS WG, Metagov, W3C, and a Mosaic tier, with no one able to say where anything lives. *Mitigation: the placement register makes the map explicit and public; the placement test makes the reasoning repeatable; and the two-tier model ensures a single rulebook rather than one per venue. Fragmentation of venues is acceptable. Fragmentation of rules is not.*

**Risk: premature or performative decentralization.** Handing decisions to a body that does not yet have the expertise or the participation, and calling it democracy. Committee-designed architecture is a well-documented failure mode. *Mitigation: threshold triggers rather than calendar triggers for the technical decisions; and an explicit statement (section 20) that the current phase requires a small group with a coherent vision — a claim the constitution makes falsifiable by attaching an expiry to it.*

**Risk: the constitution is ignored.** Written, published, and quietly worked around when it becomes inconvenient. *Mitigation: the fork drill (Scenario E) and the published decision record are the only real enforcement. A steward who can be forked and archived cannot quietly rewrite history — and that, rather than any internal committee, is what makes the rules bind.*

---

<a id="30-open-questions-for-further-design"></a>
## 30. Open Questions for Further Design

**Where should the standards actually live?** Deliberately unanswered. Convene a Civic.Social Working Group; fold specs into an adjacent venue; or host the standards work at the Linux Foundation / Joint Development Foundation and inherit a tested IPR regime without incorporating anything new. The pilot will apply its own placement test to itself, and Civic.Social's posture — stated in CONTRIBUTING.md and preserved here — is to support whichever venue has the most relevant participants and the lowest coordination overhead, *including handing over primary stewardship*.

**What is the diversification floor, in numbers?** Every honest answer will be embarrassing in year one. Publishing it anyway is the point.

**Is sortition legitimate for standards decisions?** It is legitimate for civic decisions — this program argues so at length. Whether a randomly-selected affected-party panel can meaningfully participate in decisions about credential schema versioning is a different and harder question. The answer may be that sortition suits *policy* questions (what should the trust registry refuse?) and not *technical* ones (how should status lists be encoded), which would imply a two-track standing model that this draft does not yet have.

**What is the standing threshold?** "Ship a conforming implementation" is clean until someone ships a trivial one to gain a seat. Every anti-gaming rule is also a gatekeeping rule, and the tension is real.

**How do stipends avoid becoming patronage?** The steward paying its own critics is a genuine hazard, mitigated but not eliminated by fixed terms, published selection, and ring-fenced funds. Worth testing at small scale before it is load-bearing.

**Do public institutions need a distinct standing class?** A jurisdiction adopting the identity layer carries obligations no other participant carries — statutory duties, records law, the franchise itself. Whether that warrants a special seat, or whether special seats are the beginning of the end, is unresolved.

**What happens in a hostile fork?** R7 makes forking possible by design, which means it will eventually be used adversarially — a fork of the civic specs by a party with incompatible values, carrying the vocabulary and the credibility with it. The trademark is the only real instrument here, and the pilot should think about it before it is needed rather than after.

**What is the actual boundary of what Civic.Social should govern?** The inventory in section 9 is a first attempt, not a settled scope. Establishing that boundary is one of the things the pilot is for, and it should be expected to move.

---

<a id="31-conclusion"></a>
## 31. Conclusion

Six pilots in this program build the layers of an open civic ecosystem: spaces, processes, activity, identity, credentials, and the citizen's interface onto all of it. Each one, at the point where it touches the question of who decides, defers — to a venue not yet chosen, a registry authority not yet constituted, a governance structure that "may evolve as the ecosystem grows." Those deferrals were honest. They are also, collectively, the largest unaddressed risk in the program, because an open standard that one organization controls is not an open standard. It is a platform with better manners.

This pilot proceeds from the conviction that the failure would not come from bad intentions. It would come from incentives: from a revenue line that scales with the wealth of the parties the standard constrains, from a gate that becomes profitable to keep narrow, from a reference implementation that quietly becomes a product, from a room where only the people whose employers can spare them ever show up. Every one of those failures has happened to institutions founded by people who meant well. The remedy is not better intentions; it is structure that makes the aligned choice the easy one — standing that cannot be bought, gates that cannot be monetized, an exit that cannot be closed, and a public record of every time the steward lost.

The end state is worth naming plainly, because it is the reason to do this at all: **the standards for civic governance, governed by a governance community running on Civic.Social itself.** Proposals as processes. Standing as credentials. Decisions on the public activity record, reconstructible by anyone. No other standards organization on earth is in a position to attempt that, and Civic.Social cannot attempt it yet — it lacks the participants, the funding, and the stability of specification that would make it real rather than theatrical. Saying so is not a retreat. It is the first honest precondition of ever getting there.

What the pilot delivers in the meantime is a constitution written by the party it constrains, at the moment when that party still finds it easy to be generous — and a set of triggers that will make it costly, publicly and by name, to change its mind later. That is the whole bet: that the best time for a steward to give power away is before anyone has the standing to take it.

---

*Civic.Social — civic.social | contact@civic.social*
