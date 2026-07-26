---
status: review
last-reviewed: 2026-07-26
owners: [adam]
version: 0.1
---

# Civic.Social Horizons — What Exists Now, What the Pilots Build, Where This Is Headed

## Why this document exists

Civic.Social documents are written in two different registers, and until now
nothing told a reader which one they were holding.

The **case material** — the memo, the positioning documents, the deck, *Before
& After* — describes the destination. It answers "what is this for?" and it
describes a working civic ecosystem: identity you carry between jurisdictions,
hubs that talk to each other, a single feed of everything you participate in.
That description is the point of the project, and it is worth stating clearly.

The **specifications** — Civic Space, Civic Process, Civic Activity, Civic
Identity, and the pilot specs — describe what can be built and verified
*right now*. They are deliberately conservative. Where something is not yet
settled, they say so and defer it. Where a guarantee is not yet real, they
decline to promise it.

Both are honest. The confusion comes from putting them next to each other
without a marker. A reader moving from the memo's "citizens onboard once and
carry a portable, privacy-preserving credential across jurisdictions" to Civic
Identity §7.4's "v0.1 does not provide cross-context unlinkability" has no way
to tell whether they have found a contradiction, a lie, or a schedule.

It is a schedule. This document is the schedule.

Nothing here changes what any specification requires. The specifications remain
the authority on what conformance means. This document only places their
requirements on a timeline and connects them to the promises the case material
makes.

## The three horizons

Every capability in this document is placed on one of three horizons. When a
Civic.Social document makes a claim, it is making it on one of these, and it
should say which.

**Now — v0.1.** What the current specifications actually require. A team could
build this today and a reviewer could check whether they had. If the
specifications require it, it is here. If they defer it, it is not, no matter
how central it is to the vision.

**Pilot.** What the seven pilot programs are scoped to add. This is real,
sequenced, budgeted work with named deliverables and acceptance criteria — but
it is not built yet, and pilot specs are explicit that some of it may not
survive contact with a real community. Read "Pilot" as *committed intent with
known risk*, not as *done*.

**Vision.** The destination the case material describes. Direction, not
schedule. Nothing on this horizon has a date, and some of it depends on
decisions that have not been made — the open questions each specification
carries in its final sections. Statements on this horizon are what the project
is *for*, and they are the reason the other two horizons are worth doing.

A useful test: if a sentence about Civic.Social could be falsified by someone
opening a laptop and checking, it belongs on the Now horizon. If it could be
falsified in eighteen months by a pilot report, it belongs on the Pilot
horizon. If it can only be judged years out, it is Vision.

## The horizons table

| Capability (as the case material promises it) | Now — v0.1 | Pilot | Vision |
|---|---|---|---|
| **Shared civic identity** — "onboard once → access everywhere" | DIDs and Verifiable Credentials are specified and implementable. A holder authenticates to a space by proving control of a DID. Key custody is *managed by default*, with self-custody available. | Bring-your-own-identity and cross-hub authentication; credential-verified participation in hubs; provider migration demonstrated end-to-end between two independently operated providers. | Onboard once, participate in any jurisdiction, with no operator holding anything a holder cannot take with them. |
| **A single steward does not hold your identity** | Not yet true, and the specification says so. The initial deployment concentrates DID infrastructure, credential registries, managed keys, and **social-graph metadata** in one nonprofit steward. Civic Identity §10 states this is "a known and real cost, not a technicality." | Portability is the exit path: full export of keys, credentials, and social graph, and migration to another compliant provider. §10 says this migration **SHOULD be demonstrated** during the pilot rather than assumed — if it is never exercised, the transitional claim has failed. | An ecosystem of interoperable providers under multi-stakeholder governance, where no single operator can centralize control. Stated direction, explicitly *not* a scheduled commitment. |
| **Federated Civic Hubs** aligned with real-world jurisdictions | **Federation is OPTIONAL** (Civic Space §5.1). A hub that never federates is still a conformant Civic Space. Hubs interoperate by *pull* — one hub reads another's activity feed endpoint. Nothing in v0.1 requires an inbox, an outbox, or push delivery. What v0.1 does require is that a hub which *does* federate use an open protocol rather than inventing a private one. | Two hubs exchange activity and demonstrate cross-hub interoperability with real communities. Push delivery, webhooks, and ActivityPub inbox/outbox arrive with the federation work, not before. | Every jurisdiction — town, county, school board, state, federal — running its own locally governed hub, interconnected by shared protocol. Whether federation becomes *required* for conformance in a later version is an open question (Civic Space §12.4). |
| **Civic Feed & Dashboard** — one civic inbox | The activity envelope is fixed: required fields, the canonical type registry, the two-value visibility class, and the `GET /events` response with pagination and filters. That is enough for two independent implementations to read each other's streams. | The Civic Activity Feed & Discovery Pilot and the Citizen Dashboard Pilot build the aggregating interface, the discovery lens, and the indexer. The dashboard pilot's top risk is the personal data store it depends on. | The three-layer civic interface of the memo: participation stream, civic information layer, civic tools layer — aggregating across every hub and tool a person touches. |
| **Portability — no lock-in** | Every space MUST export. The conformance floor is **Level A (Archival)**: a complete, integrity-verified copy of everything the space stewards, importable elsewhere. Unqualified "conformant Civic Space" means Level A and nothing above it. | **Level B (Continuity)** is the pilot-phase target: identity re-binding, memberships and roles reconstructed, closed-process tallies and outcomes verifiable after the move. Validated by round-trip testing between two live engines with a real community's data. | **Level C (Live-state)** — migrate a community mid-vote without interrupting it. This is the v1.0 objective and is gated on a plugin-state export contract that does not exist yet. |
| **Privacy-preserving participation** | Real but bounded. A verifier learns the jurisdiction tier, not the address. No central login service that every space must consult. But the DID is a stable identifier, so **v0.1 does not provide cross-context unlinkability**: `anonymous` and `pseudonymous` mean anonymity toward other participants and toward published outputs, *not* toward the space verifying you. Operators MUST NOT describe v0.1 participation as unlinkable. | Selective-disclosure credentials in practice; disclosure policy exercised against real processes; the honest limits tested where they bite. | Unlinkable participation where a process calls for it. This likely requires pairwise or per-space DIDs, whose cost is cross-space continuity — the "authenticate once, participate everywhere" property depends on a stable identifier. A hybrid is the likeliest answer and **has not been specified** (Civic Identity §14). |
| **Open interoperability standards** | Four canonical specifications published, plus companions. They cite established open standards — W3C DIDs, Verifiable Credentials, OpenID4VCI/VP, WebAuthn — rather than inventing alternatives. | Three foundational artifacts the specs depend on are **Planned and unpublished**: the civic JSON-LD context at `civicsocial.org/ns/civic`, the export canonicalization profile, and the Space Conformance Suite. Until they publish, conformance is validated by round-trip testing and reviewed export, not by machine. Anything a spec defers to a planned document should be read as unsettled. | A standards body other people implement against, with machine-checkable conformance and no dependency on Civic.Social the organization. |
| **Bridged to the wider fediverse (ActivityPub)** | Not built. The ActivityStreams mapping in Civic Activity §9 is, in the spec's own words, "a compatibility sketch rather than a bridge specification." The envelope is designed so a bridge can be added without invalidating activities already emitted. | The Civic Hubs pilot places full ActivityPub in scope for *evaluation*, not delivery. | Native ActivityPub support, verb-based type aliases, and civic activity legible to any fediverse client. |
| **Governed as a public good** | The nonprofit intent is stated; the governance mechanism is not yet built. | The **Civic.Social Governance Pilot** is the one pilot whose subject is this question — how the specifications themselves get governed, which the other six each defer. | A steward with no equity ownership, no data monetization, and assets permanently dedicated to public benefit — durable enough that the guarantee outlives the people who wrote it. |

## Notes on the sharpest gaps

Three rows above deserve more than a table cell, because they are the ones most
likely to be read as overclaiming.

**"Federated Civic Hubs" is a headline promise and federation is optional
today.** These are not in conflict, but the reconciliation is worth stating.
The hard part of federation is not the transport — it is agreeing on what
travels. v0.1 fixes the object: the activity envelope, the type registry, the
visibility class. Two hubs that agree on those can already exchange civic
activity by polling each other, which is enough to prove the model works.
Push delivery is an efficiency, and mandating it early would have forced every
small jurisdiction to run infrastructure it does not need to participate. The
choice was to make the *data* portable first and the *plumbing* optional, and
to require that anyone who does build the plumbing use an open protocol.

**"Privacy-preserving" is true in a narrower sense than most readers will
assume.** The word usually implies "no one can tell it was me." What v0.1
delivers is "no one learns more about me than the process requires, and no
central party sees every login." The space you participate in *does* know
which identifier acted. Civic Identity §7.4 says this in the specification
rather than leaving it for someone to discover, and it forbids operators from
describing v0.1 participation as unlinkable. Closing this gap properly is a
real design decision with a real cost, and it is on the open-questions list
rather than the schedule.

**A single steward holds the social graph, and that is the least comfortable
fact in the architecture.** Credentials can be verified by anyone; a social
graph cannot. Concentrating it in one operator during the transition recreates
the central observer this project exists to avoid. The specification names
this plainly, and the answer it offers is not a cryptographic one — it is
governance (nonprofit control), minimization (relationships and pointers, never
content), and a working exit. The exit is the load-bearing part. Portability is
what makes the transitional model transitional, which is why the specification
asks for provider migration to be *demonstrated* during the pilot rather than
merely offered.

## Two things this document does not resolve

**"Phase 3" is used for two different things.** Several specifications refer to
"the Phase 3 federation work" — an *ecosystem-wide* horizon covering the
ActivityPub bridge, push delivery, and the published JSON-LD context.
Separately, most pilot specs number their own internal phases, and each has a
"Phase 3" of its own that means something entirely different and shorter. These
two numbering systems have never been reconciled, and the ecosystem-wide
Phase 3 has no definition anywhere in this repository. Until that is settled,
read "the Phase 3 federation work" as *the federation horizon in this table*,
and read any "Phase 3" inside a pilot spec as internal to that pilot.

**The pilot horizon is not a single timeline.** The seven pilots have
dependencies on each other — hubs need identity, the dashboard needs the hub
APIs and a personal data store, credentialing needs both — and those
dependencies are documented pilot by pilot rather than in one sequence. The
Civic Hubs pilot notes that hubs can launch on conventional authentication
before Civic Identity is ready. Building the single cross-pilot schedule is
work that has not been done, and this document deliberately does not fake one.

## For anyone writing a Civic.Social document

The convention this document establishes is small: **say which horizon you are
on.**

In specifications, that already happens — "in v0.1," "deferred to," "planned,"
"out of scope for this version." Keep doing it, and keep it in the same
sentence as the claim rather than in a footnote.

In case and narrative material, the destination is the right thing to describe,
and it should be described vividly. The only requirement is that the tense not
imply it is finished. "Civic.Social connects every jurisdiction" and
"Civic.Social is built to connect every jurisdiction" cost the same number of
words and differ entirely in what they promise. Where a document paints the
destination at length — a scenario, a day-in-the-life, a "what this feels like"
passage — a single line naming it as the destination, plus a pointer here, is
enough.

---

**Last updated:** July 26, 2026
