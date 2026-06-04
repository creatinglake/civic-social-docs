---
status: review
last-reviewed: 2026-06-03
owners: [adam]
version: 0.1
---

# Civic Process Plugin Pilot — Funder & Partner Brief

Civic.Social Infrastructure Program

---

## The Problem

Civic technology has a distribution problem. Hundreds of organizations have built valuable tools for democratic participation, from deliberation platforms and advisory voting systems to participatory budgeting software, public-comment tools, and citizen-assembly applications. Many of these tools are technically excellent. Yet most communities never get to use most of them.

Part of the reason is structural. Every adopter, whether a municipality, a nonprofit, or a civic organization, has to integrate each tool separately. The integration burden falls on every host. When the per-deployment cost is higher than the perceived benefit, communities stop evaluating, even when the tool is exactly right for their use case. The tool builders, in turn, can't reach the broad audience that would otherwise use these tools. And every new tool that emerges has to rebuild the same identity, hosting, and distribution scaffolding from scratch.

The civic-technology ecosystem today is a collection of standalone tools. There is no shared plugin layer that lets a tool, once built, become installable across many civic contexts without per-context integration work. Without that layer, the ecosystem cannot generate the network effects that would make civic participation easier, more discoverable, and more powerful over time.

---

## The Opportunity

A shared plugin model would do for civic-tech tools what mobile operating systems and their app stores did for software. It would provide a common framework that any compliant tool can build against once and have it work on every compliant host, with a shared distribution layer that reaches users wherever they are. No bilateral integration agreements required.

When a civic-tech tool conforms to such a framework, every compliant host can install it without writing per-host integration code. Compliant hosts include community civic hubs, citizen dashboards, representative-office software, and third-party civic applications. Citizens authenticate once with a portable civic identity and use the tool wherever they encounter it. The tool's author keeps their own infrastructure, codebase, and roadmap; the plugin framework simply enables interoperability with an entire open civic ecosystem.

This is not a proprietary platform. It is open infrastructure that any civic-technology organization can build against, with the framework itself governed through open community-group conversation alongside the related work of the Metagov Interoperable Deliberative Tools cohort, the Decentralized Deliberation Standard Working Group, and the DemocracyNext DelibTech Network.

The Civic Process Plugin Pilot will design and demonstrate the foundational version of this framework. It sits alongside the parallel [Civic Identity Pilot](../civic-identity/civic-identity-pilot-spec.md), which is designing the shared identity layer, and the [Civic Hubs Pilot](../civic-hubs/civic-hubs-pilot-spec.md), which is designing the community-operated deployment surfaces. Together these three pilots bring the ecosystem-level vision into reach.

---

## What the Pilot Will Demonstrate

The pilot will publish a v0.1 specification for the plugin framework and demonstrate it against three working plugin examples that cover the full architectural range — including productizing **Polis** (the widely-deployed deliberation tool from the Computational Democracy Project, already deployed at `polis.civic.social`) as the reference example of a substantial external civic-tech tool integrated under the framework.

Alongside the working examples, the pilot will produce a reference host implementation, a developer harness designed to work with current AI coding tools (so tool authors can produce compliant plugins quickly), worked compatibility examples with adjacent standards work (Metagov IDT, DDS), and developer and operator documentation.

The full deliverables list, success criteria, and demonstration scenarios are detailed in the [full pilot specification](civic-process-pilot-spec.md).

---

## Design Principles

**Tool-author independence.** Civic-technology organizations retain ownership of their own services, codebases, and roadmaps. The plugin contract is the shared interface; the implementation behind it is the tool author's own. This is explicitly not absorption into a single civic platform.

**Universal by default.** A plugin is portable across any compliant host. Tool authors do not need to enumerate which environments their tool supports unless their tool genuinely depends on something only certain hosts provide.

**Simple, well-defined integration surface.** Plugins integrate with hosts through a small set of well-defined seams rather than reaching into host internals. This is what allows in-house plugins and external-service plugins to look the same from the host's point of view.

**Open-standards alignment.** The framework is being designed to interoperate with existing deliberation-technology standardization — Metagov IDT, DDS, DelibTech — not replace them.

**Community-group governance.** The specification is published as a working draft for community discussion, with long-term stewardship in whichever community-group venue has the most relevant participants.

---

## Theory of Change

This pilot is the first in a series that will progressively develop, harden, and grow the Civic Process Plugin framework. The v0.1 specification, the developer harness, and three working plugin examples shipped here are the foundation. Subsequent pilots and ongoing community-group iteration evolve the framework toward v1.0 and beyond, as more plugin authors build against it and more hosts adopt it.

In the near term, this pilot demonstrates feasibility with real working examples, reduces per-plugin authoring cost through the developer harness, and produces the artifacts an external plugin-author community can pick up and build against. Plugins become installable across compliant hosts as the parallel Civic Hubs and Civic Identity pilots mature. The ecosystem's value compounds as more tools and more hosts adopt the framework. Communities gain access to a wider range of civic-participation tools than per-deployment integration ever allowed.

The longer arc is more ambitious. Once the framework is robust enough that any tool author can build against it with confidence that their plugin will be interoperable and compatible across the ecosystem, what emerges is essentially an app marketplace for civic processes. Deliberation tools, voting systems, participatory budgeting platforms, citizen-assembly software, and entirely new categories of civic participation can be discovered, installed, and used across any compliant host. Citizens gain access to the full range of civic-technology innovation through the spaces and dashboards they already use. Communities gain agency over which civic processes they host without rebuilding integration scaffolding for each. Tool authors gain a distribution channel that compounds as the ecosystem grows.

This is the foundation for a decentralized civic operating system: the connective tissue that lets identity, hubs, processes, activity, discovery, and citizen interfaces interoperate as a single coherent layer for democratic participation, owned by no one and usable by everyone. The Civic Process Plugin framework is one of several pieces of that operating system. It is a load-bearing one — the layer that turns the civic-tech landscape from a fragmented collection of tools into shared digital infrastructure for healthy democratic societies.

---

## Timeline and Budget

**Indicative duration: 6–9 months at typical scope.** The pilot scope is flexible — the duration scales from roughly 4–6 months at a lean tier to roughly 9–12 months at an expanded tier with deeper external engagement.

**Phase 1 — Specification drafting (2 months).** Draft the v0.1 plugin specification. Validate it against existing working examples. Open conversations with Metagov IDT, the DDS Working Group, and the DelibTech Network.

**Phase 2 — Reference host implementation (2–3 months).** Build the host-side plugin machinery into the Civic.Social Hub reference implementation. Bring two existing plugin examples into specification conformance. Ship the first version of the developer harness.

**Phase 3 — Polis productization and validation (1–2 months).** Productize the Polis integration as the reference external-service plugin under the specification. Demonstrate all three plugin examples in a live host environment. Validate cross-host portability against a second host environment.

**Phase 4 — Specification finalization and community seeding (1–2 months).** Finalize the v0.1 specification based on what the work taught us. Publish the compatibility examples and developer guides. Seed the community-group conversation through outreach to adjacent standards venues.

The [full pilot specification](civic-process-pilot-spec.md) includes detailed phase deliverables.

---

## Existing Momentum

The Civic Process Plugin Pilot builds on infrastructure and relationships already in development:

**Working plugin examples.** Two of the three pilot examples are already operational in the Civic.Social Hub reference implementation. The third — Polis — is already deployed at `polis.civic.social` and integrated into the Civic.Social representative-space service. The pilot's work is primarily formalization, host-machinery development, and standards-alignment rather than greenfield plugin construction.

**Open-standards adjacencies in active development.** Three major standardization efforts — Metagov's Interoperable Deliberative Tools cohort, the Decentralized Deliberation Standard Working Group, and the DemocracyNext / AI & Democracy Foundation DelibTech Network — are publishing protocols and convening practitioners right now. The plugin framework is being designed to align with these efforts.

**Broader Civic.Social program.** This pilot is one component of a broader infrastructure program that also includes pilots for [Civic Identity](../civic-identity/civic-identity-pilot-spec.md), [Civic Hubs](../civic-hubs/civic-hubs-pilot-spec.md), the Civic Activity Feed, Civic Credentialing, and the Citizen Dashboard. The plugin framework is the layer that lets civic-tech tools install themselves across the rest of the ecosystem; co-funding with one or both of the Civic Identity and Civic Hubs pilots accelerates the end-to-end demonstration.

---

## What Comes Next

The pilot produces artifacts that support a clear transition from v0.1 working draft to broader community adoption — the specification, the developer harness, the reference host, the worked compatibility examples, and the documentation, all published openly with a public CHANGELOG and contribution guide for transparent evolution.

After the pilot, the path forward includes onboarding external plugin authors, formalizing community-group governance for the specification's evolution toward v1.0, and folding new alignment work as the adjacent standards mature. The open-standards foundation ensures the infrastructure remains viable regardless of which organizations operate it long-term.

The pilot is designed with known risks and mitigations — including specification drift from real third-party plugin needs, the operational fragility of upstream open-source dependencies like Polis, and the deliberate gap between capability declaration and runtime enforcement. The [full specification](civic-process-pilot-spec.md) details each.

---

## The Funding Ask

Civic.Social is seeking **$100,000–$175,000** to fund the Civic Process Plugin Pilot over 6–9 months at typical scope. The budget range is genuinely flexible. A lean tier of $50,000–$75,000 produces the core specification, the reference host machinery, the formalized plugin examples, and a tool-agnostic developer harness that lets plugin authors rapidly build spec-compliant civic processes with any AI coding tool. An expanded tier of $200,000–$300,000 adds an additional engineer, sustained engagement with the IDT, DDS, and DelibTech communities (including travel and convenings), multiple cross-host validations, and structured engagement with external plugin-author teams to seed an actual community of plugin authors.

The lower budget envelope compared to other pilots in the Civic.Social program reflects three things: two of the three plugin examples are already operational, the third (Polis) is already deployed, and the pilot is primarily formalization and standards-alignment work rather than greenfield construction. This investment will produce the specification, the reference implementation, the developer harness, and the alignment artifacts needed to turn civic-technology tools into installable, portable infrastructure across the broader Civic.Social ecosystem and the adjacent deliberation-technology community.

---

## About Civic.Social

Civic.Social is a project of the Mosaic Foundation, a Virginia non-stock corporation building open, federated infrastructure for civic participation. The project addresses fragmentation in the pro-democracy technology ecosystem by providing shared infrastructure — civic identity, civic hubs, civic process plugins, and a civic activity feed — that connects and amplifies existing civic tools rather than replacing them.

The project is led by Adam Lake, Founding Steward, who has spent over fifteen years studying the intersection of decentralized technology, civic infrastructure, and healthy online communities. Civic.Social grew out of that work — a long-term effort to design federated civic infrastructure that strengthens democratic participation rather than undermining it.

For inquiries: adam@civic.social · civic.social

---

## For Ecosystem Partners

Civic-technology organizations, tool authors, standards bodies, and adjacent community groups can participate in the pilot in several ways:

**As a process provider** — make your civic-participation tool a Civic Process Plugin. See the [Process Provider Brief](process-provider-brief.md) for what's in it for you, what you'd need to build, and a reading guide for the longer specification.

**As an external plugin-author partner during Phase 3** — the pilot's typical-scope plan includes structured engagement with an external plugin-author team (likely from the [Metagov IDT cohort](https://metagov.github.io/interop)) whose attempt to wrap their tool against the draft specification feeds directly into v0.1.

**As a community-group governance venue** — the long-term stewardship of the specification is an open question. [The DDS Working Group](https://github.com/dds-wg), [the Metagov IDT cohort](https://metagov.github.io/interop), [the DelibTech Network](https://www.demnext.org/projects/delibtech-network), and a dedicated Civic.Social venue are all plausible homes.

**As a standards body or consultant** — the framework is being designed to align with W3C decentralized identity standards, ActivityPub / ActivityStreams 2.0, and adjacent deliberation-technology standardization. Standards practitioners with experience in plugin architecture or federated protocols are welcome to consult.

The [full Civic Process Plugin Pilot Specification](civic-process-pilot-spec.md) is the canonical reference for everything described here.

---

*Civic.Social — civic.social | adam@civic.social*
*Civic Process Plugin Pilot — Funder & Partner Brief*
