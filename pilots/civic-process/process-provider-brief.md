---
status: review
last-reviewed: 2026-06-03
owners: [adam]
version: 0.1
---

# Civic Process Plugin Pilot — Process Provider Brief

Civic.Social Infrastructure Program

> **Who this brief is for.** Civic-tech tool authors — organizations and teams who have already built, are building, or are planning to build civic-participation infrastructure and who want their tool to reach a wider audience without per-deployment integration work. Tools like **Polis** (the Computational Democracy Project's opinion-clustering platform), **MAPLE** (the Massachusetts state legislature public testimony platform), and **Frankly** (Harvard's open-source video deliberation platform) are illustrative examples of the kind of civic process provider this brief addresses. If you're considering making your tool a Civic Process Plugin — or designing one from scratch with the plugin contract in mind from day one — this brief tells you what's in it for you, what you'd need to build, and which parts of the longer pilot specification you should read carefully.

---

## What's in it for you

Civic-tech tools today have a distribution problem. Your tool may be technically excellent, but every adopter — every municipality, every nonprofit, every civic organization — has to integrate it separately. They onboard their own users, run their own identity system, decide where your tool fits in their existing civic infrastructure, and figure out how to surface results to their community. The integration burden falls on every process host. Many potential adopters never go past evaluation because the per-deployment cost is higher than the perceived benefit, even when the tool is exactly right for their use case.

The Civic Process Plugin Pilot is the work of *designing* the framework that would remove that burden. **The sections below describe how the plugin framework is intended to work once the broader Civic.Social program is in place.** None of this is a finished, shipping product today.

The plugin pilot does not stand alone. It sits alongside complementary pilots designing the shared identity layer (the [Civic Identity Pilot](../civic-identity/civic-identity-pilot-spec.md)), the community-operated deployment surfaces (the [Civic Hubs Pilot](../civic-hubs/civic-hubs-pilot-spec.md)), and the standardized activity distribution layer (a forthcoming Civic Activity Feed Pilot, with the underlying activity model defined in the [Civic Activity Specification](../../ecosystem/civic-activity-spec.md)). Early pieces of the program exist today — the Civic.Social Hub reference implementation runs `civic.vote` and `civic.proposal` plugins, and Polis is deployed at `polis.civic.social` and integrated into the representative-space service — but the full shared identity infrastructure and federated activity stream are still under design. If you engage with the framework now, you are engaging at the design stage, not the shipped-product stage.

With those caveats explicit, the design intent is that if a tool conforms to the Civic Process Plugin Specification:

- **Compliant Civic Hubs would be able to install the tool with no per-hub integration work.** Civic Hubs — being designed in the Civic Hubs Pilot — are operated by communities, jurisdictions, and organizations. Each compliant hub would be a well-defined civic deployment surface; citizens who participate in any compliant hub would be able to use the tool.
- **Compliant Citizen Dashboards, Representative Spaces, and third-party civic applications would also be able to install it.** A plugin is universal by default — the same plugin code is intended to run anywhere a compliant host exists, so the tool would no longer be tied to one platform's data model or one organization's user base.
- **Identity would be handled by the shared decentralized identity layer being designed in the [Civic Identity Pilot](../civic-identity/civic-identity-pilot-spec.md)** (W3C DIDs and Verifiable Credentials). The plugin would not run accounts itself — citizens would arrive with verified credentials and the plugin would declare what credentials it requires for participation. Eligibility, residency, organizational membership, proof of personhood — all would be handled at the layer below the tool, *once that pilot's infrastructure is operating*.
- **Activity distribution would be handled by the standardized activity stream defined in the [Civic Activity Specification](../../ecosystem/civic-activity-spec.md).** Events from the tool would flow into the stream, and any other tool, dashboard, feed, or downstream consumer would be able to read them. The tool would not need to build webhooks or notification systems for every adopter — it would emit standard Civic Activities and they would reach the rest of the ecosystem, *once the activity feed infrastructure is in place*.
- **You retain your independence.** Your tool keeps its own infrastructure, codebase, brand, business model, and roadmap. The plugin contract is the shared interface; the implementation behind it is yours. This is explicitly not absorption into a single civic platform — it is the opposite of that.

In short: **the Civic Process Plugin framework is being designed to make civic-tech tools widely installable across jurisdictional and community deployment surfaces, with identity and activity infrastructure shared rather than rebuilt per adopter.** When the supporting infrastructure is in place, the design intent is that a tool plugs in once and reaches the whole ecosystem. Whether and when that future actually arrives depends on the supporting pilots being funded and executed — none of it is guaranteed by the Civic Process Plugin Specification alone.

---

## What you'd build

To be installable as a Civic Process Plugin, your tool needs three things:

1. **A plugin manifest** declaring your tool's identity, version, the Civic Process type(s) it provides (per the [Civic Process Specification](../../ecosystem/civic-process-spec.md)), the trust tier it runs as, and the capabilities it needs from the host (which activity types it emits, what host state it reads or writes, which external domains it calls, which identity claims it sees). The manifest is JSON; the capability declaration schema is defined in the [Civic Plugin Architecture](../../ecosystem/civic-plugin-architecture-spec.md) §4.1 and will be formalized further through the pilot.

2. **An identity-handoff seam.** When a citizen arrives at your tool through a host, they arrive with a verified identity established by the host's identity layer. Your tool consumes that identity through a documented seam — it does not run its own account system, login, or user database. Tools with an existing native identity model (Polis's XID, for example) map the host-provided identity into their internal identifier space at the boundary.

3. **An activity-emission seam.** Each meaningful action and lifecycle transition your tool produces is emitted as a standardized Civic Activity (per the [Civic Activity Specification](../../ecosystem/civic-activity-spec.md)). The activity model is designed to be representable as ActivityStreams 2.0, which gives downstream consumers a clean forward path to ActivityPub federation as the ecosystem matures.

That is the contract. Beyond that, your tool keeps its own UI, data model, business logic, and operational stack. The plugin specification is intentionally minimal about everything else.

---

## How your tool fits — the three trust tiers

The Civic Process Plugin specification defines three trust tiers based on where your tool's code is actually executed. Most existing civic-tech tools fit Tier 3.

- **Tier 1 — In-process plugin.** Your tool is implemented as code that runs inside the host's own program via a handler registry. Fast, full access, simple. Best fit for small, scoped, new tools written specifically for this ecosystem (the pilot's `civic.vote` and `civic.proposal` are Tier 1). Probably not the right tier for an existing standalone civic-tech application.

- **Tier 2 — Sandboxed plugin.** Your tool ships as a sandboxed module (likely WebAssembly-based) that the host loads and runs under capability restrictions. Best fit for third-party plugins that hosts want to install without granting full access. The sandbox runtime is deliberately deferred in the current pilot — it is the right architecture for future ecosystem growth but not yet built, and the plugin architecture (§5a) makes the boundary normative: no third-party plugin code runs in production civic processes before the sandbox exists for its tier.

- **Tier 3 — External-service plugin.** Your tool runs on its own infrastructure — your servers, your stack, your database. The host integrates with your tool over a network boundary, handing off identity at one side and receiving back Civic Activities at the other. **This is the right fit for any substantial existing civic-tech tool.** Polis is the pilot's reference Tier 3 plugin and the worked example.

A Tier 3 plugin keeps your operational footprint entirely your own. The plugin contract just defines how the host and your tool talk to each other — codified as the Tier-3 external-service contract in the [Civic Plugin Architecture](../../ecosystem/civic-plugin-architecture-spec.md) §10a: per-participant identity handoff (the XID pattern — an opaque, stable identifier per participant, never a shared service token), host lifecycle sovereignty (your service being down must not wedge the host), host-provided scheduling (don't assume a background scheduler exists), an anti-corruption adapter on the host side, and a data-ownership declaration in your manifest (what participant data you retain, and its export path).

---

## Reading guide — what to focus on in the full spec

The full [Civic Process Plugin Pilot Specification](civic-process-pilot-spec.md) is comprehensive — around 950 lines covering architecture, alignment with other standards, pilot scope, timeline, budget, risks, and an illustrative plugin catalog. As a prospective plugin author, you do not need to read all of it. Focus on:

### Highest priority — start here

- **sections 5–7** (What is a Civic Process / What is a Civic Process Plugin / Why a Plugin Model) — the framing for the whole effort. Tells you why this exists and what it's trying to solve.
- **section 11** (The Three-Tier Trust Model) — identify which tier fits your tool. Tier 3 if you have an existing service.
- **section 12** (Plugin Manifest vs Process Descriptor) — the metadata your plugin declares vs the per-instance state of running processes.
- **section 13** (Capabilities and Least Privilege) — what your plugin declares it needs (activities, state, network, identity, UI surfaces).
- **section 14** (Integration Seams — Identity and Activity) — the only two surfaces your plugin integrates through.
- **section 24** (Pilot Exemplars) — read the Polis section especially. Polis is the worked example of a Tier 3 plugin and the closest precedent for what your integration would look like.

### Medium priority — for engagement decisions

- **section 17** (Plugin Development Harness) — the conformance test suite, scaffolding, reference templates, and tool-agnostic agent-guidance file (in the `CLAUDE.md` / `AGENTS.md` style) the pilot will ship alongside the specification. Lets you produce a spec-compliant plugin paired with any modern AI coding tool in hours rather than weeks.
- **section 18** (Hosting Quality and Certification) — relevant if you would host your own Tier 3 instance and want the visibility a certification badge would provide.
- **sections 19–22** (Alignment with Existing Standards) — if your tool already participates in Metagov's Interoperable Deliberative Tools (IDT), the Decentralized Deliberation Standard (DDS), or the DelibTech Network, this cluster explains how Civic.Social's framework aligns with those efforts.
- **section 16** (Host Compatibility) — explains why plugins are universal by default and what happens when a plugin legitimately needs host-specific context (for example, a plugin that reads a representative's calendar in a Representative Space).

### Lower priority — for context

- **sections 1–4** — executive overview and strategic framing. Useful for understanding the project's mission but not action-oriented.
- **section 23, sections 25–28** — pilot scope, phases, demonstration scenarios, deliverables. Read if you are considering being one of the external plugin authors engaged during the pilot.
- **Appendix A** — illustrative landscape catalog. Useful if you want to see how Civic.Social maps the broader civic-tech tool landscape and where your tool sits.
- **section 34** (Open Questions) — the governance question is especially relevant. Civic.Social's posture is to support whichever community-group venue makes most sense for long-term spec stewardship; your input matters here.

The Status and Governance note at the top of the spec sets the framing for the whole document: this is a working draft published for community discussion, not a finished standard, and breaking changes between pre-1.0 versions are part of the iteration model.

---

## Engagement models

There is no requirement that you participate in the pilot itself to become a Civic Process Plugin. The specification is being published as a working draft for community discussion, and any tool author can implement against it independently once v0.1 is finalized.

That said, three engagement models are available:

**Community-group participation.** Engage in the open community-group conversations that shape the v0.1 → v1.0 spec evolution. Bring your tool's perspective into the discussion. No formal commitment required — just show up to the relevant meetings ([the DDS Working Group](https://github.com/dds-wg), [the Metagov IDT cohort](https://metagov.github.io/interop), [the DelibTech Network](https://www.demnext.org/projects/delibtech-network), or a dedicated Civic.Social venue as it forms). See section 34 of the full spec for the governance framing.

**External plugin-author engagement during Phase 3 of the pilot.** The pilot's typical-scope plan includes one round of structured engagement with an external plugin-author team — likely from the [Metagov IDT cohort](https://metagov.github.io/interop) — who would attempt to wrap their tool as a Civic Process Plugin against the draft specification. The lessons from that attempt feed directly into the v0.1 specification. If your tool is a fit and you have capacity, we would be interested in talking.

**Post-v0.1 plugin authorship.** Once v0.1 ships, any tool author can produce a Civic Process Plugin using the harness, conformance suite, and developer guides the pilot publishes. The plugin reaches every compliant host with no per-host integration work. This is the long-term mode and the one that scales.

---

## What we ask of a plugin author

Civic-tech tools that become Civic Process Plugins are expected to:

- Implement the plugin manifest, the identity-handoff seam, and the activity-emission seam per the specification.
- Declare capabilities honestly, against the Civic Plugin Architecture §4.1 schema. Hosts enforce declarations at the integration seams today (Stage 1 of the staged enforcement path, §5a — emission validation and identity-claim filtering); sandbox enforcement for Tier 2 follows.
- Conform to the activity model so emitted activities are designed to be representable as ActivityStreams 2.0.
- Run a proficient hosting setup if your tool is a Tier 3 (external-service) plugin. Tier 3 plugins ride on the plugin operator's infrastructure for availability, latency, throughput, and activity feed timeliness — every host that integrates your tool and every citizen who participates through it experiences your hosting quality directly, which means individual plugin operators carry real responsibility for keeping the broader ecosystem feeling healthy and performant. Optional hosting certification (section 18) is one mechanism that may emerge over time for signaling proficiency publicly, but the underlying ask is simpler than certification: run your service well, because the ecosystem's perceived quality is the sum of its plugin operators' hosting practices.
- Participate in the open community-group discussion of the spec's evolution where capacity allows.

In return, the framework is intended to give your tool a path to deployment across every compliant Civic Hub, Citizen Dashboard, Representative Space, and external civic application in the ecosystem — with shared identity, shared activity distribution, and zero per-host integration cost — *once the supporting pilots ship*. The Civic Process Plugin Specification by itself is a design proposal; the realized benefit depends on the full Civic.Social Infrastructure Program coming together.

---

## About Civic.Social

Civic.Social is a project of the Mosaic Foundation, a Virginia non-stock corporation building open, federated infrastructure for civic participation. The project addresses fragmentation in the pro-democracy technology ecosystem by providing shared infrastructure that connects and amplifies existing civic tools rather than replacing them.

The project is led by Adam Lake, Founding Steward, who has spent over fifteen years studying the intersection of decentralized technology, civic infrastructure, and healthy online communities. Civic.Social grew out of that work — a long-term effort to design federated civic infrastructure that strengthens democratic participation rather than undermining it.

For inquiries: adam@civic.social · civic.social

---

*Civic.Social — civic.social | adam@civic.social*
*Civic Process Plugin Pilot — Process Provider Brief*
