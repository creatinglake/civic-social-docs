---
status: draft
last-reviewed: 2026-04-19
owners: [adam]
version: 0.1
---

# Civic.Social Authorization Model Note

**Status:** Thinking document — draft for discussion
**Last updated:** 2026-04-14
**Scope:** Cross-cutting guidance on how authority is modeled, delegated, and enforced across the Civic.Social ecosystem. Covers the near-term role-based approach, the long-term capability-based direction, and the architectural discipline required today to keep the future open.

---

## The Question

Every civic action — submitting a vote, creating a proposal, moderating content, publishing an outcome, authorizing a delegate to act on one's behalf — is ultimately an **authorization decision**. How does the ecosystem answer "is this actor permitted to do this thing, to this resource, in this context?"

The conventional answer is role-based access control (RBAC): identify the actor, look up their role, consult a permission table. This works well for small, centrally operated systems. It breaks down badly in federated, decentralized, delegation-heavy contexts — which is exactly what Civic.Social is.

This document defines the authorization direction for the ecosystem: role-based in the near term, capability-based in the long term, with an architectural discipline today that makes the transition possible without re-plumbing the entire system.

---

## Two Models, Briefly

**Role-Based Access Control (RBAC).** The system asks: "who are you, and what role do you have?" Permissions are attached to roles (admin, moderator, member) and roles are attached to identities. Well-understood, easy to implement, ubiquitous.

**Capability-Based Authorization (object capabilities, "ocaps", ZCAPs).** The system asks: "do you hold a signed token proving you have authority to perform this specific action on this specific resource?" Capabilities can be delegated (I grant you a subset of my authority), attenuated (you get a narrower capability than I hold), revoked, audited, and transferred without the system having to know who you are in advance. The W3C Authorization Capabilities for Linked Data (ZCAP-LD) specification formalizes this pattern in the same DID/VC ecosystem that Civic Identity is built on.

The key difference is that roles attach authority to identity, while capabilities attach authority to **signed grants**. Delegation in a role system requires the central system to mediate ("add X to the moderator role"). Delegation in a capability system is a peer-to-peer act ("I sign a capability granting you this authority, scoped to this resource").

---

## Why Capability-Based Authorization Fits Civic Life

Representative democracy is, at its core, capability delegation. A citizen granting a representative the authority to act on their behalf is a capability. A voter authorizing a proxy to cast their vote is a capability. A mayor granting staff the authority to manage specific municipal processes without making them full administrators is a capability. A community appointing a moderator for one specific forum rather than the whole hub is a capability. Every one of these actions fits awkwardly or not at all into role-based models, and the workarounds — complex role hierarchies, custom delegation flows, permission tables that grow unbounded — are exactly the kind of accidental complexity that undermines long-term maintainability.

Capability-based authorization also addresses several problems that are specific to federated civic infrastructure:

**Cross-hub action.** When a citizen authenticated in one hub needs to take an action in another — endorse a proposal, sign a petition, participate in a regional vote — role-based systems require synchronizing identity and permissions across hubs. Capability-based systems simply require verifying a signed capability presented by the citizen.

**Scoped and time-bounded authority.** A citizen may authorize a civic group to act on their behalf for one specific campaign, for one week, on one hub. Expressing this cleanly in roles requires inventing disposable roles; capabilities handle it natively.

**Credential-gated delegation.** Only a verified resident should be able to delegate their voting authority within a municipal process. Capability issuance can be conditioned on verifiable credentials, linking identity verification and authorization delegation in a single cryptographic flow.

**Audit and transparency.** A delegation chain captured as a sequence of signed capabilities produces a tamper-evident record of who granted what to whom, when, and under what scope. This is important for civic legitimacy — citizens and observers need to be able to see and verify the provenance of authority.

**Revocation without coordination.** A citizen who delegated a capability can revoke it without asking the target hub's administrator to remove a role. Revocation is a first-class part of the capability model.

---

## Civic Use Cases

The following use cases illustrate where capability-based authorization concretely improves the ecosystem. They are ordered roughly from near-term relevance to longer-horizon ambition.

**Moderation delegation.** A hub administrator delegates moderation authority for a specific discussion space, campaign, or process to a trusted community member — without granting hub-wide admin rights. The moderator's authority is scoped, revocable, and auditable.

**Process creation authority.** A mayor, department head, or civic organization leader delegates authority to create specific types of civic processes (e.g., public comment periods, advisory votes) to staff or subordinates, without granting full hub administration.

**Vote delegation and liquid democracy.** A citizen delegates their voting authority on a specific proposal, for a specific duration, to a trusted neighbor, expert, or representative. The delegation is visible, revocable, and scoped to a specific process or process class.

**Credentialed representation.** A civic group that has gathered verifiable credentials from its members acts on behalf of those members in a regional or national process — signing a petition, submitting testimony, participating in a deliberation — using capabilities granted by each member.

**Inter-hub collaboration.** A neighborhood hub and a city hub collaborate on a joint process. Rather than merging governance, they exchange capabilities: the city hub holds a capability to publish results into the neighborhood hub's feed; the neighborhood hub holds a capability to submit aggregated positions into the city hub's process.

**Capability-gated AI action.** An AI assistant operating on behalf of a citizen holds a narrowly scoped capability to take specific civic actions (e.g., draft a comment, flag a process for review) without holding the citizen's full identity or authority. This becomes important as AI agents participate more in civic workflows.

---

## Where It Fits in the Ecosystem

By layer:

**Identity.** Capabilities are signed using the same cryptographic primitives as verifiable credentials. The identity layer provides the keys, the signing infrastructure, and the verification services. Capabilities are issued, held, and presented through the identity system.

**Citizen Dashboard.** The citizen's dashboard is the natural place to hold capabilities, display the capabilities the citizen has granted to others, and provide the interface for revocation. Capabilities held on-device alongside DIDs and credentials are part of the "civic wallet" pattern described in the Client Architecture Note.

**Hub.** The hub is where most enforcement happens. When a participant takes an action, the hub verifies either a role (near term) or a capability (long term) before executing. The hub must be designed with a single, replaceable authorization seam so this evolution is possible.

**Process.** Processes define what authorities exist — "who can submit a vote," "who can moderate comments," "who can finalize results." These authorities must be expressible as capabilities, not only as roles, so that delegation and scoped grants work naturally.

**Feed and Discovery.** The activity feed and discovery layer respect capability-scoped visibility — some events may be visible only to holders of specific capabilities, not just to members of a role.

---

## Architectural Discipline for the Near Term

The pilot will not implement ZCAPs. But the pilot must not paint the ecosystem into a corner that makes ZCAPs impossible or expensive to add later. The discipline required today:

**A single authorization seam.** Every authorization check in the hub and process layer should route through a single abstraction — `canActor(actor, action, resource)` or equivalent. Not scattered `if user.role === 'admin'` checks throughout the codebase. When the authorization check is a single seam, it can evolve from "look up role" to "verify capability" without touching the rest of the system.

**Authority as resolvable claims, not baked-in role strings.** Instead of storing permissions as hardcoded role enums, represent them as structured claims that can be resolved against either a role table (today) or a capability verifier (tomorrow). This is a data modeling choice made early.

**Process-defined authorities, not hub-defined roles.** Each civic process type should declare the authorities it recognizes (`civic.vote.submit`, `civic.proposal.finalize`, `civic.process.moderate`). Today those authorities map to hub roles. Tomorrow they map to capabilities. The process plugin framework should treat authorities as first-class objects that can be bound to either model.

**Replaceable identity adapter.** As already specified in the hub architecture, the identity adapter must be modular. Capability verification is an identity-layer concern — it belongs in the same adapter as DID authentication and credential verification.

**Audit log compatibility.** Authorization decisions should be loggable in a form that extends naturally from "action X taken by user Y with role Z" to "action X taken by user Y presenting capability C, chained from grant G, issued by Z." The audit format should anticipate the capability model.

---

## Phasing

**Phase 1 (Pilot).** Role-based access control. Roles defined per hub, bound to conventional accounts or stub identity. Single authorization seam. Authority claims modeled as structured objects. No ZCAPs. This is what the hub pilot ships.

**Phase 2 (Post-pilot, with mature Civic Identity).** Introduce capability issuance and verification for a small set of high-value use cases — most likely moderation delegation and process creation delegation — while keeping role-based access control as the default. Hybrid period.

**Phase 3 (Capability-native).** Core authorization model is capability-based. Roles become one of several ways to express authorities, implemented as pre-issued capability bundles. Vote delegation, cross-hub action, and liquid-democracy experiments become possible.

The pilot document should commit only to Phase 1 but name the trajectory.

---

## Expertise and Communities to Engage

Capability-based authorization is a niche but mature area with clear experts and active working groups. The following are the most directly relevant:

**Manu Sporny and Dave Longley (Digital Bazaar).** Co-editors of the W3C ZCAP-LD specification. Active in the W3C Credentials Community Group. The right people for the formal specification work and for connecting Civic.Social to the broader verifiable credentials ecosystem.

**Christopher Lemmer Webber and the Spritely Institute.** Spritely is building federated social infrastructure on object capabilities (Spritely Goblins, OCapN protocol). The conceptual alignment with Civic.Social is striking — Spritely is rethinking social systems for security and agency via capabilities; Civic.Social is rethinking social systems for democratic legitimacy. There is likely real technical and values overlap worth exploring.

**The Decentralized Identity Foundation (DIF).** Working groups on delegation, authorization, and credentials. Useful for community connection and finding additional contributors.

**Spruce Systems and Transmute.** Commercial VC/DID infrastructure companies with production experience in capability-style delegation. Useful for pragmatic implementation guidance when the ecosystem moves toward Phase 2.

**Mark S. Miller and the object capability research tradition.** The foundational academic and industry body of work on ocaps dates back decades (E language, Caja, Spritely's Goblins heritage). When the architecture is being designed in earnest, this tradition is the intellectual anchor.

---

## Relationship to Other Pilots

This note informs several pilot documents:

**Civic Hubs Pilot.** Hubs must be designed with a single authorization seam (see Section 12, Item 1, and the forthcoming subsection on authorization model evolution). The pilot uses RBAC; the architecture accommodates capabilities later.

**Civic Identity Pilot.** Capabilities are signed with DID keys and verified through the identity layer. The identity pilot should note that the identity service will eventually issue and verify capabilities, not only credentials.

**Citizen Dashboard Pilot.** The dashboard holds capabilities as part of the civic wallet pattern. Capability management UI (view granted, revoke, delegate) is a dashboard concern and should be named even if not built in Phase 1.

**Civic Process Pilot.** Process specifications should declare authorities as named, structured objects that can bind to either roles (today) or capabilities (tomorrow).

**Civic Feed and Discovery Pilot.** Capability-scoped event visibility is a long-horizon consideration for the feed architecture.

---

## Open Questions

- What is the minimum capability framework the pilot architecture needs to anticipate? (Strict ZCAP-LD? A simpler internal abstraction that can be upgraded to ZCAP-LD?)
- How does the authorization model handle anonymous or pseudonymous participation, where the actor's identity may be unknown but their authority must still be verifiable?
- How do capabilities interact with credentialed eligibility? (Is "resident of Portland" a credential, a capability, or both?)
- What is the revocation propagation model across federated hubs?
- Should the ecosystem adopt ZCAP-LD as written, or define a civic-specific capability profile?

---

## Summary

The Civic.Social ecosystem is, structurally, a federated system of delegated authority — which is to say, it is a capability-based system whether or not that is made explicit in the code. The near-term pragmatic choice is role-based access control. The long-term architectural direction is capability-based authorization. The discipline required today is to keep the authorization check a single, replaceable seam and to model authorities as structured, resolvable claims rather than hardcoded role enums. Doing this costs almost nothing in Phase 1 and preserves the capability-native future.
