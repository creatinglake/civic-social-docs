---
status: draft
last-reviewed: 2026-07-03
owners: [adam]
version: 0.1
---

# Civic Activity Feed & Discovery Pilot

Civic.Social Infrastructure Program

> **Status and governance.** This specification is a **working draft published for community discussion** — not a final standard. It builds directly on the [Civic Activity Specification](../../ecosystem/civic-activity-spec.md) and the [Discovery Layer Specification](../../ecosystem/discovery-layer-spec.md), and follows the same governance posture as the other Civic.Social pilot specifications: breaking changes between pre-1.0 versions are expected and welcomed, and the v0.1 → v1.0 evolution happens openly, in public, with the people who plan to build on it.

---

## Table of Contents

### [How to Read This Document](#how-to-read-this-document-1)

### Executive Overview
1. [Executive Summary](#1-executive-summary)
2. [Purpose of the Pilot](#2-purpose-of-the-pilot)
3. [Relationship to the Civic.Social Pilot Program](#3-relationship-to-the-civicsocial-pilot-program)
4. [Strategic Importance](#4-strategic-importance)

### Strategic Context
5. [What is the Civic Activity Feed](#5-what-is-the-civic-activity-feed)
6. [Discovery is an Extension of the Feed](#6-discovery-is-an-extension-of-the-feed)
7. [Architectural Role — Proving the Activity Layer](#7-architectural-role--proving-the-activity-layer)

### Feed Architecture
8. [One Engine, Many Lenses](#8-one-engine-many-lenses)
9. [The Five Lenses](#9-the-five-lenses)
10. [The Shared Feed Classifier](#10-the-shared-feed-classifier)
11. [Envelope Validation at Ingestion](#11-envelope-validation-at-ingestion)
12. [The Embeddable Feed Widget](#12-the-embeddable-feed-widget)
13. [Digest and Notification Delivery](#13-digest-and-notification-delivery)
14. [Subscriptions, Personalization, and the PDS Seam](#14-subscriptions-personalization-and-the-pds-seam)

### Discovery Architecture
15. [The Reference Indexer](#15-the-reference-indexer)
16. [Space Identity and the Migration Re-Binding Protocol](#16-space-identity-and-the-migration-re-binding-protocol)
17. [Ranking Neutrality as a Design Principle](#17-ranking-neutrality-as-a-design-principle)

### Pilot Implementation
18. [Minimum Viable Pilot Scope](#18-minimum-viable-pilot-scope)
19. [Pilot Phases and Timeline](#19-pilot-phases-and-timeline)
20. [Pilot Demonstration Scenarios](#20-pilot-demonstration-scenarios)
21. [Success and Validation Criteria](#21-success-and-validation-criteria)
22. [Expected Deliverables](#22-expected-deliverables)

### Ecosystem and Partnerships
23. [Relationship to Other Civic.Social Pilots](#23-relationship-to-other-civicsocial-pilots)
24. [Estimated Development Effort and Team Roles](#24-estimated-development-effort-and-team-roles)
25. [Potential Pilot Partners](#25-potential-pilot-partners)
26. [Estimated Budget](#26-estimated-budget)

### Pilot Plan
27. [Risks and Mitigations](#27-risks-and-mitigations)
28. [Open Questions for Further Design](#28-open-questions-for-further-design)

### Conclusion and Future Work
29. [Conclusion](#29-conclusion)

---

<a id="how-to-read-this-document-1"></a>
## How to Read This Document

This document is the canonical specification for the Civic Activity Feed & Discovery Pilot. It is written for multiple audiences, and not every reader needs to read every section.

**Funders and program evaluators** should focus on the Executive Overview (sections 1–4), the Success and Validation Criteria (section 21), and the Pilot Plan (sections 24–28). These sections explain why a shared civic activity layer matters, how the pilot will be measured, and what it will cost.

**Technical implementers** — feed consumers, indexer builders, embed integrators — should focus on the Feed Architecture (sections 8–14), the Discovery Architecture (sections 15–17), and the Expected Deliverables (section 22). These sections define the components the pilot ships and the contracts third parties integrate against.

This pilot specification refers throughout to the canonical documents in the Civic.Social ecosystem folder:

- **[Civic Activity Specification](../../ecosystem/civic-activity-spec.md)** — the protocol-level definition of a Civic Activity: envelope schema, activity types, visibility, transport, and the ActivityStreams bridge roadmap. Everything this pilot aggregates is a Civic Activity.
- **[Discovery Layer Specification](../../ecosystem/discovery-layer-spec.md)** — the hybrid, pluralistic discovery model: federated publication, plural indexing, manifests, and the migration re-binding protocol. This pilot ships the reference implementation.
- **[Civic Space Specification](../../ecosystem/civic-space-spec.md)** — the scoped host contract of the spaces whose activity the feed aggregates (§1.3–1.6) and the Space API Profile whose endpoints the feed consumes (§7).

The pilot does not redefine what those documents already cover. It implements them, validates them through working infrastructure, and surfaces what they need to make stronger.

**A note on wire vocabulary.** The protocol-level term throughout this document is **Civic Activity**. Per the Civic Activity Specification §14, the ratified v0.1 wire format keeps the field name `event_type` and the transport endpoint `GET /events`; the v0.2 rename (`activity_type`, `GET /activities`) is coordinated with the ActivityStreams bridge work. This pilot builds against the v0.1 wire format and treats the rename as a tracked migration, not a blocker.

---

<a id="1-executive-summary"></a>
## 1. Executive Summary

The Civic Activity Feed & Discovery Pilot will design, build, and validate the distribution and navigation layer of the Civic.Social ecosystem: **an inbox for civic life** — a single shared stream of civic activity, refracted into lenses a citizen can filter, follow, and act from.

Civic participation today is episodic because civic information flow is fragmented. Citizens miss votes they were eligible for, comment periods they cared about, and meetings in their own county, because every civic tool announces itself through its own email list, its own site, its own notification silo. Organizations that run participation processes have no shared distribution infrastructure, so every process starts its audience from zero. The ecosystem cannot generate network effects without a shared activity layer — and no individual platform can build that layer for itself.

The pilot delivers that layer as three tightly-coupled pieces of infrastructure:

- **The reference Activity Feed engine** — a single feed component that ingests Civic Activities from any conformant Civic Space, validates them against the Civic Activity Specification at the door, classifies them once through a shared classifier, and serves them through **five lenses**: Inbox, Notifications, Discovery, Space View, and Embed. One engine, many lenses — the engine is a reusable Component in the five-layer architecture; the lenses are how every interface consumes it.
- **The embeddable feed widget** — the Activity Feed as a standalone surface a third party can drop into any web page. A city portal, a newsroom, or a nonprofit campaign page can host a live civic activity stream with zero custom code and none of the rest of the stack. This is the pilot's most shippable artifact and its clearest demonstration that the activity layer is infrastructure, not a destination.
- **The reference discovery indexer** — the first working implementation of the Discovery Layer Specification: manifest and feed ingestion from `/.well-known/civic.json`, normalization into a searchable index keyed by space DID, query endpoints by jurisdiction, type, and category, and the space-migration re-binding protocol that lets a community move its hub without vanishing from the map. Ranking is **chronological plus geographic proximity only** — a deliberate design principle, not a missing feature.

The pilot also productizes two things the reference implementation has already proven in the field: the **email digest** (running today in the Civic.Social Hub) becomes a lens-driven digest/notification delivery channel, and the hard-won lesson that feed-worthiness logic drifts when it is copied — the reference implementation accumulated multiple diverging copies of "should this activity appear here?" — becomes the **single shared classifier** that every lens, digest, and notification renderer consumes. Finally, the pilot gives the ecosystem its first working conformance pressure: because the feed validates the Civic Activity envelope at ingestion and rejects or flags nonconforming activities, the feed is the ecosystem's first serious *consumer* — and therefore its de facto conformance checker. Publishers learn whether their activities are spec-compliant the moment they try to be seen.

### Why This Matters Now

The Civic.Social reference hub is live, emitting spec-aligned activities, and running its first community pilot. The Civic Process Plugin Pilot is standardizing what gets emitted; the Civic Hubs Pilot is standardizing who emits it. What is missing is the layer that makes any of it *visible*: today, activity from a hub reaches only the people already inside that hub. The activity layer is the difference between a set of conformant-but-isolated civic tools and an ecosystem. This pilot is deliberately scoped so that its first deliverable — the embeddable widget against a live hub — is useful to a real city portal within months, while the indexer and lens architecture build toward the full federation vision underneath.

---

<a id="2-purpose-of-the-pilot"></a>
## 2. Purpose of the Pilot

The Civic Activity Feed & Discovery Pilot proves the **activity layer** of the Civic.Social ecosystem: that Civic Activities emitted by independent Civic Spaces can be aggregated, validated, classified, and surfaced to citizens through a single reusable engine — and that discovery of new spaces, processes, and issuers emerges from that same stream rather than requiring a separate system.

It is the work that turns the [Civic Activity Specification](../../ecosystem/civic-activity-spec.md) and the [Discovery Layer Specification](../../ecosystem/discovery-layer-spec.md) into shippable infrastructure: a reference feed engine with five lenses, an embeddable widget, a reference indexer, and the documentation that lets any third party consume the stream or reproduce the index. The pilot is explicitly *not* a social feed product. It builds no engagement-ranking algorithm, no recommendation model, and no attention-optimization loop. It is distribution infrastructure for civic participation, and its design principles (section 17) commit it to staying that.

---

<a id="3-relationship-to-the-civicsocial-pilot-program"></a>
## 3. Relationship to the Civic.Social Pilot Program

The Civic Activity Feed & Discovery Pilot is one component of the broader Civic.Social Infrastructure Program. It operates alongside several complementary pilot initiatives.

**Civic Hubs Pilot** — Civic Hubs (community-scoped Civic Spaces) are the primary *publishers* into the activity layer: they emit Civic Activities through their single emission path and publish the discovery manifests the indexer ingests. This pilot is the primary consumer of what the Civic Hubs Pilot standardizes.

**Citizen Dashboard** — the individual-scoped Civic Space is the primary *consuming interface* for the feed engine: the Inbox, Notifications, and Discovery lenses are the dashboard's core surfaces. The dashboard renders lenses; this pilot builds the engine behind them.

**Civic Identity Pilot** — personalization (subscriptions, follows, read state, notification preferences) belongs in the citizen's Personal Data Store, defined by the Civic Identity Specification. This pilot defines the PDS-shaped seam and ships an interim store behind it (section 14). The PDS itself is not yet built — this is a named coordination risk (section 27).

**Civic Process Plugin Pilot** — processes are the dominant source of activity. The plugin framework's activity-emission seam is what guarantees the feed a well-formed stream; this pilot's envelope validation is what holds emitters to it.

**Civic Credentialing & Profiles Pilot** — credential issuance activities ("a credential was issued to you") are a canonical Notifications-lens input, and credential issuers are a discoverable entity class in the index.

These pilots can each be run independently — preferred sequencing is described in section 23, but no pilot is a hard prerequisite for another. This pilot can be funded and executed on its own track: the reference hub already emits a live, spec-aligned activity stream to build against.

---

<a id="4-strategic-importance"></a>
## 4. Strategic Importance

The civic technology ecosystem is fragmented not only at the level of tools and identity, but at the level of **information flow**. Citizens miss participation opportunities because there is no shared channel through which opportunities travel; they rely on email lists, word of mouth, and whichever platforms they already happen to check. Organizations running participation processes each rebuild their own announcement infrastructure and each start from an audience of zero. This is the layer where network effects live or die: identity portability and process interoperability are necessary, but they compound only when participation in one place makes activity *visible* elsewhere — when a resident who joined their county hub for a budget vote discovers, through the same stream, the school board consultation and the regional transit assembly. Without a shared activity layer, every conformant tool is still an island.

The strategic choice this pilot makes is that the activity layer must be **infrastructure, not a platform**. Three commitments follow:

1. **Aggregation over centralization.** The feed aggregates activity from independent, sovereign Civic Spaces; it never becomes the place activity has to originate. Publication is federated and permissionless per the Discovery Layer Specification.
2. **Plural indexing over gatekeeping.** The reference indexer is one indexer among potentially many. Anyone can reproduce the index from public feeds and manifests — and the pilot's success criteria (section 21) make that reproducibility a measured claim, not a slogan.
3. **Neutral ranking over engagement optimization.** MVP ranking is chronological and geographic only. This is a design principle with governance weight (section 17), not a deferred feature.

---

<a id="5-what-is-the-civic-activity-feed"></a>
## 5. What is the Civic Activity Feed

The **Civic Activity Feed** is the aggregation and distribution layer that surfaces Civic Activities to citizens — one shared stream refracted into many lenses. It is best understood as **an inbox for civic life**: the single place where everything a citizen follows, everything directed at them, and everything worth discovering arrives as structured, actionable items.

Every item in the feed is a **Civic Activity** conforming to the Civic Activity Specification: a JSON object with a canonical `event_type`, a timestamp, an actor, a jurisdiction, an `action_url` a citizen can follow to participate, source attribution identifying the emitting Civic Space, a namespaced `data` payload, and a visibility class. The feed defines no second item shape — display fields (title, summary, process type, time bounds) are derived from the envelope, per the Discovery Layer Specification §7.1.B.

The feed aggregates activity from Civic Spaces of every scope — community-scoped Civic Hubs, entity-scoped Representative Spaces, individual-scoped Citizen Dashboards where their activity is public — plus non-space publishers (organizations, process providers, government systems) that publish conformant feeds and manifests. It is consumed by every interface in the ecosystem: dashboards, hubs, third-party embeds, and downstream tools reading the raw stream over the API. What the feed is *not*: it is not a content platform (it hosts no content of its own — every item links back to its source via `action_url`), not a moderation authority (visibility and disclosure are set by the emitting process and space, per Civic Activity Specification §7), and not a metrics engine (any engagement or participation metrics are computed by originating systems and merely displayed).

---

<a id="6-discovery-is-an-extension-of-the-feed"></a>
## 6. Discovery is an Extension of the Feed

The key architectural insight, carried over from the Discovery Layer Specification: **discovery is not a separate system — it is an extension of the feed.** The feed already contains everything discovery needs: every Civic Activity carries a jurisdiction, a source space, a process type, and a timestamp. Discovery is what happens when that stream is made navigable — indexed, filtered, searched, and browsed by someone who *doesn't yet follow* the things producing it. A citizen's Inbox answers "what's happening in the things I follow?"; Discovery answers "what exists that I should be following?" Both are views over the same stream.

This is why the pilot delivers the feed and the discovery layer together rather than as two pilots. Separating them would create an artificial boundary: the indexer's ingestion pipeline is the feed's ingestion pipeline; the Discovery lens is one of the five lenses on the same engine; the manifest the indexer reads (`/.well-known/civic.json`) is the same manifest that tells the feed where a space's activity endpoint lives. One pipeline, one index, many views. The division of labor with the Discovery Layer Specification is clean: the spec defines the *model* (federated publication, plural indexing, manifest and descriptor contracts, the re-binding protocol); this pilot ships the *reference implementation* and validates the model against live publishers.

---

<a id="7-architectural-role--proving-the-activity-layer"></a>
## 7. Architectural Role — Proving the Activity Layer

The Civic.Social architecture is organized into five canonical layers, read bottom → top: **Open Web Standards** (DIDs, Verifiable Credentials, ActivityPub/ActivityStreams, JSON-LD) → **Civic Specifications** (the four canonical specs: Civic Space · Civic Process · Civic Activity · Civic Identity, plus companions) → **Sovereign Foundation** (participant-owned identity and data: person / entity / community) → **Components** (reusable building blocks) → **Interfaces** (the software people use).

This pilot operates at the **Components** layer and proves the **Civic Activity Specification** beneath it. The Activity Feed engine is one of the two components explicitly designed to be shippable as a standalone embed on any web page (the other is the Process runtime, covered by the Civic Process Plugin Pilot). The engine is built once and consumed everywhere: the Citizen Dashboard, Civic Hubs, Representative Spaces, and external embeds are all *interfaces over the same component*, differing only in which lens they render and how they authenticate.

Each of the other canonical specs reaches its full value only when the activity layer works. The Civic Identity Specification makes citizens portable, but portability matters only if there is a stream that follows them. The Civic Process Specification makes every action emit an activity, but emission without aggregation is a log nobody reads. The Civic Space Specification requires every space to publish a manifest and a feed, but manifests without an indexer are addresses without a map. This pilot is where those obligations pay off. Federation is a capability of the Civic Activity Specification and the Discovery Layer — cross-cutting, not a layer of its own. In v0.1 the feed aggregates over HTTP feed-polling per the Space API Profile; the ActivityPub bridge is Phase-3 work in the Civic Activity Specification's roadmap, and this pilot's ingestion pipeline is designed so a federation transport can be added behind the same normalization step without touching the lenses (see section 18, out of scope).

---

<a id="8-one-engine-many-lenses"></a>
## 8. One Engine, Many Lenses

The central design move of this pilot is that there is **one activity feed engine**, and everything a user experiences as "a feed" is a **lens** over it.

The engine owns four responsibilities, executed once, in one place:

1. **Ingestion** — pull Civic Activities from the feed endpoints (`GET /events`) of every subscribed or indexed source, plus accept push delivery (webhooks) from sources that support it.
2. **Validation** — check every incoming activity against the Civic Activity Specification envelope before it enters the store (section 11).
3. **Classification** — pass every valid activity through the shared classifier exactly once, annotating it with its surfaces, kind, and display shape (section 10).
4. **Serving** — answer lens queries: given a viewer (or no viewer), a lens, and filters, return the correctly-scoped, correctly-ordered slice of the stream.

A lens is *not* a copy of the feed, a separate pipeline, or a fork of the classification logic. It is a named query profile plus a per-viewer state overlay (read state for Inbox, attention state for Notifications — see section 9). This is the direct productization of a failure mode the reference implementation already lived through: when each surface decides for itself what is feed-worthy, the copies drift, and the same activity appears in the digest but not the feed, or in the hub view but not the dashboard. The one-engine-many-lenses architecture makes that drift structurally impossible — there is exactly one place where a feed decision can be wrong, and therefore exactly one place to fix it. The engine is a reusable Component in the five-layer architecture, consumed three ways: as an in-process library (the reference hub), over its serving API (the Citizen Dashboard, third-party apps), or as the packaged embed widget (external websites, section 12). All three consumption modes hit the same store, the same classifier, and the same lens semantics.

---

<a id="9-the-five-lenses"></a>
## 9. The Five Lenses

The pilot ships five lenses. Together they cover the full range of how civic activity reaches people — from "everything I follow" to "a filtered slice on someone else's website."

**9.1 Inbox.** Aggregated activity from everything the viewer follows: spaces, processes, topics, organizations, credential issuers. The Inbox is the "civic inbox" of the pilot's framing — the one place a citizen checks. It carries **read state**: items are new until seen, and the viewer's position in the stream persists across devices (state lives behind the PDS seam, section 14). Ordering is chronological; filters (jurisdiction, type, source, "closing soon") narrow the candidate set but never re-rank it.

**9.2 Notifications.** Activities **directed at the viewer specifically**, as distinct from ambient activity in things they follow: @-mentions in deliberation threads, votes the viewer is eligible for in their jurisdiction, credentials issued *to them*, results published for processes they participated in, replies to their contributions. Notifications carry **unread/attention state** — a count the interface can badge — and are the primary input to notification delivery (section 13). The distinction between Inbox and Notifications is the classifier's `surface` decision (section 10): "your county posted a meeting agenda" is Inbox; "you are eligible to vote, closes Friday" is Notifications.

**9.3 Discovery.** **Unsubscribed** activity: the lens for finding what you don't yet follow. Discovery is served from the reference indexer's view of the ecosystem-wide stream, filtered by the viewer's jurisdiction and interests but explicitly *not* limited to their follow graph. It surfaces new hubs operating near the viewer, processes open in their jurisdiction, and credential issuers active in their area — each with a follow action that feeds back into the Inbox. Discovery works logged-out (jurisdiction supplied explicitly) and is the lens the Discovery Layer Specification's query endpoints exist to serve.

**9.4 Space View.** Everything happening inside a **single Civic Space**: one hub's stream, one representative space's stream. This is the lens a Civic Hub renders on its own front page and the lens a hub operator uses to see their community at a glance. Space View requires no viewer at all — it is a public window into one space's public activity — and respects `meta.visibility`: restricted activities appear only to viewers the space's authorization seam admits.

**9.5 Embed.** A **filtered activity surface a third-party site can drop into any page** — Space View, or any filter combination (jurisdiction, process type, source set), packaged as the embeddable widget (section 12). The Embed lens is read-only, public-visibility-only in v0.1, and is the pilot's proof that the feed component stands alone without the rest of the stack.

All five lenses consume the same classifier output and the same store. A sixth lens costs a query profile, not a pipeline.

---

<a id="10-the-shared-feed-classifier"></a>
## 10. The Shared Feed Classifier

Every activity that enters the engine passes through **one shared classifier** that answers, in a single decision:

```
classify(activity, context) → {
  surface:  which lenses it belongs to (inbox | notifications | discovery | space_view | embed — any subset),
  kind:     semantic category driving grouping and delivery (participation_open | participation_urgent |
            result | lifecycle | contribution | credential | mention | system),
  display:  rendering directive (headline template, summary derivation, action label, grouping key)
}
```

The classifier is a pure, deterministic function over the activity envelope — `event_type`, `data.process.type`, jurisdiction, actor, visibility, timing fields — plus viewer context where the lens is personal (is this actor the viewer? is the viewer eligible? does the viewer follow the source?). It is consumed by **every** lens and by **every** delivery renderer: the five lenses, the email digest, and future push/notification channels all read the same `{surface, kind, display}` annotation. Feed-worthiness logic exists in exactly one place.

This is a lesson bought with real drift. The reference implementation accumulated multiple independent copies of "is this activity feed-worthy, and how should it render?" — one in the feed UI, one in the digest renderer, others in per-surface filters — and they diverged exactly as copies do. The pilot productizes the single-classifier design as a named, tested, documented component with a versioned rule set.

**Admin-configurable classification is a future phase, not MVP.** The long-term direction is that a space operator can tune classification for their community — promote a process type to Notifications, demote system lifecycle chatter, define local kinds — through configuration rather than code. The pilot ships the classifier with a fixed, published default rule set and a schema deliberately shaped so rules can later become data. Building the admin surface now would freeze the rule model before real operators have used the defaults; it is explicitly deferred (section 18).

---

<a id="11-envelope-validation-at-ingestion"></a>
## 11. Envelope Validation at Ingestion

The feed service validates every incoming activity against the Civic Activity Specification **at the ingestion boundary**, before anything enters the store:

- **Required fields** (Civic Activity Specification §3): `id`, `version`, `event_type`, `timestamp`, `actor`, `jurisdiction`, `action_url`, `source`, `data`, `meta.visibility` — and `process_id` for process activities.
- **Type discipline** (§4): the `event_type` is canonical or a declared extension; every `civic.process.*` activity carries the `data.process.type` discriminator.
- **Structural rules**: parseable ISO-8601 timestamps, namespaced `data` payloads, resolvable `source` attribution.

Nonconforming activities are **rejected or flagged, never silently normalized**. Hard failures (missing required fields, unparseable envelope) are rejected and logged against the source; soft failures (missing recommended `source.space_id`, unknown-but-well-formed extension types) are ingested with a conformance flag and surfaced in a per-publisher conformance report. Publishers can query their own report — the feed tells you *why* your activity didn't appear.

The strategic point is larger than data hygiene: **the feed is the ecosystem's first serious consumer, and therefore its de facto conformance checker.** Because appearing in the feed — and in every dashboard, digest, and city-portal embed downstream of it — is the whole reason to emit activities, validation at ingestion converts the Civic Activity Specification from a document into a contract. This directly serves the conformance-phasing model the specs commit to (Activity spec §14): specs define the aspirational target state, and implementations converge through the pilots. The reference hub itself does not yet validate at emission; this pilot's ingestion validator is the pressure that closes that gap, and the validator ships as a standalone library so emitters can run the identical checks before publishing.

---

<a id="12-the-embeddable-feed-widget"></a>
## 12. The Embeddable Feed Widget

The Activity Feed works as a **standalone widget a third party can drop into any web page** — no Civic.Social account, no dashboard, no rest-of-stack required. A city government hosts a live activity stream on its own portal; a local newsroom embeds the "closing soon" slice for its county; a nonprofit embeds one hub's Space View on a campaign page.

The integration contract is deliberately minimal:

```html
<script src="https://cdn.civic.social/feed-widget.js"></script>
<civic-feed source="https://hub.floydcountyva.civic.social"
            jurisdiction="us-va-floyd"
            filter="participation" />
```

The widget:

- consumes any conformant activity feed endpoint directly, or the reference indexer for multi-source and jurisdiction-wide slices — it works against **any** spec-compliant Civic Space, not just the reference hub;
- renders the Embed lens: public-visibility activities only, read-only, chronological, with every item linking out via `action_url` to the source space where participation (and authentication) actually happens;
- carries no tracking, no cookies, and no behavioral instrumentation — consistent with the ranking-neutrality and privacy commitments (section 17);
- is themeable (CSS custom properties) and accessible, so it can sit inside a government site's design system and meet public-sector accessibility requirements.

The widget matters strategically out of proportion to its size. It is the pilot's fastest path to real-world utility (a city portal can adopt it before adopting anything else), the clearest demonstration that the feed is infrastructure rather than a destination, and the on-ramp by which organizations that embed the widget later become publishers into the stream they are displaying. The success criteria (section 21) make "third-party page, live hub, zero custom code" a measured claim.

---

<a id="13-digest-and-notification-delivery"></a>
## 13. Digest and Notification Delivery

Feeds are pull; civic life needs push — a citizen should not have to check an inbox to learn that a vote they are eligible for closes Friday. The reference hub already runs a working **email digest** in its community pilot, with its own rendering pipeline. The pilot productizes it as a proper delivery channel of the engine:

- **The digest is a lens consumer, not a fork.** The digest renderer reads the same classifier output (`{surface, kind, display}`) as every on-screen lens — the many-drifting-copies fix (section 10) applies to delivery channels with full force, and the existing digest's independent rendering logic is retired onto the shared classifier.
- **Digest** — a periodic (weekly by default) summary of the viewer's Inbox lens, grouped by the classifier's `kind` and grouping keys: new participation opportunities first, then results, then ambient activity. Space-scoped digests (a hub's weekly summary to its members) are the same renderer pointed at the Space View lens.
- **Notification delivery** — items the classifier routes to the Notifications surface with urgent kinds (`participation_urgent`, `mention`, `credential`) are eligible for immediate delivery rather than the next digest. v0.1 delivery is email; the channel interface is designed so push and other transports plug in later without touching classification.
- **Preferences** — frequency, channel, and kind-level opt-outs are viewer preferences stored behind the PDS seam (section 14). Delivery honors `meta.visibility` exactly as on-screen lenses do.

Deliverables here are deliberately modest — productize what exists, on the shared classifier, with a clean channel interface — because delivery is where scope creep lives (SMS, push apps, per-space branding). The pilot holds the line at email.

---

<a id="14-subscriptions-personalization-and-the-pds-seam"></a>
## 14. Subscriptions, Personalization, and the PDS Seam

Personal lenses need personal state: the follow graph (which spaces, processes, topics, organizations, and issuers the viewer follows), read state (Inbox), attention state (Notifications), filters, and delivery preferences.

The architecturally honest home for all of it is the citizen's **Personal Data Store** — the Civic Identity Specification's portable, citizen-owned data layer. Subscriptions are the citizen's data, not the feed's: a citizen who moves to a different feed provider, or a different Citizen Account Provider, takes their civic follow graph with them. The Civic Space Specification (§4.6) already models Subscription/Following as a canonical object for exactly this reason.

**The PDS is not yet built.** The Civic Identity Pilot defines it, and no implementation exists today. This pilot therefore:

1. defines the **subscription-state interface** the engine consumes — a narrow, PDS-shaped contract covering the follow graph and per-viewer lens state;
2. ships an **interim implementation** of that interface backed by the reference hub's own storage, honestly labeled as provider-held state pending the PDS;
3. commits to **migration**: when a PDS implementation lands, the interim store's contents are exportable into it, and the engine swaps backends without lens-visible changes.

This is the same replaceable-adapter discipline the Civic Space Specification applies to identity (§7.3), applied to personal data. It is also a named **coordination risk** (section 27): if the seam is drawn wrong, the eventual PDS integration becomes a rebuild instead of a swap. Mitigation is to keep the interface minimal, to review it jointly with the Civic Identity Pilot while both are in draft, and to treat every field the interim store accumulates beyond the interface as a red flag. Anonymous and logged-out use degrades gracefully: Discovery, Space View, and Embed work with no viewer at all; Inbox and Notifications simply require one.

---

<a id="15-the-reference-indexer"></a>
## 15. The Reference Indexer

The pilot ships the **reference indexer**: the first working implementation of the Discovery Layer Specification's indexing layer, and the backing service for the Discovery lens and the multi-source widget.

Per the Discovery Layer Specification §7.2, the indexer:

- **Ingests manifests** from `/.well-known/civic.json` — accepting both the current space-form manifest (`space: { id, scope, type }`) and the legacy top-level `type: "hub"` form for the life of v0.1 (Civic Space Specification §7.2.0);
- **Ingests activity feeds** from every manifest-declared feed endpoint, through the same validation boundary as the feed engine (section 11) — the indexer and the engine share one ingestion pipeline;
- **Normalizes** publishers into the three discovery entity classes: Civic Spaces (keyed by space DID), Civic Processes (participation processes via their descriptors; information processes via the minimal record), and Civic Activities;
- **Exposes query endpoints** by jurisdiction, type, and category — e.g. `GET /events?jurisdiction=us-va-floyd`, `GET /processes?category=process`, `GET /spaces?location=virginia` — plus basic keyword search across indexed entities;
- **Ranks** results chronologically with geographic proximity as the secondary signal, and nothing else (section 17);
- **Honors the migration re-binding protocol** (section 16).

Publication into the index is **open and permissionless**: any system that serves a conformant manifest and feed can be indexed, with no registration authority. And the index itself is **reproducible by design** — a pure function of public manifests and public feeds, so an independent party can run the indexer codebase (or write their own against the Discovery Layer Specification) and converge on the same index. Plural indexing — regional, issue-specific, third-party — is the model, and the reference indexer is deliberately just the default instance of it. The success criteria (section 21) measure reproducibility directly, because it is the difference between a discovery *commons* and a discovery *gatekeeper*.

---

<a id="16-space-identity-and-the-migration-re-binding-protocol"></a>
## 16. Space Identity and the Migration Re-Binding Protocol

A discovery layer that loses a community when it changes hosting providers is a lock-in mechanism wearing an open-standards costume. The Civic Space Specification's portability contract (§9) guarantees a space can migrate between engines, providers, and domains; the Discovery Layer Specification §7.4 defines how discovery survives that move; this pilot implements it. The indexer's implementation obligations, in full:

1. **Identity is the DID, not the URL.** Index entries for Civic Spaces — and the provenance of their activities — are keyed on the **space DID** (`source.space_id`; Civic Space Specification §3.5). The serving URL is a resolvable attribute re-derived from the DID document, never the identity itself. Legacy publishers without space DIDs are indexed by URL with a conformance flag, and upgrade in place when they adopt a DID.
2. **The migration signal.** On migration, a space emits **`civic.space.migrated`** as the final activity from its old location, carrying the new binding (Civic Activity Specification §4.4). An indexer consuming the old feed re-binds the space's entry — new URL, same DID, same history — on receipt.
3. **The tombstone.** For as long as the old domain remains under the community's control, the old `/.well-known/civic.json` serves a `moved` marker: `{ "moved": { "space": "<space DID>", "url": "<new URL>" } }`. An indexer encountering a tombstone re-resolves and re-binds — this is the recovery path when the migration activity was missed.
4. **No dangling entries.** After re-binding, previously indexed activities remain valid — their `source.space_id` is unchanged — and only the resolvable location updates. The old URL is retained as a historical alias so inbound links keep working.

The pilot validates this with a **live migration drill**: a test space is migrated between two engines mid-pilot, and the indexer must re-bind with zero lost history (section 21). `source.space_id` is optional in wire v0.1 and required in v0.2; the indexer prefers it wherever present, which makes the feed layer an adoption driver for space DIDs.

---

<a id="17-ranking-neutrality-as-a-design-principle"></a>
## 17. Ranking Neutrality as a Design Principle

MVP ranking is **chronological, with geographic proximity as the only secondary signal**. No engagement ranking, no recommendation model, no trending, no personalized ordering. This is a deliberate design principle, stated with governance weight — not a feature the pilot didn't get to.

Three reasons, in ascending order of importance:

1. **Civic information has a natural ordering.** What is happening, near me, in time order, is the correct default for civic life — the questions are "what's new in my county?" and "what closes soon?", both answered by time and place. Urgency ("closing soon") is a *filter* over deadline metadata the publisher declared, not a score the feed invented.
2. **Engagement optimization is the failure mode this infrastructure exists to avoid.** Every attention-economy pathology — outrage amplification, virality loops, popularity compounding into more popularity — enters through the ranking function. A civic distribution layer that optimizes for engagement will, given time, optimize civic life for conflict. Declining to build the mechanism is a stronger safeguard than building it carefully.
3. **Neutrality is what makes shared infrastructure trustworthy.** Hubs, governments, and organizations will publish into a stream they can reason about: activities appear when they happen, to the people whose place they concern. The moment ordering becomes discretionary, the feed operator becomes an editorial actor, and every publisher has to ask whose thumb is on the scale.

Anything beyond time and place is **future, opt-in, and transparent**, per the Discovery Layer Specification's phasing: relevance ranking and personalization in a later phase, and user-selectable, publicly-inspectable ranking providers after that — with chronological always available and every non-chronological ordering explainable per-item ("shown because: you follow this hub; closes in 2 days"). The pilot's contribution to that future is negative space: it commits the reference implementation, the widget, and the reference indexer to neutrality, so any future ranking has to argue its way in through the open governance process rather than drift in as a product tweak.

---

<a id="18-minimum-viable-pilot-scope"></a>
## 18. Minimum Viable Pilot Scope

### What the Pilot Will Demonstrate

An end-to-end activity-layer lifecycle:

1. A Civic Space (the reference hub) emits Civic Activities through its single emission path and publishes its discovery manifest.
2. The feed engine ingests the stream, validates every envelope at the boundary, and rejects/flags nonconforming activities with a queryable conformance report.
3. The shared classifier annotates every valid activity once; all five lenses and the digest renderer consume the same annotation.
4. A citizen sees followed activity in their Inbox, is notified of a vote they are eligible for, discovers a second space through the Discovery lens, and follows it — with all personal state behind the PDS-shaped seam.
5. A third-party web page embeds the feed widget against a live hub with zero custom code.
6. The reference indexer ingests manifests and feeds from multiple independent publishers, answers jurisdiction/type/category queries, and survives a live space migration via the re-binding protocol.
7. An independent party reproduces the index from public feeds and manifests alone.

### What is In Scope

- **The reference Activity Feed engine** — ingestion, validation, classification, serving; the five lenses (Inbox, Notifications, Discovery, Space View, Embed).
- **The shared feed classifier** with a fixed, published default rule set and versioned schema.
- **Envelope validation at ingestion** with per-publisher conformance reporting, shipped also as a standalone validator library for emitters.
- **The embeddable feed widget** — packaged, themeable, accessible, documented.
- **The reference discovery indexer** — manifest/feed ingestion, DID-keyed normalization, query endpoints, chronological + geographic ranking, the full migration re-binding protocol.
- **Digest and notification delivery** — the existing email digest productized onto the shared classifier; urgent-kind immediate email delivery; preference model.
- **The PDS-shaped subscription-state interface** with an interim reference-hub-backed implementation.
- **Documentation for third-party feed consumers** — consuming the stream over the API, embedding the widget, running or reproducing an indexer, and publishing conformant activity (the emitter's view of the validation rules).

### What is Explicitly Out of Scope

- **Algorithmic and engagement-based ranking.** Excluded as a design principle (section 17), not deferred as a feature. Future relevance ranking, if any, enters through the Discovery Layer Specification's phased, transparent, user-selectable model.
- **ActivityPub native federation.** The Phase-3 bridge per the Civic Activity Specification §9/§13. The ingestion pipeline is transport-extensible by design, but v0.1 aggregates over HTTP feed-polling and webhooks only.
- **Admin-configurable classification.** The classifier ships with a fixed default rule set; the operator-tuning surface is a future phase (section 10) so real usage can shape the rule model first.
- **The Personal Data Store itself.** This pilot defines and consumes the seam; building the PDS is the Civic Identity Pilot's work.
- **Global engagement metrics and proof-of-personhood.** Verified-participation metrics ("312 residents of your county participated") are a compelling direction, but computing and validating them belongs to the identity layer and originating processes; the feed would only display them. Deferred to the Civic Identity Pilot's track.
- **Moderation and content policy.** Publisher-level trust decisions (which sources an indexer or lens admits, spam handling) get *hooks* in this pilot — per-source admit/flag/reject controls at ingestion — but no moderation policy, reporting workflow, or governance model. See risks (section 27).
- **Delivery channels beyond email.** Push, SMS, and in-app channels plug into the channel interface later.

### Conformance Phasing

Consistent with the ecosystem-wide posture (Civic Activity Specification §14): **specifications define the aspirational target state; reference implementations converge on them through the pilot program.** This pilot is itself a convergence instrument — its ingestion validator is how emitters discover their distance from the target. Where this pilot's own components fall short of the specs mid-pilot (e.g., `source.space_id` handling before publishers adopt DIDs, interim subscription storage pending the PDS), the gap is documented in the conformance report rather than papered over. The pilot's exit state is honest: what conforms, what is flagged, what is deferred to which phase.

---

<a id="19-pilot-phases-and-timeline"></a>
## 19. Pilot Phases and Timeline

**Indicative duration: 6–9 months at typical scope**, scaling with the tiers in section 26. The timeline is moderated by what already exists: the reference hub emits a live, spec-aligned activity stream; a working feed UI and email digest run in the current community pilot; and the Discovery Layer and Civic Activity specifications are drafted. The pilot's work is consolidation, productization, and the indexer — not greenfield.

### Phase 1 — Engine Consolidation and the Shared Classifier (2 months)

Extract the feed engine from the reference hub into the reusable component: single ingestion path, envelope validation at the boundary, and the shared classifier replacing every drifting copy of feed-worthiness logic (feed UI, digest renderer, per-surface filters). Publish the classifier's default rule set and the validator library. Stand up the per-publisher conformance report. *Key deliverables: feed engine v0.1, shared classifier with published rules, validator library, conformance reporting.*

### Phase 2 — Lenses and Delivery (2 months)

Implement the five lenses as query profiles over the engine. Define the PDS-shaped subscription-state interface (jointly reviewed with the Civic Identity Pilot) and ship the interim implementation. Re-platform the existing email digest onto the classifier; add urgent-kind immediate delivery and the preference model. Integrate the Inbox/Notifications/Discovery lenses into the Citizen Dashboard surface. *Key deliverables: five lenses live, subscription-state interface + interim store, productized digest and notification delivery.*

### Phase 3 — The Embed and the Indexer (2–3 months)

Package the Embed lens as the standalone widget (CDN distribution, theming, accessibility pass, integration docs) and land it on at least one real third-party page against the live hub. Build the reference indexer: manifest/feed ingestion, DID-keyed entities, query endpoints, chronological + geographic ranking. Run the live migration drill (`civic.space.migrated` + tombstone re-binding, zero lost history). *Key deliverables: embeddable widget deployed on a third-party page, reference indexer live over multiple publishers, migration drill passed.*

### Phase 4 — Independent Reproduction, Documentation, and Handoff (1–2 months)

Support an independent party in reproducing the reference index from public feeds and manifests alone, and fold what that surfaces back into the Discovery Layer Specification. Complete the third-party consumer documentation set (API consumers, widget integrators, indexer operators, activity publishers). Publish the pilot report: validated claims, conformance state of the ecosystem's publishers, and the revision inputs for the Civic Activity and Discovery Layer specifications' next drafts. *Key deliverables: independent index reproduction, complete consumer documentation, pilot report, spec revision inputs.*

---

<a id="20-pilot-demonstration-scenarios"></a>
## 20. Pilot Demonstration Scenarios

### Scenario A — The Civic Inbox

A resident of Floyd County follows their county hub and a regional issue organization. Their county opens an advisory budget vote. The activity is validated at ingestion, classified as `participation_open`, and appears in the resident's Inbox; because the resident is eligible and the window is closing, a `participation_urgent` notification is delivered by email. The resident clicks through via `action_url`, participates on the hub, and the resulting `vote_submitted` and later `result_published` activities complete the loop in the same stream. This validates ingestion → classification → lens → delivery → participation end-to-end.

### Scenario B — The Zero-Code Embed

A city web team pastes the two-line widget snippet into their existing portal, pointed at their hub's feed endpoint and filtered to participation activities. The portal now shows a live, accessible, on-brand civic activity stream — with no Civic.Social account, no backend integration, and no custom code. This validates the standalone-component claim and is the pilot's most demonstrable public artifact.

### Scenario C — Discovery Without a Follow Graph

A citizen new to the ecosystem opens the Discovery lens, supplies (or confirms) their jurisdiction, and browses: hubs operating near them, processes currently open in their county, credential issuers active in their state. They follow their county hub; it appears in their Inbox from that moment. This validates the indexer's query endpoints, the geography-first ranking, and the discovery-to-subscription loop.

### Scenario D — The Space That Moved

A community migrates its hub from one hosting provider to another under the Civic Space portability contract. The old location emits `civic.space.migrated`; the old domain serves the `moved` tombstone. The reference indexer re-binds: same DID, same history, new URL — subscribers' Inboxes continue uninterrupted, and the widget embeds pointing at the old feed re-resolve. This validates the re-binding protocol and demonstrates, concretely, that the discovery layer does not lock communities to providers.

### Scenario E — The Nonconforming Publisher

A third-party system begins publishing activities missing `meta.visibility` and using an undeclared type. The feed rejects the hard failures and flags the rest; the publisher queries their conformance report, runs the standalone validator locally, fixes their emitter, and their activity flows. This validates the feed's role as the ecosystem's working conformance checker.

---

<a id="21-success-and-validation-criteria"></a>
## 21. Success and Validation Criteria

### Deliverable Criteria

The pilot is successful upon production of the reference feed engine with all five lenses, the shared classifier, the embeddable widget, the reference indexer, productized digest/notification delivery, and the third-party consumer documentation set (section 22).

### Technical Validation

Each of the following is a measurable, pass/fail claim:

- **Zero-code embed.** A third-party web page (not operated by Civic.Social) embeds the feed widget against a live hub using only the documented snippet — no custom code, no backend work — and displays a live activity stream.
- **Independent index reproduction.** An independent party, given only the Discovery Layer Specification, the public manifests, and the public feeds, reproduces the reference index: same entities, same DID keys, same activity provenance. Divergences are treated as spec or implementation bugs and resolved.
- **Single classifier, zero drift.** Every lens, the digest, and notification delivery render from the same classifier annotation. Validated by a drift test: for a fixed activity corpus, every surface's feed-worthiness and display decisions are byte-identical to the classifier's output; no surface carries independent classification logic.
- **Validation at the boundary.** No activity failing the required-envelope checks (Civic Activity Specification §3–§5) exists in the store. A seeded corpus of malformed activities is 100% rejected or flagged, each with a queryable reason.
- **Migration continuity.** In the live migration drill, the indexer re-binds within one ingestion cycle of the `civic.space.migrated` activity (or tombstone encounter), with zero lost activities and zero dangling entries; pre-migration activity provenance remains keyed to the unchanged space DID.
- **Notification correctness.** Activities directed at a viewer (eligibility, mentions, credentials issued to them) reach the Notifications lens and — for urgent kinds — email delivery; ambient followed activity never does. Validated against a scripted scenario corpus.
- **Ranking neutrality.** Every lens and every indexer query orders by time (with geographic proximity where declared) and nothing else — verified by inspection and by ordering tests over seeded corpora.
- **State behind the seam.** All personal state (follows, read state, preferences) is accessed exclusively through the subscription-state interface, demonstrated by swapping the interim backend for a mock without lens-visible changes.

### Future Usability Evaluation

User-facing metrics — whether citizens discover opportunities they would otherwise have missed, digest open-and-click-through behavior, widget adoption by real portals, publisher time-to-conformance — will be observed through the pilot deployments (the reference hub's live community pilot is the natural instrument). The pilot documents findings and recommends evaluation criteria for the production phase rather than committing to adoption targets it cannot control.

---

<a id="22-expected-deliverables"></a>
## 22. Expected Deliverables

- **Reference Activity Feed engine (v0.1).** The reusable component: single ingestion path, envelope validation, shared classification, lens serving. Consumable in-process, over its API, and via the widget.
- **The five lenses.** Inbox (read state), Notifications (attention state), Discovery, Space View, and Embed — implemented as query profiles over the one engine.
- **Shared feed classifier (v0.1).** The single `activity → {surface, kind, display}` function with a published, versioned default rule set — consumed by every lens and every delivery renderer. Admin configurability documented as the follow-on phase.
- **Envelope validator + conformance reporting.** Ingestion-boundary validation with per-publisher conformance reports, and the validator packaged as a standalone library for emitters.
- **Embeddable feed widget.** CDN-distributed, themeable, accessible, tracking-free; deployed on at least one real third-party page against a live hub.
- **Reference discovery indexer (v0.1).** Manifest and feed ingestion, space-DID-keyed normalization, jurisdiction/type/category query endpoints, chronological + geographic ranking, full migration re-binding protocol.
- **Digest and notification delivery.** The existing reference-hub email digest productized onto the shared classifier, plus urgent-kind immediate delivery and the preference model, behind a channel interface ready for future transports.
- **Subscription-state interface + interim store.** The PDS-shaped seam for follows and personal lens state, with the interim implementation and the documented migration path to the real PDS.
- **Third-party feed consumer documentation.** Four guides: consuming the activity stream over the API, embedding the widget, operating or reproducing an indexer, and publishing conformant activity.
- **Pilot report.** What was built, what was validated, the ecosystem's measured conformance state, and revision inputs for the Civic Activity and Discovery Layer specifications.

---

<a id="23-relationship-to-other-civicsocial-pilots"></a>
## 23. Relationship to Other Civic.Social Pilots

This pilot has integration points with every other pilot in the program, but it can be funded and executed independently. Preferred sequencing maximizes leverage; no pilot is a hard prerequisite.

**Civic Hubs Pilot.** Hubs are the primary publishers: their emission paths, manifests, and feed endpoints are what this pilot ingests. *Preferred sequencing: in parallel* — the feed's conformance reporting is most useful while hubs are actively converging on the specs. *Hard dependency: none* — the reference hub already publishes a sufficient live stream.

**Citizen Dashboard.** The dashboard is the primary consuming interface for the personal lenses. *Preferred sequencing: dashboard consumes this pilot's engine as it lands* — the lenses are the dashboard's core surfaces, and building them twice would recreate the drift problem. *Hard dependency: none* — the engine's API and the widget stand alone.

**Civic Identity Pilot.** Personalization state belongs in the PDS, which the Civic Identity Pilot defines and which **does not yet exist**. This is the most important cross-pilot coordination in the program for this pilot: the subscription-state seam (section 14) must be reviewed jointly while both pilots are in draft. *Preferred sequencing: identity's PDS design slightly ahead of or parallel with this pilot's Phase 2.* *Hard dependency: none* — the interim store carries the pilot, at the cost of a tracked migration and a named risk (section 27).

**Civic Process Plugin Pilot.** Processes produce most of the stream; the plugin framework's activity-emission seam and manifest-declared activity types are what make ingestion-side validation tractable (undeclared types are detectable). *Preferred sequencing: process pilot slightly ahead* (its posture as well). *Hard dependency: none.*

**Civic Credentialing & Profiles Pilot.** Credential-issuance activities are a canonical Notifications input, and issuers are a discoverable entity class. *Preferred sequencing: credentialing later.* *Hard dependency: none.*

---

<a id="24-estimated-development-effort-and-team-roles"></a>
## 24. Estimated Development Effort and Team Roles

Indicative timeline: **6–9 months at typical scope** across four phases (section 19), scaling to 4–6 months lean or 9–12 months expanded (section 26). The compressed range reflects what already runs: a live activity stream, a working feed UI, and a shipping email digest. The pilot is consolidation and productization plus one genuinely new service (the indexer).

Roles required (responsibilities, not headcount):

- **Civic architecture lead.** Owns the lens/classifier architecture, the conformance posture, and the revision inputs back into the Civic Activity and Discovery Layer specifications.
- **Lead full-stack engineer.** Owns the feed engine extraction, the classifier, the lenses, the delivery channels, and the widget (packaging, theming, accessibility, third-party deployment).
- **Indexer engineer.** Owns the reference indexer, the migration re-binding implementation, and the independent-reproduction support in Phase 4.
- **Identity/PDS advisor (part-time, shared with the Civic Identity Pilot).** Reviews the subscription-state seam so the interim store migrates cleanly to the PDS.
- **Documentation specialist (part-time).** Owns the four consumer guides.

At the **lean tier**, most responsibilities are absorbed by the founding steward with AI-assisted development, as with the other pilots in the program. The **typical tier** adds part-time contractors for the widget and documentation. The **expanded tier** funds the indexer as a properly independent service, multiple third-party embed deployments, and sustained publisher-onboarding work.

---

<a id="25-potential-pilot-partners"></a>
## 25. Potential Pilot Partners

**Embed hosts** — the widget's proving ground: a municipal or county web portal (the Floyd County pilot relationship is the natural first candidate), a local newsroom, or a civic nonprofit's campaign site. The success criterion requires at least one real third-party deployment.

**Publishers** — systems beyond the reference hub that can publish conformant manifests and feeds, giving the indexer genuinely plural sources: additional Civic.Social hub deployments, the representative-space service already running in production, and — as the Civic Process Plugin Pilot's exemplars mature — external tools like Decidim or Polis whose activity flows through plugin emission.

**Independent indexer operator** — a civic-data or research organization (a university civic-tech lab, a state-level civic data collaborative, or an organization like Ballotpedia with structured-civic-data experience) to perform the Phase 4 independent index reproduction — the pilot's strongest evidence that discovery is a commons. The feed's role as conformance checker also makes this pilot relevant to the interoperability communities the Civic Process Plugin Pilot engages (Metagov IDT's flatfile requirement is satisfied in part by exactly the feed endpoints this pilot consumes and documents); those alignment conversations are shared with that pilot rather than duplicated here.

---

<a id="26-estimated-budget"></a>
## 26. Estimated Budget

**The pilot scope is flexible, and the budget scales with it.** As with the Civic Process Plugin Pilot, figures assume the founding-steward operating model with contractors brought in by tier; a conventionally-staffed team would roughly double or triple the totals. The scope is moderated by the working feed and digest already running in the reference hub.

### Lean — $50,000 – $80,000 (≈ 4–6 months)

Engine consolidation, shared classifier, envelope validation with conformance reporting, the five lenses over the reference hub's own stream, digest re-platformed onto the classifier, and the widget packaged and deployed on one partner page. Indexer delivered at minimal scope: manifest ingestion, DID keying, and jurisdiction queries over a small publisher set; migration protocol implemented but drilled against a test space only. Documentation as quickstarts.

### Typical — $100,000 – $160,000 (≈ 6–9 months)

Everything in lean, plus: the full reference indexer with type/category queries and search across a plural publisher set; the live migration drill; the independent index reproduction with a named partner; notification delivery with the preference model; the complete four-guide documentation set; and the jointly-reviewed PDS seam with the Civic Identity Pilot.

### Expanded — $180,000 – $275,000 (≈ 9–12 months)

Everything in typical, plus: multiple third-party embed deployments with real government/newsroom partners; publisher onboarding for two or more external systems (conformance support through the validator and reports); the indexer operated as an independent service with published uptime; groundwork for the admin-configurable classifier (rule-set-as-data schema validated against operator interviews); and spec-revision contributions carried into the Civic Activity v0.2 wire-rename planning.

The main cost drivers across tiers: the number of genuinely independent publishers ingested (each surfaces new conformance edge cases), the depth of the embed's production-readiness work (accessibility, theming, government design-system fit), whether the Civic Identity Pilot runs in parallel (reduces seam-design risk and rework), the independent-reproduction partner's engagement depth, and documentation depth across the four consumer audiences.

---

<a id="27-risks-and-mitigations"></a>
## 27. Risks and Mitigations

**Risk: the PDS dependency turns into a rebuild.** Personal lens state architecturally belongs in the Personal Data Store, which is defined by the Civic Identity Pilot and **not yet built**. If this pilot's interim store accretes structure the eventual PDS cannot absorb, the swap becomes a migration project. *Mitigation: keep the subscription-state interface minimal and jointly reviewed with the Civic Identity Pilot while both are in draft; treat any interim-store field outside the interface as a defect; demonstrate backend-swap via mock as a success criterion (section 21); document the migration path as a deliverable.*

**Risk: activity volume outgrows the pilot's ingestion and serving architecture.** A feed that works for one hub's stream may degrade across dozens of publishers, or during a high-participation process that emits thousands of activities in a day. *Mitigation: design ingestion as an incremental, per-source cursor model from the start; load-test against synthetic corpora at 100× the current reference-hub volume; classify once at ingestion (not per-query) so lens serving cost is independent of rule complexity; treat the indexer and engine's shared pipeline as the single place scaling work lands.*

**Risk: feed spam and abuse.** Open, permissionless publication means bad actors can publish manifests and feeds too — junk activities, impersonation of civic sources, jurisdiction-squatting. A civic feed that fills with spam loses trust faster than a social feed does. *Mitigation: envelope validation already filters malformed abuse; ship per-source moderation hooks (admit/flag/reject controls at ingestion, per indexer and per lens operator) without shipping a moderation policy; key identity on DIDs so impersonation is a verifiable claim rather than a string match; scope full moderation/reporting governance as a named follow-on and an open question (section 28) rather than pretending v0.1 solves it.*

**Risk: ranking-neutrality erosion.** Product pressure ("surface the important stuff") is the standard path by which neutral feeds acquire engagement ranking one reasonable tweak at a time. *Mitigation: state neutrality as a governance commitment in this document and the Discovery Layer Specification, not an implementation detail; enforce it with ordering tests in the success criteria; require any future ranking to enter through the spec's phased, transparent, user-selectable model with chronological always available and per-item explanations.*

**Risk: the wire-format rename (`event_type` → `activity_type`) lands mid-pilot.** The v0.2 rename is planned in the Civic Activity Specification §14. *Mitigation: build all pilot components against a single internal envelope model with the wire mapping isolated at ingestion and serving edges; when v0.2 lands, support dual-read for the documented deprecation window — a mapping change, not an architecture change.*

**Risk: scope creep from "feed infrastructure" toward "feed product."** Notification channels, richer cards, engagement affordances, and per-space theming are each individually reasonable and collectively a platform. *Mitigation: hold the deliverable line at the components and contracts in section 22; email-only delivery; the widget is read-only; anything interactive belongs to the consuming interfaces, not the engine.*

---

<a id="28-open-questions-for-further-design"></a>
## 28. Open Questions for Further Design

**Who moderates an open stream, and where?** The pilot ships moderation *hooks* (per-source controls at ingestion) but not moderation *policy*. Is source admission an indexer-level decision, a lens-operator decision, a jurisdiction-verified allowlist, or all three at different layers? The plural-indexing model implies plural moderation — which is a strength for autonomy and a hazard for consistency. The pilot will document where moderation pressure actually appears.

**What is the right eligibility signal for Notifications?** "Votes you are eligible for" requires the feed to know something about eligibility, which is properly the process's and identity layer's knowledge. v0.1 approximates with jurisdiction matching; the correct long-term seam — processes publishing eligibility descriptors the classifier can evaluate against a viewer's credentials without learning them — needs joint design with the Civic Identity and Process pilots.

**How should read/attention state scale across many lenses and devices?** Per-item read state for high-volume streams is a real storage and sync question, and its answer shapes the PDS seam. Cursor-based ("read up to here") versus per-item models both have failure modes the pilot will surface.

**Where does deduplication land?** `dedupe_key` is carried but uninterpreted in wire v0.1 (Civic Activity Specification §12). Aggregating plural sources makes duplicates inevitable (the same activity via a hub's feed and an indexer). The feed is the natural place for dedupe semantics to be forced; the pilot will propose them for the activity spec's next revision.

**What does the embed need for restricted-visibility content?** v0.1 embeds are public-only. City portals will eventually want authenticated embeds (resident-only streams on a city site). That requires an identity handoff into a third-party page — a hard problem owned jointly with the Civic Identity Pilot.

**When does the classifier's rule set become community-governed?** Admin-configurability is the named next phase — but "the hub operator tunes classification" and "the community governs what its members' inboxes prioritize" are different claims. The second touches the same governance questions as ranking, and should probably travel through the same open process.

---

<a id="29-conclusion"></a>
## 29. Conclusion

The Civic Activity Feed & Discovery Pilot is the work that turns the Civic Activity Specification and the Discovery Layer Specification into shippable infrastructure: one feed engine refracted into five lenses, an embeddable widget any web page can host, a reference indexer any party can reproduce, and the validation boundary that makes the ecosystem's activity contract enforceable in practice.

Its deliverables serve two audiences at once. For the Civic.Social ecosystem, the pilot delivers the distribution layer every other pilot presumes: hubs' activity becomes visible beyond their own walls, the dashboard gets its core surfaces, processes get an audience that compounds instead of resetting to zero, and identity gets a stream worth carrying between providers. For the broader civic ecosystem, the pilot delivers something rarer — a civic information layer with its incentives stated up front: chronological and geographic ordering as a governance commitment, plural and reproducible indexing instead of a gatekeeper, portability that survives provider changes, and a widget that gives any city portal a live civic stream without asking it to join anything.

The strategic bet is that civic participation compounds when civic activity is visible — and that visibility infrastructure for democratic life must refuse, structurally, the attention-economy mechanics that made social feeds untrustworthy. An inbox for civic life, not a feed competing for it: that is what this pilot builds, and the measure of its success is that others can embed it, reproduce it, and build on it without permission.

---

*Civic.Social — civic.social | contact@civic.social*
