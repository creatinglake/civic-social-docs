---
status: review
last-reviewed: 2026-05-29
owners: [adam]
version: 0.2
---

# Civic Process Plugin Pilot

Civic.Social Infrastructure Program

> **Status and governance.** This specification is a **working draft published for community discussion** — not a final standard. The Civic Process Plugin Specification will be developed and maintained through open community-group meetings, with initial drafts contributed by Civic.Social and ongoing refinement coordinated with the deliberation-technology and open-standards communities (Metagov, DemocracyNext / DelibTech, the DDS Working Group, and others as the appropriate governance venue is established — see section 34). Breaking changes between pre-1.0 versions are expected and welcomed as part of that conversation; the v0.1 → v1.0 evolution happens openly, in public, with the people who plan to build on it.

---

## Table of Contents

### [How to Read This Document](#how-to-read-this-document-1)

### Executive Overview
1. [Executive Summary](#1-executive-summary)
2. [Purpose of the Pilot](#2-purpose-of-the-pilot)
3. [Relationship to the Civic.Social Pilot Program](#3-relationship-to-the-civicsocial-pilot-program)
4. [Strategic Importance](#4-strategic-importance)

### Strategic Context
5. [What is a Civic Process](#5-what-is-a-civic-process)
6. [What is a Civic Process Plugin](#6-what-is-a-civic-process-plugin)
7. [Why a Plugin Model for Civic Processes](#7-why-a-plugin-model-for-civic-processes)
8. [Process Surfaces — Where Plugins Appear](#8-process-surfaces--where-plugins-appear)
9. [Federated vs Embedded Processes](#9-federated-vs-embedded-processes)
10. [Architectural Role of the Plugin Framework](#10-architectural-role-of-the-plugin-framework)

### Plugin Architecture
11. [The Three-Tier Trust Model](#11-the-three-tier-trust-model)
12. [Plugin Manifest vs Process Descriptor](#12-plugin-manifest-vs-process-descriptor)
13. [Capabilities and Least Privilege](#13-capabilities-and-least-privilege)
14. [Integration Seams — Identity and Activity](#14-integration-seams--identity-and-activity)
15. [Versioning and Contract Compatibility](#15-versioning-and-contract-compatibility)
16. [Host Compatibility](#16-host-compatibility)
17. [Plugin Development Harness](#17-plugin-development-harness)
18. [Hosting Quality and Certification for External Service Plugins](#18-hosting-quality-and-certification-for-external-service-plugins)

### Alignment with Existing Standards and Initiatives
19. [Why This Matters](#19-why-this-matters)
20. [Metagov Interoperable Deliberative Tools (IDT)](#20-metagov-interoperable-deliberative-tools-idt)
21. [Decentralized Deliberation Standard (DDS)](#21-decentralized-deliberation-standard-dds)
22. [Deliberation & Technology (DelibTech) Network](#22-deliberation--technology-delibtech-network)

### Pilot Implementation
23. [Minimum Viable Pilot Scope](#23-minimum-viable-pilot-scope)
24. [Pilot Exemplars — Vote, Proposal, Polis](#24-pilot-exemplars--vote-proposal-polis)
25. [Pilot Phases and Timeline](#25-pilot-phases-and-timeline)
26. [Pilot Demonstration Scenarios](#26-pilot-demonstration-scenarios)
27. [Success and Validation Criteria](#27-success-and-validation-criteria)
28. [Expected Deliverables](#28-expected-deliverables)

### Ecosystem and Partnerships
29. [Relationship to Other Civic.Social Pilots](#29-relationship-to-other-civicsocial-pilots)
30. [Estimated Development Effort and Team Roles](#30-estimated-development-effort-and-team-roles)
31. [Potential Pilot Partners](#31-potential-pilot-partners)
32. [Estimated Budget](#32-estimated-budget)

### Pilot Plan
33. [Risks and Mitigations](#33-risks-and-mitigations)
34. [Open Questions for Further Design](#34-open-questions-for-further-design)

### Conclusion and Future Work
35. [Conclusion](#35-conclusion)

### Appendix
- [Appendix A — Civic Process Plugin Catalog (Illustrative)](#appendix-a--civic-process-plugin-catalog-illustrative)

---

<a id="how-to-read-this-document-1"></a>
## How to Read This Document

This document is the canonical specification for the Civic Process Plugin Pilot. It is written for multiple audiences, and not every reader needs to read every section.

**Funders and program evaluators** should focus on the Executive Overview (sections 1–4), the Success and Validation Criteria (section 27), and the Pilot Plan (sections 30–34). These sections explain why a plugin framework for civic processes matters, how the pilot will be measured, and what it will cost.

**Vendors and technical implementers** should focus on the Plugin Architecture (sections 11–16), the Pilot Exemplars (section 24), and the Expected Deliverables (section 28). These sections define the technical specification that every Civic Process Plugin must meet and the three concrete exemplars the pilot will deliver against.

**Ecosystem partners** — civic technology platforms, deliberation tool developers, standards bodies, and host environment maintainers — should focus on the Strategic Context (sections 5–10), the Alignment with Existing Standards section (sections 19–22), and the Relationship to Other Civic.Social Pilots (section 29). These sections describe how the plugin framework fits the broader deliberation-technology landscape and how external tools can participate.

**All readers** benefit from the Executive Summary and the definitions of *Civic Process* (section 5) and *Civic Process Plugin* (section 6), which establish the two foundational concepts the rest of the document is built around.

The document is intentionally comprehensive. Sections build on each other, but each group can be read independently depending on the reader's needs.

This pilot specification refers throughout to three companion documents in the Civic.Social ecosystem folder:

- **[Civic Process Specification](../../ecosystem/civic-process-spec.md)** — the protocol-level definition of a Civic Process: lifecycle, identity requirements, event contracts, and action contracts.
- **[Civic Plugin Architecture](../../ecosystem/civic-plugin-architecture-spec.md)** — the working architectural draft describing how processes are *packaged, trusted, scoped, and installed* as plugins, including the three-tier trust model, manifest, and capability layer.
- **[Civic Activity Specification](../../ecosystem/civic-event-spec.md)** — the activity/event distribution layer that every plugin must conform to.

The pilot does not redefine what those documents already cover. It refers to them, validates them through working exemplars, and surfaces what they need to make stronger.

---

<a id="1-executive-summary"></a>
## 1. Executive Summary

The Civic Process Plugin Pilot will design, prototype, and validate the plugin framework that allows independently-built civic participation tools — advisory voting, proposal systems, deliberation platforms, participatory budgeting, citizen assemblies — to interoperate across the Civic.Social ecosystem and to be embedded across many kinds of host environments.

Civic technology today is structurally fragmented. Hundreds of organizations have built valuable tools for democratic participation, but those tools rarely interoperate. Each tool brings its own identity system, its own data model, its own interface, and its own discovery mechanism. From a citizen's perspective, participation requires navigating disconnected platforms with separate accounts. From a host's perspective — a municipality, a civic hub operator, a representative's office — embedding a new participation tool means a bespoke integration every time.

The Civic Process Plugin Framework addresses this fragmentation by defining a common specification that any civic participation tool can implement. A Civic Process Plugin declares a manifest, conforms to a defined process lifecycle, integrates with shared decentralized identity, and emits standardized Civic Activities. Any compliant host environment — a Civic Hub, a citizen dashboard, a representative space, an external website — can install the plugin and run it under a uniform integration boundary.

The pilot will deliver three concrete plugin exemplars spanning the full architectural range:

- **`civic.vote`** — an in-process plugin implementing advisory voting, already running in the Civic.Social Hub reference implementation.
- **`civic.proposal`** — an in-process plugin implementing proposal submission, which spawns a vote process, already running in the Civic.Social Hub reference implementation.
- **Polis** — an external-service plugin wrapping Polis, the widely-deployed deliberation tool from the Computational Democracy Project. A live Polis instance is already deployed and integrated; the pilot will productize that integration as a Tier 3 plugin under the full plugin specification.

These two classes — *in-process* (the host runs the plugin's code directly) and *external service* (the host launches a separate application and exchanges identity and activities with it across a defined boundary) — cover the realistic range of how a civic participation tool gets integrated. The plugin framework's design is that both classes look the same to the host, because they integrate through the same two seams: identity handoff and activity emission. Section 11 describes the full three-tier trust model in detail, including a sandboxed middle tier that is deliberately deferred in this pilot.

The pilot will also produce the Civic Process Plugin Specification: the formal specification a plugin must meet to be installable in a compliant host. This is the primary public artifact of the pilot.

Critically, the pilot is designed for honest alignment with existing standardization work in the deliberation-technology ecosystem. The Civic Process Descriptor and the Civic Activity Feed together already satisfy the flatfile interoperability requirement defined by Metagov's Interoperable Deliberative Tools (IDT) project. The pilot will document this alignment formally and explore deeper compatibility with the Decentralized Deliberation Standard (DDS) and the Deliberation & Technology (DelibTech) Network. Civic.Social is not attempting to replace these efforts; it is building infrastructure designed to interoperate with them.

### Why This Matters Now

Democratic participation increasingly depends on digital tools, yet those tools cannot talk to each other. The cost of that fragmentation has become visible enough that the deliberation-technology field is now actively trying to solve it: a 2024–2026 wave of standardization work — Metagov's Interoperable Deliberative Tools project, the Decentralized Deliberation Standard, the DemocracyNext / AI & Democracy Foundation DelibTech Network — has emerged precisely because the field has collectively recognized that interoperability is the missing layer.

The window is open. Metagov's IDT cohort of fourteen funded teams is implementing the first flatfile interoperability requirement right now. The DDS Working Group is publishing its protocol as a working draft, with the opportunity for outside contributors to shape it before it hardens. The DelibTech Network is convening practitioners for the first time around shared protocols. The Civic Process Plugin Pilot is the Civic.Social contribution to that effort: a working plugin framework that demonstrates the model in practice, provides a concrete integration target for the rest of the ecosystem, and meets the standardization wave with a complementary architecture that is honestly aligned with what the field is already building.

---

<a id="2-purpose-of-the-pilot"></a>
## 2. Purpose of the Pilot

The Civic Process Plugin Pilot defines what it means to be a Civic Process, demonstrates how a Civic Process becomes a Plugin, and proves that Civic Process Plugins can be installed and run across multiple kinds of host environments.

The pilot is foundational, not platform-specific. It is the work that turns the [Civic Process Specification](../../ecosystem/civic-process-spec.md) and the [Civic Plugin Architecture](../../ecosystem/civic-plugin-architecture-spec.md) into shippable infrastructure: a reference implementation of the framework, three plugin exemplars that exercise it, and the public specification that any future plugin author can build against.

The pilot is explicitly *not* the Civic Hubs Pilot in disguise. Civic Hubs are one important host environment for plugins, but they are not the only one. Plugins must also be embeddable in citizen dashboards, in representative spaces, on government websites, and inside third-party civic applications. The pilot validates the plugin framework against this broader surface — beginning with Civic Hubs but designed from the outset to be host-agnostic.

---

<a id="3-relationship-to-the-civicsocial-pilot-program"></a>
## 3. Relationship to the Civic.Social Pilot Program

The Civic Process Plugin Pilot is one component of the broader Civic.Social Infrastructure Program. It operates alongside several complementary pilot initiatives.

**Civic Identity Pilot** — defines the decentralized identity layer that plugins integrate with for authentication, credential verification, and participation eligibility. Civic Process Plugins consume identity through this layer; they never manage their own accounts.

**Civic Hubs Pilot** — defines community-operated digital civic spaces that host civic processes. Civic Hubs are the primary, but not the only, host environment for Civic Process Plugins. The Civic Hubs Pilot exercises the plugin framework from the host side; this pilot defines and validates the framework itself.

**Civic Activity Feed Pilot** — defines how civic participation events are aggregated and distributed across the ecosystem. Civic Process Plugins emit Civic Activities that flow into this feed.

**Civic Credentialing & Profiles Pilot** — defines how civic badges and credentials are issued, displayed, and verified. Some plugins may issue or consume civic credentials as part of participation.

**Citizen Dashboard** — a personal civic interface that aggregates participation opportunities. Dashboards can themselves host or surface plugin interfaces directly to citizens.

These pilots can each be run independently — a preferred sequencing is described in section 29, but no pilot is a hard prerequisite for another. The Civic Process Plugin Pilot can be funded and executed on its own track, and its outputs make every other pilot easier to deliver.

---

<a id="4-strategic-importance"></a>
## 4. Strategic Importance

The civic technology ecosystem is highly fragmented. Hundreds of organizations have built valuable tools for democratic participation — deliberation platforms, advisory voting tools, participatory budgeting systems, citizen assembly software, public comment platforms, petition tools. These tools rarely interoperate.

From a citizen's perspective, participation requires navigating many disconnected platforms with separate accounts, different interfaces, and no shared discovery mechanism. From a host organization's perspective — a city government, a civic nonprofit, a community hub — adding a new participation tool means a custom integration: a new login system, a new data export, a new way for citizens to find the activity.

This fragmentation produces structural problems that no individual platform can solve on its own. Citizens face friction at every transition between tools. Civic participation opportunities have low visibility because there is no shared distribution layer. The civic-tech ecosystem lacks network effects: a participant on one platform brings nothing to the next. And organizations rebuild common infrastructure — identity, activity feeds, embed widgets — over and over.

A plugin framework addresses this directly. When a civic participation tool conforms to a shared plugin specification, three things become possible at once. The tool can be installed across many host environments without per-host integration work. Its activity flows into a shared distribution layer, increasing visibility for civic participation opportunities across the ecosystem. And citizens authenticate once, with a portable civic identity, and carry that identity across every plugin they encounter.

The plugin framework is therefore not a feature of the Civic.Social platform; it is *the* interoperability layer of the ecosystem. It is what turns a collection of independent civic tools into a coherent participation infrastructure. Without it, every other pilot in the program is solving a narrower problem than it should be.

---

<a id="5-what-is-a-civic-process"></a>
## 5. What is a Civic Process

A **Civic Process** is a structured, stateful civic interaction that enables participation between citizens, organizations, and institutions, under defined rules and eligibility requirements. The [Civic Process Specification](../../ecosystem/civic-process-spec.md) defines the protocol-level model in full; this section summarizes the concept for readers of the pilot document.

Examples of Civic Processes include:

- advisory voting on a community decision
- proposal submission and amendment
- participatory budgeting
- citizen assemblies and structured deliberation
- petition gathering
- public consultation on a policy or rule
- legislative testimony collection
- public-comment periods

Every Civic Process has a lifecycle (`draft → scheduled → active → closed → finalized`), one or more participation actions with defined input and output contracts, identity and credential requirements, and a published descriptor that other systems can read to discover and integrate with the process. Every action and lifecycle transition emits at least one Civic Activity, so external observers can reconstruct the process state by reading its activity history.

A Civic Process is the underlying *service*. It is not a user interface, not a discovery feed, and not an identity system. It is the structured interaction itself. How that interaction is presented to citizens — through which interface, in which host environment, on which device — is a separate concern handled by the host and, where applicable, by the plugin's declared UI surfaces.

---

<a id="6-what-is-a-civic-process-plugin"></a>
## 6. What is a Civic Process Plugin

A **Civic Process Plugin** is a packaged, installable implementation of one or more Civic Processes that can run inside any compliant host environment. The [Civic Plugin Architecture](../../ecosystem/civic-plugin-architecture-spec.md) is the working draft for the plugin model; this section summarizes its key ideas.

Where the Civic Process Specification defines *what a process is*, the Civic Plugin Architecture defines *the layer above that* — how processes get packaged, trusted, scoped, and installed across hosts. The two specs are intentionally separate and intentionally composable: any process that conforms to the process spec can in principle become a plugin, and the plugin layer is what makes that process portable.

A Civic Process Plugin is defined by:

- A **manifest** that declares the plugin's identity, version, the process type(s) it provides, the trust tier it runs as, the capabilities it requests, any host-specific requirements (optional — plugins are universal by default), and the contract version it targets.
- A **process descriptor** (or descriptor template) for each instance the plugin can create.
- A clear declaration of which **trust tier** the plugin runs as: in-process, sandboxed, or external service.
- Integration through the host's **identity and activity seams** rather than direct access to host internals.

The same plugin specification supports very different implementations: a small first-party voting handler running in the same process as the host; a sandboxed third-party WebAssembly module; or a full external application like Polis, with its own database and infrastructure, exchanging identity handoffs and activity events with the host across a defined boundary. This range — and the discipline of treating all three uniformly from the host's point of view — is the central design move of the plugin framework.

---

<a id="7-why-a-plugin-model-for-civic-processes"></a>
## 7. Why a Plugin Model for Civic Processes

Modern digital ecosystems scale through modular architectures. Plugin systems, APIs, and shared protocols allow independent services to interoperate across many environments. Democratic-participation technologies have not yet reached this level of interoperability; most operate as stand-alone systems with their own identity, interface, and discovery mechanisms.

A plugin model addresses this directly. In the plugin model, civic-tool organizations build **processes**, not entire platforms. Hosts — Civic Hubs, dashboards, representative spaces, external websites — enable processes as installable plugins. Citizens participate across many processes using shared identity and a shared activity layer.

The benefits compound across the ecosystem. Civic-tech organizations lower their barrier to reach: they build a process once and it runs anywhere a compliant host exists. Hosts gain a library of pre-integrated participation tools without bespoke integration work. Citizens get a single civic identity that works across every plugin they encounter, and discover participation opportunities through a shared activity feed instead of needing to know every platform individually.

Crucially, the plugin model preserves independence. Civic-tool organizations retain ownership of their own services, data, and roadmaps. They are not absorbed into a single platform; they ship a plugin and continue to evolve their underlying tool on their own terms. The plugin specification is the shared interface; the implementation behind it is the plugin author's own.

The plugin model is also a deliberate alternative to the *centralization* model that has shaped most consumer software: rather than building a single civic participation platform that owns the citizen relationship, the plugin framework creates **shared digital infrastructure that lets many independent tools cohere into one ecosystem**. This is the principle that distinguishes Civic.Social from a civic-tech platform play.

---

<a id="8-process-surfaces--where-plugins-appear"></a>
## 8. Process Surfaces — Where Plugins Appear

A Civic Process Plugin is portable across multiple kinds of host environments. The plugin framework deliberately does not assume that processes live only inside Civic Hubs. A plugin can appear as:

- **A process running inside a Civic Hub** — for example, an advisory vote a municipality runs for its residents.
- **A module inside a Citizen Dashboard** — for example, a "participate in this consultation" panel inside a citizen's personal civic interface.
- **An embedded widget inside a Representative Space** — for example, a constituent-feedback process embedded in a representative's office software (an active surface in the Civic.Social ecosystem, currently running in production).
- **An embedded widget on an external organization's website** — for example, a deliberation tool embedded in a nonprofit's campaign page.
- **An entry from a Civic Activity Feed** — when a citizen discovers a participation opportunity through their feed and follows it directly into the plugin.
- **A direct deep link** — when a process is shared by URL through email, messaging, or other channels.

In all cases, the *process* is the underlying service; the surface is the interface. A plugin is universal by default — the same plugin code can appear across any of these surfaces, because the integration contract (identity handoff + activity emission) is uniform across host types. A plugin author may declare host-specific requirements when the plugin genuinely depends on context only certain hosts provide (see section 16), but the expected case is that a single plugin runs everywhere a compliant host exists. This is more permissive than WordPress's "this plugin works with X" model, and it is the right default for an interoperability framework whose whole point is portability.

---

<a id="9-federated-vs-embedded-processes"></a>
## 9. Federated vs Embedded Processes

All three trust tiers are equally supported in the framework, but they cluster into two useful conceptual groupings — *federated* and *embedded* — based on where the plugin's code actually runs. Neither model is preferred over the other; both have produced working exemplars in this pilot.

In the **federated model** (Tier 3), the civic-tool organization operates its own service, with its own infrastructure, database, and operational model. Host environments integrate that service through standardized descriptors, identity handoffs, and activity events; they do not embed the tool's code at all. Polis is the canonical example. This is the realistic integration model for any substantial existing tool whose codebase, identity model, and operational footprint already exist outside the Civic.Social ecosystem.

In the **embedded model** (Tier 1 and Tier 2), the plugin's code runs inside the host's process boundary — either directly (Tier 1, for first-party code the host fully trusts) or inside a sandbox (Tier 2, for code the host wants to run but does not fully trust). Embedded execution is faster and simpler for plugin authors but places more responsibility on the host's trust enforcement. `civic.vote` and `civic.proposal` are the embedded exemplars in this pilot.

The plugin manifest declares which tier the plugin runs as, and the host enforces the trust boundary appropriate to that tier. Federated execution is the natural fit for civic-tech tools that already have their own operational identity (and is the only realistic option when the tool's codebase cannot be embedded in the host). Embedded execution is the natural fit for new process types that are small, scoped, and built specifically against the Civic Process Plugin Specification. The framework supports both equally precisely so the ecosystem can absorb both new tools and existing tools without forcing either into the wrong shape.

---

<a id="10-architectural-role-of-the-plugin-framework"></a>
## 10. Architectural Role of the Plugin Framework

The Civic.Social architecture is organized around **four canonical open specifications** — the Civic Identity Spec, the Civic Hub Spec, the Civic Process Spec, and the Civic Activity Spec — each of which extends underlying open web standards (W3C DIDs and Verifiable Credentials, ActivityPub, ActivityStreams) into a civic-participation context. Together those four specs form the interoperability foundation that the rest of the ecosystem — citizen dashboards, activity feeds, discovery interfaces, representative spaces, and any number of third-party civic applications — builds on as interfaces and downstream tools, not as foundational components. The plugin framework defined in this document operates at the **Civic Process Spec** layer: it is the work that turns the Civic Process Spec into installable, portable, interoperable participation tools.

The plugin framework sits between the Civic Process Spec (which defines what a process *is*) and the host environments that run processes (Civic Hubs, citizen dashboards, representative spaces, external embeds). It is the translation layer that lets a single process implementation run across many hosts without per-host integration work. It is also the integration point with the Civic Identity Spec (plugins consume identity from the shared layer rather than managing their own) and with the Civic Activity Spec (plugins emit activities into the shared distribution layer).

The plugin framework is a keystone of the Civic.Social interoperability story. The Civic Identity Spec makes citizens portable across the ecosystem; the Civic Activity Spec makes participation visible everywhere it happens; the Civic Hub Spec makes the spaces where participation occurs interchangeable; and the plugin framework — built on the Civic Process Spec — makes the *precesses themselves* portable. Each of the other three canonical specs reaches its full value only when plugins are also standardized: without portable plugins, the Civic Identity Spec becomes single-sign-on to a small number of disconnected tools rather than a credential that works across an open ecosystem; the Civic Activity Spec becomes aggregation across siloed sources rather than a coherent stream of participation; and the Civic Hub Spec gives communities interchangeable hubs that still require bespoke per-process integration to host anything inside them.

### The Framework at a Glance

The plugin framework sits between host environments at the top of the stack and the shared Civic.Social layers at the bottom, with the plugin itself in the middle. The pieces:

**Host environments** — the places a Civic Process Plugin can be installed: Civic Hubs, Representative Spaces, Citizen Dashboards, external websites and embeds. Any compliant host can install any compatible plugin.

**Civic Process Plugin** — the unit that gets installed. Each plugin carries two things:

- A **manifest** declaring its identity, version, trust tier, requested capabilities, and any host-specific requirements (optional — plugins are universal by default).
- The actual **process logic** — the lifecycle, action handlers, and descriptor of the civic process it implements.

**Trust tier** — declared in the plugin's manifest and enforced by the host:

- **Tier 1 — In-process.** First-party code the host runs directly. Current exemplars: `civic.vote`, `civic.proposal`.
- **Tier 2 — Sandboxed.** Third-party code the host runs inside a WebAssembly-style sandbox. Deliberately deferred in this pilot.
- **Tier 3 — External service.** A separate application the host integrates with across a network boundary. Current exemplar: Polis.

**Shared Civic.Social layers** — what the plugin connects to through its two integration seams:

- **Civic Identity** (DIDs and Verifiable Credentials). The host authenticates participants through this layer; the plugin receives identity through the **identity seam**.
- **Civic Activity** (the standardized activity stream and distribution layer). The host emits activities into this layer; the plugin contributes to it through the **activity seam**.

A plugin only ever touches the host through those two seams — identity in, activity out. That uniformity is what allows a Tier 1 in-process handler and a Tier 3 external service to look the same from the host's point of view.

---

<a id="11-the-three-tier-trust-model"></a>
## 11. The Three-Tier Trust Model

*The architecture described in sections 11–18 is the v0.1 proposal — the starting point this pilot publishes for community discussion. The specific shapes (capability classes, manifest fields, trust tier names) are likely to evolve through community-group feedback during the v0.1 → v1.0 conversation; the structural commitments (least privilege, identity-and-activity-only integration, universal-by-default plugins, opt-in host requirements) are the durable proposals.*

A Civic Process Plugin is not one kind of thing. Different plugins live in different places depending on how much the host trusts the plugin's code and where that code actually runs. The plugin architecture proposes three tiers covering this range — from "the host wrote this plugin itself and runs it directly" at one end, through "the host runs the plugin but inside a sandbox that limits what it can do," to "the plugin is a separate application running on its own and the host just talks to it over a network" at the other end. Every plugin declares which tier it runs as; the host enforces the boundary appropriate to that tier.

The single guiding principle is **least privilege**: every plugin is assumed untrusted by default and receives only the access it explicitly declares and the host explicitly grants.

**Tier 1 — In-process handler.** First-party code that the host has written and vetted, running directly inside the host's own program. Fast, full access, simple — the plugin is effectively a built-in feature of the host that the host happens to expose through the plugin contract. The current `civic.vote` and `civic.proposal` plugins in the Civic.Social Hub reference implementation are Tier 1. Most early experimental plugins will be Tier 1, and that is appropriate — but the architecture must not assume everything is Tier 1.

**Tier 2 — Sandboxed plugin.** Code the host wants to run but does not fully trust — typically a third-party plugin built by someone other than the host operator. The plugin runs inside a sandbox (likely a WebAssembly runtime such as Extism) that limits what it can read, write, and call out to. The sandbox runtime is **deliberately deferred** in this pilot — it is the right architecture for future third-party plugins, but it is premature to build before third parties want to ship plugins we did not write.

**Tier 3 — External service plugin.** A whole separate application running on its own infrastructure, with its own stack and database. The host does not embed the plugin's code at all; it launches the external service, hands off the citizen's identity at the boundary, and receives results back as Civic Activities. Polis is the canonical example — a full Clojure/Postgres/React application that the host integrates with rather than ingests. The pilot will productize Polis as the first Tier 3 plugin under the full specification.

The Civic Process Specification's required endpoints (`GET /process/:id`, `POST /process/:id/action`) describe a Tier 1 process talking the host's own interface. Tier 3 plugins will not speak that natively — they integrate through the identity-handoff and activity-emission seams instead. The plugin framework is built to accommodate this asymmetry: the *external* interface any host expects is identity-and-activity; the *internal* interface a process speaks is its own.

---

<a id="12-plugin-manifest-vs-process-descriptor"></a>
## 12. Plugin Manifest vs Process Descriptor

A clean conceptual separation is essential here, because the existing [Civic Process Specification](../../ecosystem/civic-process-spec.md) section 6.1 describes a process *descriptor*, which is sometimes confused with a plugin *manifest*. They are different objects answering different questions.

The **process descriptor** describes a **running instance**. It is the answer to "what is this specific process right now?" For example: *"Library Expansion Vote, process-123, currently active, requires resident credential, accepts the `submit_vote` action."* The descriptor exists only after a process instance has been created. The Civic Process Specification covers this fully.

The **plugin manifest** describes the **installable type**. It is the answer to "what is this plugin and what does it need?" — before any instance exists. A manifest declares:

- the plugin's stable identity, human name, and version
- which Civic Process type(s) it provides (e.g. `civic.vote`)
- any host-specific requirements it has (optional — plugins are universal by default; this field is only present when the plugin genuinely needs context that only certain host types provide)
- which trust tier it runs as (Tier 1, 2, or 3)
- the capabilities it requests (see section 13)
- the version of the process / action / activity contracts it was built against (see section 15)

The manifest is what a host inspects when deciding whether to make a plugin available; the descriptor is what a host (or a citizen) inspects when interacting with a specific instance of that plugin. The pilot will formalize the manifest schema as part of the Civic Process Plugin Specification.

---

<a id="13-capabilities-and-least-privilege"></a>
## 13. Capabilities and Least Privilege

The single most important addition the plugin layer makes over the bare Civic Process Specification is **capability declaration**.

The process spec is thorough about what *participants* are allowed to do — which credentials a citizen needs to take part. That is eligibility, and it is about people. The process spec says nothing about what the *plugin code itself* is allowed to do. These are two different questions, and the second one is the entire "don't become a WordPress security nightmare" concern: WordPress plugins run with near-total access to the host, and the integrity of a civic vote or a structured deliberation cannot tolerate that.

So a Civic Process Plugin declares the capabilities it needs, and the host grants them narrowly. The relevant capability classes are:

- **Activities** — which activity types the plugin may emit and which it may listen to.
- **State** — which process state it may read and which it may write.
- **Network** — which external domains, if any, it may call out to. A Tier 3 plugin like Polis needs this; an in-process voting plugin should not.
- **Identity** — which identity claims about a participant it may see. A plugin should see the minimum needed to do its job, not the participant's full credential set.
- **UI surfaces** — where in the host interface it may render.

The pilot will define the capability schema and require every plugin manifest to declare its capabilities, even in advance of an enforcement engine. The declaration itself is the discipline: it forces every plugin to be honest about its blast radius, and it sets up the data the eventual enforcement layer will use. **Build the declarations now; defer the enforcement until it is needed.**

---

<a id="14-integration-seams--identity-and-activity"></a>
## 14. Integration Seams — Identity and Activity

Two integration seams are already well-defined in the Civic Process Specification, and the plugin framework treats them as the *only* surfaces a plugin may integrate through (irrespective of trust tier).

**Identity (section 3 of the process spec).** Participants arrive at a plugin with a DID-based identity and verifiable credentials, established by the host's identity layer. The plugin receives identity through this seam; it never runs its own account system, its own login, or its own user database. Tier 3 external-service plugins that have their own native identity systems (such as Polis's XID) are integrated by mapping the host-provided identity into the plugin's internal identifier space at the boundary — never by asking the participant to re-authenticate.

**Activity (section 5 of the process spec, plus the full Civic Activity Specification).** Every action and lifecycle transition emits a standardized Civic Activity through the single activity-emission path. Plugins coordinate by emitting and listening to activities, not by calling each other directly. A Tier 1 plugin emits activities natively into the host's emitter. A Tier 3 plugin emits activities by posting them across the host-plugin boundary, which the host then forwards into its emitter. Either way, downstream consumers — the Civic Activity Feed, dashboards, other plugins, other hubs — see a single coherent activity stream.

**ActivityStreams 2.0 representability is a design goal.** Every Civic Activity emitted by a plugin is designed to be representable as an ActivityStreams 2.0 (AS2) activity per the mapping defined in the [Civic Activity Specification](../../ecosystem/civic-event-spec.md) section 9. This forward-compatibility design lets the Civic.Social activity layer bridge to native ActivityPub federation in a later phase without breaking existing plugins or hosts. Native ActivityPub publishing is not required of plugins or hosts in v0.1, and whether to promote AS2 representability from "design goal" to "hard requirement" is itself a community-discussion question — the v0.1 spec is designed to make the AS2 mapping achievable but does not yet mandate it. The plugin development harness (section 17) can include an AS2 mapping check as one of its conformance probes if the community decides it should be required.

This discipline is what makes a Tier 1 in-process handler and a Tier 3 external service look the same from the host's point of view, and what makes the activity feed a trustworthy source-of-truth for civic participation. The pilot will validate it across all three exemplar plugins.

---

<a id="15-versioning-and-contract-compatibility"></a>
## 15. Versioning and Contract Compatibility

The process, action, activity, and descriptor contracts are effectively the public API that plugins are written against. They need versions, and the plugin manifest must declare which contract version it targets.

When a host installs a plugin, it inspects the manifest's declared contract version and compares it against its own supported versions. If the plugin targets a contract version the host cannot satisfy, the host may refuse the plugin, run it in a degraded mode, or flag it for the operator's attention. Without this discipline, every change to the underlying contracts risks silently breaking every plugin in the ecosystem.

The pilot will formalize the versioning model and ship the first version-tagged contracts (`civic-process` v0.1, `civic-activity` v0.1, manifest v0.1) as part of the Civic Process Plugin Specification.

---

<a id="16-host-compatibility"></a>
## 16. Host Compatibility

A Civic Process Plugin is **universal by default**: it works in any compliant host environment. A plugin's manifest does not need to enumerate the host types it supports, and a host may install any plugin that satisfies its capability requirements. The same `civic.vote` plugin runs unchanged in a Civic Hub, a Representative Space, a Citizen Dashboard, or an external embed — that portability is the entire reason the plugin model exists.

A plugin author *may* declare host-specific requirements when the plugin genuinely depends on something only certain hosts provide — for example, access to a host-specific UI surface, host-provided context such as a representative's calendar, or trust assumptions that only certain host types satisfy. When such requirements are declared, the host enforces them at install time and refuses installations into hosts that cannot satisfy them. The default — no host requirements declared — means "this plugin works anywhere a compliant host exists."

This framing is intentional. Asking every plugin author to enumerate the host types where their plugin can run would invert the model's central promise. The expected case is universal compatibility; the constrained case is the rare exception that the plugin author opts into.

The pilot does not need to build multi-host rendering machinery up front — that becomes relevant only when plugins with genuine host-specific surface requirements emerge. What the pilot does need to build is the *opt-in declaration mechanism* — so a plugin that genuinely needs host-specific context can say so, and a host can accept or refuse accordingly. The default behavior the pilot's three exemplars exercise is universal compatibility: `civic.vote`, `civic.proposal`, and Polis all run in any compliant host without declared host requirements.

---

<a id="17-plugin-development-harness"></a>
## 17. Plugin Development Harness

A specification alone is rarely enough to make plugin authorship easy. The Civic Process Plugin Pilot will produce, alongside the specification itself, a **plugin development harness** — a set of files and tools designed to sit alongside any AI coding tool (Claude Code, Cursor, Aider, GitHub Copilot, or any future agent-style developer environment) and help a developer produce a spec-compliant Civic Process Plugin.

The harness is not an AI. It is the structured set of project artifacts that AI coding tools work best against:

- **A conformance test suite** — machine-runnable tests exercising every required behavior in the Civic Process Plugin Specification (manifest validation, capability declaration parsing, identity-seam integration, activity emission contract). Any AI coding tool can run the suite against a work-in-progress plugin and use the failures to drive the next round of edits.
- **A scaffolding tool** — a generator that produces a working Civic Process Plugin skeleton (manifest, basic action handler, activity emission stubs) that a developer, with or without AI assistance, can extend.
- **Reference plugin templates** — minimal working plugins (similar in shape to `civic.vote`) that demonstrate idiomatic implementations and can serve as starting points or worked examples.
- **Tool-agnostic agent guidance** — a project-level guidance file in the spirit of `CLAUDE.md` or `AGENTS.md` that any AI coding tool can read to learn the spec's invariants, the conformance expectations, and the integration seams. This guidance file is the harness's central artifact: it is what enables an AI coding tool to produce spec-compliant output without the developer having to re-explain the architecture every session.

The motivation is specific to 2026. Software is increasingly written with AI assistance, and AI coding tools work best when paired with a clear, testable specification and a working harness to validate against. A plugin author paired with the Civic Process Plugin Specification, the conformance test suite, and an AI coding tool can produce a compliant plugin in hours rather than weeks — and the resulting plugin is more likely to be correct because the AI runs the conformance suite as part of its own development loop.

The harness is itself a public artifact of the pilot, intended to live in the open reference-implementation repository alongside the specification. It is **tool-agnostic by design**: any AI coding tool — current or future — that can read project guidance files and run shell commands can use it. As new agent-style developer tools emerge, the harness should remain useful without modification.

The strategic bet is straightforward. Interoperability without good tooling produces specifications nobody implements. Interoperability with a tool-agnostic harness — especially in an era of AI-assisted development — produces an ecosystem of compliant plugins faster than the historical civic-tech baseline would suggest is possible.

---

<a id="18-hosting-quality-and-certification-for-external-service-plugins"></a>
## 18. Hosting Quality and Certification for External Service Plugins

External-service (Tier 3) plugins introduce a quality dimension that in-process (Tier 1) plugins do not have. When a host environment installs a Tier 3 plugin, it is depending on the plugin operator's infrastructure for availability, latency, throughput, and feed timeliness. A Polis plugin hosted on a well-maintained server feels fast and reliable; the same plugin hosted on an overloaded or under-resourced server degrades the experience for every host that integrates it — and, by extension, for every citizen who participates through it.

The framework needs a way to surface these quality differences without gatekeeping the ecosystem. The proposed mechanism is **optional hosting certification** for Tier 3 plugin operators.

A certification — issued initially by Civic.Social and, over time, potentially by third-party certifiers — would attest that a specific Tier 3 plugin hosting instance meets defined standards for:

- **Availability** — uptime over a defined window
- **Latency** — response times for plugin actions and activity emissions
- **Throughput** — capacity for participation under realistic load
- **Activity feed timeliness** — how quickly emitted activities reach downstream consumers
- **Operational practices** — backups, incident response, security hygiene, data export

Certified hosting instances would carry a visible signal — a verification badge in any Civic Process Plugin discovery interface or library, for example — that helps host operators choose well when integrating a Tier 3 plugin. **Certification is not gatekeeping.** Uncertified plugins remain installable; certification is a quality signal layered over the spec, not a barrier to participation. Over time, distinct certification *classes* may emerge (for example "Basic," "Production," and "Civic-grade") corresponding to different reliability thresholds appropriate to different participation contexts — a neighborhood deliberation hub may be fine with a Basic-tier Polis instance, while a binding municipal vote may want Civic-grade.

The certification program itself is **not a Phase 1 pilot deliverable**. The pilot will *scope and document* the program — what it certifies, who issues certifications, what the visible signals look like, what classes are appropriate — as part of the Civic Process Plugin Specification. Actually building and operating the certification program is a follow-on undertaking that depends on the first wave of Tier 3 plugins reaching production and on broader ecosystem governance input on who should run the program long-term. The pilot's Polis Tier 3 exemplar will serve as the reference case study for what the standards should reasonably require.

The broader point is that ecosystem performance is itself an interoperability concern. A plugin specification can guarantee that any compliant plugin runs in any compliant host, but it cannot, on its own, guarantee that the citizen experience will be performant across host-plugin pairs. The certification layer is the framework's answer to that gap, and it is specifically calibrated to the operational realities of Tier 3 plugins where the framework has the least direct control over runtime quality.

---

<a id="19-why-this-matters"></a>
## 19. Why This Matters

A 2024–2025 wave of standardization work has emerged in the deliberation-technology field, driven by the field's collective recognition that interoperability is the missing layer. Three efforts are most directly relevant to the Civic Process Plugin framework: Metagov's Interoperable Deliberative Tools (IDT) project, the Decentralized Deliberation Standard (DDS), and the Deliberation & Technology (DelibTech) Network.

These efforts are not redundant with the Civic.Social plugin framework; they are *complementary*. Each addresses a slightly different layer of the same problem. Where Civic.Social provides a plugin framework that makes participation tools installable across host environments, Metagov IDT provides a flatfile data-interop standard that lets tools exchange deliberation data; DDS provides a deliberation-specific protocol on top of AT Protocol; and DelibTech provides a practitioner network coordinating the field.

Civic.Social's approach is to design the plugin framework to be **honestly compatible with these efforts wherever compatibility is achievable without compromising the framework's own architectural commitments**. We are not trying to subsume these standards; we are trying to make sure a Civic Process Plugin can participate in their ecosystems naturally. The following three sections describe the alignment posture for each.

---

<a id="20-metagov-interoperable-deliberative-tools-idt"></a>
## 20. Metagov Interoperable Deliberative Tools (IDT)

Metagov's Interoperable Deliberative Tools project is the most concrete and most directly compatible standardization effort in this space. Coordinated by Eugene Leventhal, advised by Aviv Ovadya, Amy Zhang, Josh Tan, Colin Megill, and Liz Barry, the project funded approximately fourteen deliberation tool teams (including Decidim, Stanford PB, Pol.is-adjacent tools, and MAPLE) to implement a common interoperability requirement: every tool must publish its deliberation data in a flatfile format (JSON, JSON-LD, or CSV) and accept that format as input.

The IDT specification is deliberately minimal. It does not prescribe a schema; it prescribes the *capability* — every participating tool can export its data in a readable format and import data from another tool's export. The goal is not a single canonical data model but practical bottom-up interop between existing tools.

The Civic Process Plugin framework already satisfies this requirement structurally, and understanding *how* matters — because IDT's minimalism is intentional and is the same strategic bet the Civic Process Plugin Specification is making. Two artifacts the framework produces — the Civic Process Descriptor and the Civic Activity Feed — together constitute a complete flatfile representation of any process the framework hosts.

The **Civic Process Descriptor** (defined in the [Civic Process Specification](../../ecosystem/civic-process-spec.md) section 6.1) is a JSON object describing what a process *is*: its type, title, jurisdiction, lifecycle configuration, action schemas, and eligibility requirements. Every process publishes its descriptor at `GET /process/:id`; the hub's full process catalog is available at `GET /process`. This is the static, structural side of the export.

The **Civic Activity Feed** (defined in the [Civic Activity Specification](../../ecosystem/civic-event-spec.md)) is a stream of JSON activity objects covering every action and every lifecycle transition that occurred — votes, comments, proposals, process state changes — each with attribution, timestamps, and a namespaced payload. The feed is available at `GET /events`, filterable by `process_id` for per-process slices. This is the dynamic, participation-record side of the export.

Together, descriptor + activity feed contain everything a downstream consumer needs to replay the process in another tool, analyze the deliberation outside the host, or archive it as a permanent record. All JSON. All on documented HTTP endpoints. All with stable, versioned schemas published in the underlying spec documents. The export side of IDT's requirement is unambiguously satisfied today; the import side is structurally compatible — the descriptor is already importable via `POST /process`, and an activity-feed import endpoint will be added during the pilot to fully close the round-trip.

The conceptual key is **schema neutrality**. IDT does not require Civic.Social's schemas to match Decidim's, Pol.is's, MAPLE's, or anyone else's. It requires every participating tool to publish *its own* data in a documented flatfile format that another tool can read. Schema mapping between tools is then a pairwise concern that any researcher, integrator, or downstream tool can solve once both sides expose documented flatfiles. We do not need to change our data model to satisfy IDT — we need to demonstrate that our endpoints produce IDT-acceptable flatfiles and that our system can ingest a corresponding flatfile in return.

This is also why IDT's minimalism is the right strategic bet, and why the Civic Process Plugin Specification deliberately adopts the same posture. A common deliberation schema across every civic-tech tool is decade-long standards work, and waiting for it would hold up the deployment of civic infrastructure that communities in the United States and around the world need now. IDT's "publish a flatfile, accept a flatfile" requirement is a low-cost, fast-to-implement bar that unblocks practical interoperability today while deeper schema work continues in parallel. The Civic Process Plugin framework runs on the same two timelines: ship a working interoperability capability immediately, contribute to longer-term schema standardization as it matures, and let the two run alongside each other rather than gating one on the other.

The pilot will:

- formalize this alignment in the Civic Process Plugin Specification, documenting how the descriptor + activity feed maps to IDT's flatfile requirement
- include a worked example showing a Civic Process Plugin exporting its full state in IDT-compatible JSON and re-importing it
- where the IDT specification calls for fields the Civic Process model does not have, evaluate them as candidate additions for the next spec revision (without compromising Civic.Social's architectural commitments)
- engage with the IDT working group to surface where the two specifications can converge

The opportunity here is significant. Of the three efforts in this section, IDT is the one where compatibility is most achievable with the least architectural disruption — and the one where a public declaration of compatibility carries the most ecosystem weight. The IDT cohort includes tools (Decidim, MAPLE) that are already plausible plugin targets for the Civic.Social framework. Compatibility is the bridge to those communities.

---

<a id="21-decentralized-deliberation-standard-dds"></a>
## 21. Decentralized Deliberation Standard (DDS)

The Decentralized Deliberation Standard is the most architecturally ambitious effort in the deliberation-interop space. Authored by Nicolas Gimenez (ZKorum, Agora Citizen Network) and published as a working draft on 2026-01-13 by the DDS Working Group under MPL-2.0, DDS defines an open protocol for sovereign, verifiable, interoperable, and resilient deliberation. The spec is hosted at `dds.xyz/spec/dds-protocol` with companion Design Rationale, Anonymity, and Implementation addenda.

DDS uses a hybrid three-layer architecture: **AT Protocol** as the transport and identity layer (Personal Data Servers, the Firehose, AppViews, and the Lexicon schema system); **Arweave / Filecoin / Logos Storage** as an optional archival layer for walkaway recovery; and **Ethereum** as an optional verification layer for tamper-evident result commitment. The Lexicon system — AT Protocol's PDS-enforced, machine-readable schema definitions, essentially JSON Schemas that the network itself refuses to accept invalid records against — is the heart of DDS's interoperability story, not the transport alone. DDS organizes its lexicons in two layers: **base lexicons** (`org.dds.identity.*`, `org.dds.auth.*`, `org.dds.org.*`, `org.dds.ref.*`) define shared primitives every DDS component uses, and **product lexicons** (`org.dds.module.polis`, `org.dds.module.sense`, `org.dds.module.survey`, `org.dds.module.vote`, `org.dds.result.pca`, `org.dds.result.summary`) define specific consultation types and analysis outputs. Cross-app interoperability happens because any tool can read another tool's product lexicons off the Firehose and reference them explicitly via `org.dds.ref.*`. This is a substantially more structured interop story than Metagov IDT's "publish a flatfile, accept a flatfile" approach — DDS prescribes the schemas as well as the capability.

DDS organizes deliberation as an iterative cycle of three phases: **Plan** (the Organizer designs the consultation, including round structure, modules, and eligibility), **Collect** (a deliberation platform gathers participant input under one or more product lexicons), and **Analyze** (an Analyzer Agent reads collected data off the Firehose, runs analysis, and publishes a result lexicon). The lifecycle is many-to-many: multiple Analyzers can process the same collected data with different algorithms, and analysis from one round can trigger the next. This is structurally richer than — but conceptually adjacent to — the linear Civic Process lifecycle (`draft → scheduled → active → closed → finalized`). A direct mapping is achievable, but the two specifications abstract similar territory in different shapes; the mapping work will need to handle the iterative-vs-linear difference deliberately.

DDS defines an **Analyzer Protocol** with three trust levels — Reputation (trust the Analyzer's track record), Spot Check (any party can independently re-run the public computation against the public Firehose data), and Trustless (cryptographic proof, marked as future work pending zkML maturity) — plus optional on-chain result commitment via Ethereum hash for tamper-evidence. The Civic Process Plugin framework does not currently have a direct analogue. Analysis in Civic.Social is left to the plugin author, and the host treats it as part of the plugin's internal logic. This is a deliberate scope difference: DDS is a deliberation-domain protocol, where rigorous post-hoc analysis (clustering, sensemaking, summarization) is core; the Civic Process Plugin Specification is a more general civic-participation framework that has to cover votes, proposals, petitions, and public comment where the "analysis" is often as simple as a tally. For deliberation processes specifically, the DDS Analyzer Protocol is the relevant precedent — and worth evaluating as a possible extension to the plugin framework for processes that need verifiable computation.

DDS organizes participant identity around **four Participant Identity Levels** (Level 0 Identified; Level 1 Pseudonymous, the default; Level 2 Anonymous ZK-verified persistent; Level 3 Anonymous ZK-verified per-deliberation), plus an orthogonal Guest mode for unverified or soft-verified ephemeral participation. The DDS identity model aligns naturally with the Civic Identity Pilot's three-dimensional policy model (assurance, disclosure, context). A direct mapping between DDS Identity Levels and Civic Identity's disclosure presets is achievable and worth formalizing as part of the alignment work — it is one of the cleanest places where Civic.Social and DDS can converge without either side compromising.

**Terminology collision worth disambiguating up front.** DDS uses the word *Tier* in a way that overlaps with — but does not match — the Civic Process Plugin Specification's use. DDS's **Hosting Tiers** describe where a user's Personal Data Server lives (Tier 1 Managed, where an app auto-provisions a PDS; Tier 2 Self-Hosted, where the user brings their own). This is about the user's identity infrastructure. The Civic Process Plugin Specification's **Trust Tiers** describe how a plugin's code is executed inside a host (Tier 1 In-process, Tier 2 Sandboxed, Tier 3 External service). This is about how the host runs the plugin. The two tier systems are completely orthogonal — a DDS Hosting Tier 1 user (managed PDS) could participate in a process hosted by a Civic Process Plugin Trust Tier 3 plugin (external service). When both specifications are in scope at once, qualify the term to avoid confusion.

**Polis is a notable cross-spec coincidence.** DDS's first and canonical product lexicon is `org.dds.module.polis`, with `opinion` and `vote` record types modeling Polis-style deliberation. The Civic Process Plugin Pilot's Tier 3 exemplar productizes the same Polis system from the other direction. This is an opportunity, not a conflict: the pilot's Polis plugin can emit data in `org.dds.module.polis` shape as part of its Civic Activity payload, satisfying both specifications at once for Polis-mediated deliberations. This is the most concrete and most achievable DDS alignment the pilot can ship.

The pilot will:

- document a structural mapping between DDS Plan / Collect / Analyze and the Civic Process lifecycle, and between DDS product lexicons and Civic Activity payloads
- formalize the alignment between DDS Participant Identity Levels and the Civic Identity Pilot's disclosure dimensions
- explore whether the Polis Tier 3 plugin can emit data in `org.dds.module.polis` shape, satisfying both specifications for Polis-mediated deliberations
- engage with the DDS Working Group — and with Nicolas Gimenez / ZKorum directly — to surface where the two specifications can converge and where they should remain distinct
- include DDS compatibility as a permanent open question in the Civic Process Plugin Specification, to be re-evaluated as DDS itself matures

**One real consideration on transport.** DDS's choice of AT Protocol as the required transport and identity foundation is well-reasoned: AT Protocol's PDS-based identity model is one of the most coherent walkaway-capable designs in the federated-identity space, and the Lexicon system is more structured than ActivityPub's vocabulary. But AT Protocol federation outside Bluesky is still maturing, the Personal Data Server ecosystem is small, and the Lexicon system requires every participating tool to run on or bridge to AT Protocol. Civic.Social currently targets ActivityPub forward-compatibility (see the [Civic Activity Specification](../../ecosystem/civic-event-spec.md) section 9) precisely because ActivityPub has a longer track record as an open federation protocol and a broader implementation base across civic-tech tools (Bonfire, Mastodon, and Decidim's emerging support, among others). The two transport choices are not strictly incompatible — a Civic Process Plugin could in principle publish to both — but the pilot will not commit Civic.Social to AT Protocol as a required transport layer. Where Civic.Social and DDS can align is at the **data model** level (lifecycle phases, identity levels, the Polis lexicon shape) more than at the **transport** level.

**On-chain result commitment is out of scope for v0.1 but a potential future direction.** DDS's optional Ethereum-anchored result commitment provides tamper-evident archival of analysis results, which matters for high-stakes processes like binding municipal votes where the integrity of the result must outlive any single operator. The Civic Process Plugin Specification has no equivalent in v0.1 — the activity feed is the auditable record but is not cryptographically anchored to any external chain. For processes where this matters, the DDS commitment protocol is the right precedent to evaluate as a future optional addition.

The DDS Working Group is still in working-draft phase, and the spec itself names lexicon formalization, guest identity, zkML feasibility, and chain selection among its open questions. This is the window for Civic.Social to engage and shape the specification toward civic-scale use cases — jurisdiction-level deliberation, verified civic identity integration, hub-to-hub deliberation events.

---

<a id="22-deliberation--technology-delibtech-network"></a>
## 22. Deliberation & Technology (DelibTech) Network

The Deliberation & Technology (DelibTech) Network, launched in September 2025 by DemocracyNext and the AI & Democracy Foundation, is an invite-only international network of technologists, practitioners, and experts building, studying, and using technology for deliberative democracy. Co-leads include Claudia Chwalisz, Sammy McKinney, Aviv Ovadya, and Kyle Redman. The network's three workstreams include guiding technological innovation, developing principles and protocols for deploying emerging technologies in deliberation, and supporting strategic collaboration between members.

DelibTech has not, as of this writing, published a formal interoperability specification, and the network's stated mode is convening and coordination rather than standards publication. There is therefore nothing concrete for the Civic Process Plugin framework to conform to at the specification level today.

The Civic.Social posture toward DelibTech is alignment of vocabulary, values, and intent. The Civic Process Plugin Pilot will:

- track DelibTech's published principles and protocols as they emerge, and surface any that have implications for the plugin framework
- name DelibTech as an aligned community in the Civic Process Plugin Specification
- where appropriate, explore conversations with network co-leads about how the Civic Process Plugin framework relates to the network's protocol work

Notably, there is meaningful person-overlap between DelibTech and Metagov IDT (Aviv Ovadya is a co-lead of DelibTech and an advisor to IDT; Amy Zhang is an advisor to IDT and a member of DelibTech). The deliberation-interop community is small and tightly connected, and alignment with one effort tends to imply alignment with the others.

---

<a id="23-minimum-viable-pilot-scope"></a>
## 23. Minimum Viable Pilot Scope

This section defines the boundary between what the pilot will build and demonstrate and what remains part of the broader architectural vision for future phases.

### What the Pilot Will Demonstrate

The pilot will demonstrate an end-to-end plugin lifecycle:

1. A plugin author writes a Civic Process Plugin conforming to the Civic Process Plugin Specification.
2. The plugin declares its manifest, including its trust tier, requested capabilities, any host-specific requirements (optional — universal by default), and targeted contract version.
3. A host environment (a Civic Hub in the primary demo; a representative space in a secondary demo) inspects the manifest and installs the plugin.
4. The plugin runs in its declared trust tier, integrating only through the host's identity and activity seams.
5. A citizen, authenticated through the Civic Identity layer (or through a stub identity adapter in early phases), participates in an instance of the plugin's process type.
6. The plugin emits standardized Civic Activities — designed to be representable as ActivityStreams 2.0 per the section 14 design goal — that flow into the Civic Activity Feed and are visible to downstream consumers.
7. A Tier 3 external-service plugin (Polis) demonstrates the same lifecycle with a real external civic-tech tool.

This loop demonstrates the core value proposition: any compliant plugin can run in any compliant host, under a single uniform integration interface.

### What is In Scope

The pilot will produce:

- **Civic Process Plugin Specification.** A formal specification defining the plugin manifest, the capability schema, the trust tiers, the integration seams, the versioning model, and the conformance requirements. This is the primary public artifact of the pilot.
- **Reference plugin host implementation.** The host-side machinery — manifest inspection, capability granting, identity handoff, activity ingestion — built into the Civic.Social Hub reference implementation. The Civic Hubs Pilot will consume this; this pilot delivers it.
- **Three plugin exemplars** (see section 24) covering Tier 1 in-process (`civic.vote`, `civic.proposal`) and Tier 3 external service (Polis).
- **Worked IDT-compatibility example** demonstrating that the Civic Process Plugin descriptor + activity feed satisfies Metagov IDT's flatfile interoperability requirement.
- **DDS structural mapping document** describing the alignment between the Civic Process lifecycle and DDS's Plan/Collect/Analyze model, and between Civic Activities and DDS deliberation events.
- **Plugin Development Harness (v0.1)** — a tool-agnostic set of conformance tests, scaffolding, reference plugin templates, and an `AGENTS.md`-style guidance file that any AI coding tool can read to help a developer produce a spec-compliant plugin. See section 17.
- **Hosting certification program design** — scoped standards and discovery signals for optional Tier 3 plugin operator certification, published as part of the specification. Operating the program is a follow-on undertaking and is not a Phase 1 deliverable. See section 18.
- **Plugin author developer guide** describing how to write a Civic Process Plugin against the specification.
- **Host operator integration guide** describing how to integrate the plugin framework into a host environment.

### What is Explicitly Out of Scope

The following are part of the architectural vision but are not deliverables of this pilot:

- **Tier 2 sandbox runtime.** The WebAssembly / Extism sandbox is deliberately deferred. Tier 1 and Tier 3 cover the full architectural range the pilot needs to validate; Tier 2 is the right architecture for future third-party plugins and will be built when third parties want to ship plugins we did not write.
- **Capability enforcement engine.** The pilot requires every plugin to *declare* its capabilities and validates that hosts can read and reason about those declarations. Actual enforcement (e.g. blocking a plugin's network call to an undeclared domain) is deferred. Build the declarations now; defer the enforcement until it is needed.
- **Multi-host rendering machinery for plugins with host-specific requirements.** Most plugins are universal and run unchanged across any compliant host. A small minority will genuinely need host-specific context — for example, a constituent-feedback widget in a Representative Space that reads the representative's calendar to surface upcoming town halls (a Civic Hub has no equivalent surface), or a plugin that renders into a hub's announcement banner (which a Citizen Dashboard does not have). The pilot validates the *opt-in declaration mechanism* — a plugin that needs host-specific context can declare it, and a host can read and enforce that declaration at install time. The full runtime that routes such plugins to host-specific surfaces and gives them access to host-specific context is deferred until real host-specific plugins emerge to drive the requirements; the universal-by-default case (which all three pilot exemplars fit) requires nothing additional.
- **The processes themselves.** This pilot defines the *framework* that civic processes plug into; building the full library of civic process types (participatory budgeting, citizen assemblies, public consultations beyond the three exemplars) is downstream work. Some of that work is already underway in parallel — `civic.vote` and `civic.proposal` are operational and Polis is already deployed — but additional process types are out of scope here.
- **Hosting the plugins at production scale.** This pilot produces working prototypes that validate the framework, not production-scale plugin hosting infrastructure.

### Prototype vs Production Intent

The pilot produces working prototypes that demonstrate the viability of the plugin framework. The plugin specification, the manifest schema, the capability classes, the trust-tier definitions, and the integration interfaces produced during the pilot are intended to serve as the foundation for production development. The goal is to prove the model, validate it against three exemplar plugins spanning the full tier range, and produce a public specification any future plugin author can build against.

---

<a id="24-pilot-exemplars--vote-proposal-polis"></a>
## 24. Pilot Exemplars — Vote, Proposal, Polis

The pilot will deliver three concrete plugin exemplars, each chosen to exercise a different region of the plugin specification.

### Exemplar 1 — `civic.vote` (Tier 1, In-Process)

An advisory voting plugin already running in the Civic.Social Hub reference implementation as a Tier 1 in-process handler. The pilot will formalize its manifest, capability declaration, and contract versioning to bring it into full Civic Process Plugin Specification conformance. `civic.vote` is the simplest viable plugin and serves as the canonical Tier 1 reference implementation.

### Exemplar 2 — `civic.proposal` (Tier 1, In-Process)

A proposal-submission plugin already running in the Civic.Social Hub reference implementation. `civic.proposal` is more architecturally interesting than `civic.vote` because a proposal action spawns a `civic.vote` process — exercising the plugin framework's ability to handle composition between process types. The pilot will formalize its manifest and capability declarations, and use it to validate the cross-plugin coordination model (one plugin emitting activities that another plugin's host listens for).

### Exemplar 3 — Polis (Tier 3, External Service)

Polis is the widely-deployed deliberative-survey tool from the Computational Democracy Project, used for structured group sensemaking across hundreds of public deliberations. It is also the canonical *hard case* for the plugin architecture: a full external application with its own stack (Clojure backend, Postgres, React frontend, math worker), its own native identity model (XID), and a substantial existing codebase that cannot be embedded in-process.

A self-hosted Polis instance is already deployed live at `polis.civic.social` and integrated into the representative-space service. The pilot will productize that integration as the first Tier 3 plugin under the full Civic Process Plugin Specification, exercising:

- the identity-handoff seam (Civic Identity ↔ Polis XID)
- the activity-emission seam (Polis conversation events ↔ Civic Activities)
- the capability declaration model for external-service plugins (network access, identity claims, UI surfaces for the embedded conversation)
- the operational realities of integrating with a substantial external tool whose roadmap we do not control

Polis was selected for this role both because it is architecturally the right test case — if the plugin framework can absorb Polis, it can absorb most other external civic-tech tools — and because Polis is a widely-respected reference implementation in the deliberation field, giving the integration ecosystem weight beyond the technical demonstration.

Together, the three exemplars exercise the full plugin specification: in-process plugins (Vote), in-process plugins with cross-plugin coordination (Proposal), and external-service plugins (Polis). What the pilot learns from each will flow back into the next revision of the Civic Process Plugin Specification.

---

<a id="25-pilot-phases-and-timeline"></a>
## 25. Pilot Phases and Timeline

**Indicative duration: 6–9 months at typical scope.** The pilot scope is flexible, and the timeline scales with it — see section 32 for the three indicative scope tiers (lean ≈4–6 months, typical ≈6–9 months, expanded ≈9–12 months). The phase breakdown below describes the typical-scope plan; lean and expanded variants compress or extend the same phases proportionally.

The overall timeline is moderated by the fact that two of the three exemplar plugins (`civic.vote`, `civic.proposal`) are already running in the Civic.Social Hub reference implementation, and the third (Polis) is already deployed live at `polis.civic.social`. The pilot's work is therefore primarily formalization, host-machinery development, and standards-alignment — not greenfield plugin construction.

### Phase 1 — Specification Drafting and Architecture Validation (2 months)

Draft the first complete version of the Civic Process Plugin Specification, including the manifest schema, capability classes, trust-tier definitions, integration seams, and versioning model. Validate the proposed specification against the two existing Tier 1 exemplars (`civic.vote`, `civic.proposal`) and the live Polis deployment. Open conversations with Metagov IDT and the DDS Working Group on alignment questions.

Key deliverables: Civic Process Plugin Specification v0.1 draft, IDT alignment document, DDS structural mapping draft.

### Phase 2 — Reference Plugin Host Implementation (2–3 months)

Build the host-side plugin machinery into the Civic.Social Hub reference implementation: manifest inspection, capability granting, identity handoff, and activity ingestion. Bring the two Tier 1 exemplars into full specification conformance — primarily by formalizing their manifests and capability declarations against the spec. Begin host-side integration for the Tier 3 (Polis) exemplar, building on the existing live deployment.

Key deliverables: reference plugin host code, `civic.vote` and `civic.proposal` upgraded to full specification conformance, Polis integration host scaffolding, and the first part of the Plugin Development Harness (conformance test suite and scaffolding tool).

### Phase 3 — Polis Productization and Multi-Tier Validation (1–2 months)

Productize the Polis integration as the first Tier 3 plugin against the full Civic Process Plugin Specification, building on the existing live deployment. Demonstrate the full plugin lifecycle for all three exemplars in a live host environment. Validate that the same plugin framework runs cleanly across a second host environment — the representative-space service, which is already operating in production, is the natural candidate.

Key deliverables: Polis Tier 3 plugin in full specification conformance, multi-tier demonstration environment, cross-host portability proof, and Plugin Development Harness completion (reference plugin templates and tool-agnostic agent-guidance file).

### Phase 4 — Specification Finalization, Community Seeding, and Handoff (1–2 months)

Finalize the v0.1 Civic Process Plugin Specification based on what the exemplar work taught us, while keeping the document framed as a working draft open to revision. Write the plugin author developer guide and the host operator integration guide. Publish the IDT-compatibility worked example and the DDS structural mapping. Scope and document the Tier 3 hosting certification program (what it certifies, who issues, certification classes, discovery signals) as part of the specification. Prepare the public release and seed the community-group conversation — initial outreach to the DDS Working Group, the Metagov IDT cohort, and the DelibTech Network, and publication of a public CHANGELOG and contribution guide so the spec can evolve transparently from v0.1 onward in whichever community-group venue takes up its stewardship (see section 34).

Key deliverables: v0.1 Civic Process Plugin Specification (community-draft release), plugin author guide, host operator guide, IDT alignment example, DDS mapping document, hosting certification program design, public CHANGELOG and contribution guide, community-group seeding outreach completed.

---

<a id="26-pilot-demonstration-scenarios"></a>
## 26. Pilot Demonstration Scenarios

### Scenario A — In-Process Plugin in a Civic Hub

A community operates a Civic Hub on the Civic.Social Hub reference implementation. The hub administrator installs the `civic.vote` plugin from the local plugin registry. The plugin's manifest declares Tier 1, no network capability, and a single `civic.process.vote_submitted` activity. The host inspects, installs, and exposes the plugin. A community member, authenticated through the Civic Identity layer, participates in an advisory vote. A `civic.process.vote_submitted` activity flows into the hub's activity feed and is visible in the Civic Activity Feed Pilot's aggregation layer. This scenario validates the simplest viable plugin lifecycle.

### Scenario B — Cross-Plugin Coordination

The same hub additionally installs the `civic.proposal` plugin. A community member submits a proposal through the proposal plugin; the proposal action emits a `civic.process.proposal_created` activity. The proposal plugin's logic then creates a downstream `civic.vote` process to ratify the proposal, validating that one plugin can compose with another through the activity layer rather than through direct calls. This scenario validates cross-plugin coordination and the activity-as-coordination-medium discipline.

### Scenario C — External-Service Plugin (Polis)

The hub installs the Polis plugin. The plugin's manifest declares Tier 3, network capability scoped to the Polis service endpoint, and identity-handoff requirements. The host launches a Polis deliberation, hands off the citizen's Civic Identity into Polis's XID system at the boundary, and lets the citizen participate in the embedded Polis conversation. As the conversation produces results, the plugin emits `civic.process.*` activities back into the hub's activity feed. This scenario validates the Tier 3 model end-to-end.

### Scenario D — Cross-Host Plugin Portability

A non-hub host environment (the representative-space prototype) installs `civic.vote`. The same plugin binary that ran inside the Civic Hub now runs inside a representative's office software, exposing a constituent advisory vote. This scenario validates the *portability* claim — the same plugin specification works across multiple host types — and is the most direct evidence that the framework is host-agnostic rather than hub-specific.

---

<a id="27-success-and-validation-criteria"></a>
## 27. Success and Validation Criteria

### Deliverable Criteria

The pilot will be considered successful upon production of:

- **A complete Civic Process Plugin Specification** covering the manifest, capability schema, trust tiers, integration seams, and versioning model, suitable for third-party plugin authors to build against.
- **Three working plugin exemplars** (`civic.vote`, `civic.proposal`, Polis) installed in the reference host, demonstrating Tier 1 and Tier 3 of the trust model.
- **A reference plugin host implementation** in the Civic.Social Hub demonstrating manifest inspection, capability granting, identity handoff, and activity ingestion.
- **A documented IDT (Interoperable Deliberative Tools) compatibility example** showing that the Civic Process Plugin descriptor + activity feed satisfies the Metagov IDT flatfile interoperability requirement.
- **A documented DDS structural mapping** between the Civic Process lifecycle and the DDS Plan/Collect/Analyze model.
- **Plugin author and host operator guides** sufficient for third-party developers to write a plugin or integrate the framework into a new host.

### Technical Validation

The pilot will validate the following through working implementations:

- **Manifest inspection.** Every exemplar plugin ships a manifest; the host successfully inspects every manifest and makes correct install / refuse decisions based on declared tier, capabilities, and contract version.
- **Capability declaration.** Every exemplar plugin declares its full capability set; the declarations are inspectable, machine-readable, and complete (no plugin takes an action outside its declared capabilities, even though enforcement is deferred).
- **Identity-seam-only integration.** No plugin (Tier 1 or Tier 3) manages its own account system. Every authenticated participant arrives at the plugin through the host's identity layer.
- **Activity emission.** Every plugin action and lifecycle transition emits a standardized Civic Activity. An external observer can reconstruct the state of any plugin instance by reading its activity history alone.
- **Cross-tier uniformity.** From the host's point of view, a Tier 1 plugin and a Tier 3 plugin look the same: same manifest shape, same identity-handoff seam, same activity-emission contract. Internal differences are invisible above the integration boundary.
- **IDT compatibility.** A working export pipeline emits Civic Process Plugin data in a format the Metagov Interoperable Deliberative Tools (IDT) specification accepts.
- **Cross-host portability.** At least one plugin runs unchanged in a second host environment.

### Future Usability Evaluation

User-facing metrics — plugin author developer experience, time-to-first-plugin, host operator integration time, citizen participation rates inside each exemplar — will be evaluated through the pilot deployments. The pilot will document findings and recommend evaluation criteria for future plugin work.

---

<a id="28-expected-deliverables"></a>
## 28. Expected Deliverables

The Civic Process Plugin Pilot is intended to produce both specification and working infrastructure.

- **Civic Process Plugin Specification (v0.1).** The formal, versioned specification covering the manifest, capabilities, trust tiers, integration seams, and contract versioning. The primary public artifact.
- **Reference plugin host implementation.** Production-quality host-side plugin machinery built into the Civic.Social Hub.
- **`civic.vote` plugin.** Tier 1 in-process exemplar, in full specification conformance.
- **`civic.proposal` plugin.** Tier 1 in-process exemplar with cross-plugin coordination, in full specification conformance.
- **Polis plugin.** Tier 3 external-service exemplar, productized from the existing live deployment.
- **IDT (Interoperable Deliberative Tools) compatibility example.** Worked demonstration showing the descriptor + activity feed satisfying Metagov IDT's flatfile interop requirement, with both export and import paths.
- **DDS structural mapping document.** Lifecycle and activity mappings between Civic Process and DDS Plan/Collect/Analyze, with open questions identified.
- **Plugin Development Harness (v0.1).** A tool-agnostic set of conformance tests, scaffolding, reference templates, and agent-guidance files (in the spirit of `CLAUDE.md` / `AGENTS.md`) that any AI coding tool can read to help a developer produce a spec-compliant plugin. See section 17.
- **Hosting certification program design.** A scoped, documented design for the optional Tier 3 hosting certification program (what it certifies, who issues, certification classes, visible discovery signal) — published as part of the Civic Process Plugin Specification. Operating the program is a follow-on undertaking and is not a Phase 1 deliverable. See section 18.
- **Plugin author developer guide.** Practical documentation for third-party developers writing new plugins.
- **Host operator integration guide.** Practical documentation for integrating the plugin framework into a new host environment.
- **Cross-host portability proof.** At least one plugin running unchanged in a second host environment (most likely the representative space).
- **Pilot report.** Written documentation of what was built, what was validated, and what the work taught us about the framework.

---

<a id="29-relationship-to-other-civicsocial-pilots"></a>
## 29. Relationship to Other Civic.Social Pilots

The Civic Process Plugin Pilot has integration points with every other pilot in the Civic.Social Infrastructure Program, but it can be funded and executed independently. There is a **preferred sequencing** that maximizes leverage between pilots, but no pilot is a hard prerequisite for any other.

### Civic Identity Pilot

Civic Process Plugins consume identity from the Civic Identity layer — DID-based authentication and verifiable credentials for participation eligibility. If the Civic Identity Pilot is running ahead of or in parallel with this pilot, plugins authenticate against real Civic Identity infrastructure. If not, plugins authenticate against a stub identity adapter designed to be swappable for the real layer without rebuilding the plugins. **Preferred sequencing: Civic Identity slightly ahead.** **Hard dependency: none** — the stub adapter is sufficient for pilot validation.

### Civic Hubs Pilot

Civic Hubs are the primary (but not only) host environment for plugins. The Civic Hubs Pilot exercises the plugin framework from the host side. **Preferred sequencing: in parallel.** **Hard dependency: none** — the Civic Process Plugin Pilot can deliver its reference host implementation independently and the Civic Hubs Pilot can later adopt it.

### Civic Activity Feed Pilot

Civic Process Plugins emit Civic Activities that flow into the Civic Activity Feed. The activity-emission seam is part of this pilot's specification; the aggregation and distribution layer above it is the Activity Feed Pilot's work. **Preferred sequencing: this pilot slightly ahead.** **Hard dependency: none** — plugins can emit activities to a local hub feed before the cross-hub aggregation layer exists.

### Civic Credentialing & Profiles Pilot

Some plugins may issue or consume civic credentials as part of participation (for example, a participation badge issued upon completing a deliberation). The credentialing layer is downstream of this pilot. **Preferred sequencing: credentialing later.** **Hard dependency: none.**

### Citizen Dashboard

Citizen Dashboards can host plugin interfaces directly. Because plugins are universal by default, a plugin written for a Civic Hub can also run inside a dashboard without modification. The dashboard work is downstream of this pilot. **Preferred sequencing: dashboard later.** **Hard dependency: none.**

The pilot program is deliberately designed so that the order in which pilots are funded does not block any individual pilot from delivering value. The Civic Process Plugin Pilot is its own work, defining what it means to be a Civic Process and making those processes portable and identity-aware across the ecosystem.

---

<a id="30-estimated-development-effort-and-team-roles"></a>
## 30. Estimated Development Effort and Team Roles

Indicative timeline: **6–9 months at typical scope** across four phases (see section 25), scaling to 4–6 months at lean scope or 9–12 months at expanded scope (see section 32 for tier definitions). The compressed range relative to other pilots reflects that two of the three exemplar plugins are already operational and the third is already deployed live; the pilot is primarily formalization, host machinery, and standards-alignment work rather than greenfield construction.

Roles required (responsibilities, not headcount):

- **Civic architecture lead.** Owns the Civic Process Plugin Specification and the alignment posture with IDT, DDS, and DelibTech.
- **Lead full-stack engineer.** Owns the reference plugin host implementation and the Tier 1 exemplar conformance work.
- **Integration engineer (Tier 3 / Polis).** Owns the productization of the Polis plugin, including the identity handoff, activity emission, and operational integration with the upstream Polis project.
- **Identity / standards advisor (part-time).** Advises on DID (Decentralized Identifier), VC (Verifiable Credential), IDT (Metagov Interoperable Deliberative Tools), and DDS (Decentralized Deliberation Standard) alignment questions.
- **Ecosystem coordinator (part-time).** Owns outreach and alignment conversations with Metagov, DDS Working Group, DelibTech Network, and prospective plugin author organizations.
- **Documentation specialist (part-time).** Owns the plugin author and host operator guides.

At the **lean tier**, most of these responsibilities are absorbed by the founding steward, with AI-assisted development covering the engineering execution and selective advisor engagement for the identity/standards questions. The **typical tier** adds a part-time contractor or two (commonly a technical writer for the guides and a part-time engineer for the harness or Polis productization), with the founding steward continuing to hold the architecture and ecosystem-coordination work. The **expanded tier** brings an additional engineer to meaningful capacity and funds dedicated time for ecosystem coordination, multiple external plugin-author engagements, and convenings. The role list above describes the work; the staffing pattern reflects the tier.

---

<a id="31-potential-pilot-partners"></a>
## 31. Potential Pilot Partners

Three categories of partner are relevant to the pilot.

### Plugin Author Partners

Organizations building deliberative or participatory tools that could become Civic Process Plugins. Strong candidates include several teams from the Metagov IDT cohort: **Decidim** (the widely-deployed participatory democracy platform, already part of the IDT cohort with a CSV interoperable export/import workstream), **MAPLE** (the Massachusetts public testimony platform), and **the Computational Democracy Project** (Polis upstream). Beyond the IDT cohort, **Ethelo**, **Remesh**, and **Go Vocal (formerly CitizenLab)** are also natural candidates.

### Host Environment Partners

Organizations operating host environments where plugins would run. The pilot's primary host is the Civic.Social Hub reference implementation. A second-host validation is part of the pilot scope; the most likely candidate is the **representative space** prototype already being built within the Civic.Social ecosystem. Beyond that, **Bonfire Networks** and **Decidim** are potential alternative host targets.

### Standards and Network Partners

Organizations whose standardization work the pilot aligns with: **Metagov** (Interoperable Deliberative Tools project), the **DDS Working Group**, and the **DelibTech Network** (DemocracyNext + AI & Democracy Foundation). Engagement with each is part of the pilot scope (sections 20–22).

---

<a id="32-estimated-budget"></a>
## 32. Estimated Budget

**The pilot scope is flexible, and the budget scales with it.** The plugin framework's core deliverables — the Civic Process Plugin Specification, the reference host machinery, and the formalization of the existing exemplars against the specification — can be delivered with a small focused team in a relatively compact timeline. Standards-alignment depth, multi-host portability validation, external plugin-author engagement, and documentation depth all scale upward from there. Three indicative tiers are described below; any tier produces a self-contained, usable release, and the higher tiers compound on the lower ones rather than replacing them.

This pilot's scope is also moderated by what already exists. The two Tier 1 exemplars (`civic.vote`, `civic.proposal`) are already operational in the Civic.Social Hub reference implementation, and a live Polis instance is already deployed and integrated with the representative-space service. The pilot's work is formalization, host machinery, and standards-alignment — not greenfield plugin construction. This is why even the lean tier produces a publishable specification and a working reference host.

The figures below assume the way Civic.Social currently operates: the founding steward leads the work, with contractors and advisors brought in for specific pieces as the tier scales up. If the pilot were staffed instead with multiple full-time engineers, a dedicated ecosystem coordinator, and a dedicated documentation specialist (the more conventional nonprofit-team approach), the totals would roughly double or triple. The figures reflect actual operating practice, not a hypothetical fully-staffed team.

### Lean — $50,000 – $75,000 (≈ 4–6 months)

Specification drafted and published as the Civic Process Plugin Specification v0.1. Reference host machinery (manifest inspection, capability granting, identity handoff, activity ingestion) built into the Civic.Social Hub. The two existing Tier 1 exemplars (`civic.vote`, `civic.proposal`) formalized against the specification. Polis Tier 3 integration documented as a reference pattern, building on the existing live deployment, but not fully productized against the new specification. The plugin development harness (conformance test suite plus AGENTS.md-style guidance for AI coding tools) shipped at first-version completeness. Minimal documentation beyond a plugin author quickstart. Single host environment. Executed primarily by the founding steward; budget covers founder compensation, hosting, tools, and selective advisor fees.

### Typical — $100,000 – $175,000 (≈ 6–9 months)

Everything in lean, plus: Polis fully productized as a Tier 3 plugin under the full specification. Worked IDT compatibility example (export and import paths). DDS structural mapping document. Complete plugin author and host operator guides. Cross-host portability proof — the same plugin running in a second host environment (the representative-space service is the natural candidate). One round of external plugin-author engagement, likely with one team from the Metagov IDT cohort. Budget adds a part-time contractor or two (commonly a technical writer for the guides and a part-time engineer for the harness or Polis productization), plus advisor fees, travel for one or two community-group meetings, and ecosystem outreach time.

### Expanded — $200,000 – $300,000 (≈ 9–12 months)

Everything in typical, plus: an additional engineer at meaningful capacity, sustained engagement with the Metagov IDT and DDS Working Group communities (including travel and convenings), two or more cross-host portability proofs, multiple external plugin-author engagements producing at least one third-party plugin built against the specification, stronger documentation and reference materials (including video walkthroughs and worked plugin templates), and specification governance scaffolding for ongoing third-party contribution.

### Cost Drivers Within and Across Tiers

- the depth of host-side plugin machinery built into the reference implementation
- the operational complexity of the Tier 3 Polis productization (the AGPL-licensing and upstream-maintenance considerations of integrating with an open-source external tool whose maintenance velocity is small — see also section 33)
- the depth of standards-alignment work — a structural mapping document versus a working bridge to DDS, for example
- the number of host environments the framework is validated against
- the depth of plugin author and host operator documentation produced
- the volume of outreach and convening with external plugin-author organizations
- whether the Civic Identity Pilot is running in parallel (reduces stub-adapter work)

The right tier depends on how aggressively the funder wants the framework to enter the broader deliberation-technology ecosystem. The lean tier produces a credible technical foundation. The typical tier additionally produces the ecosystem evidence — the IDT compatibility example, the DDS mapping, the cross-host portability proof — that make the framework legible to outside organizations. The expanded tier additionally invests in the human work — the convenings, the partnerships, the third-party engagements — that turn a published specification into an actual community of plugin authors.

---

<a id="33-risks-and-mitigations"></a>
## 33. Risks and Mitigations

**Risk: the plugin specification drifts from what real third-party plugins actually need.** The pilot's three exemplars are all built by the Civic.Social team, which means the specification is being validated against a single perspective. *Mitigation: engage at least one external plugin author (likely from the Metagov IDT cohort) during Phase 3 to attempt a fourth plugin against the draft specification; iterate the specification based on what that attempt surfaces.*

**Risk: Polis upstream maintenance is fragile.** The Polis project upstream (`compdemocracy/polis`) is functioning but not thriving — small maintainer team, slowing commit cadence, dependabot backlog, and a long-running math-worker rewrite that could stall. Upstream bug fixes, security patches, and new features cannot be relied on to land on any predictable timeline. The Civic.Social Polis instance at `polis.civic.social` is already self-hosted, which removes hosting as a dependency — the remaining risk is the upstream project itself. *Mitigation: continue running the existing self-hosted instance; be prepared to carry any patches or forks Civic.Social needs independently of upstream; document this maintenance posture as part of the Tier 3 reference design so other adopters of the framework go in with eyes open about external-tool dependencies.*

**Risk: the IDT specification evolves in ways that diverge from the Civic Process Plugin Specification.** IDT is itself a draft specification under active iteration. *Mitigation: maintain active engagement with the IDT working group throughout the pilot; treat IDT alignment as a moving target with versioned compatibility claims; surface divergences as inputs to the next revision of the Civic Process Plugin Specification.*

**Risk: DDS adopts an AT Protocol commitment that we cannot match.** DDS requires AT Protocol as its transport and identity foundation. Civic.Social currently targets ActivityPub forward-compatibility. If DDS hardens around AT Protocol while the Civic.Social architecture commits to ActivityPub, deep transport-level integration becomes harder. *Mitigation: scope the DDS alignment work to the data model (lifecycle, activities, credentials) rather than the transport; treat transport-level alignment as a separate, longer-term question; maintain the option to publish to both transports without committing to either as a required dependency.*

**Risk: capability declarations without enforcement create false confidence.** A plugin author or a host operator might assume the system is enforcing capability boundaries when the pilot is only requiring declarations. *Mitigation: document the deferred-enforcement design choice prominently in the plugin specification and the host operator guide; mark the gap explicitly in conformance criteria; build the enforcement engine in a follow-on phase before any production deployment hosts third-party plugins.*

**Risk: scope creep from "framework" toward "platform."** A pilot focused on a framework specification can drift into building the things the framework should host (more process types, more host integrations, more UI). *Mitigation: hold the line on the three-exemplar scope; new process types are downstream work, not pilot scope; the framework's value is the specification, not the count of plugins.*

**Risk: the hosting certification program is itself a substantial undertaking.** Designing, operating, and policing a hosting certification program for Tier 3 plugins is a real operational commitment — defining standards, evaluating instances, issuing badges, handling appeals, sustaining the program over time. If the pilot commits to running the program rather than scoping it, the pilot blows past its scope and budget. *Mitigation: the pilot explicitly scopes and documents the certification program as part of the specification but does not operate it. Building and running the program is a follow-on undertaking dependent on the first wave of Tier 3 plugins reaching production and on broader ecosystem governance input on who should run it long-term. See section 18 and the open question in section 34.*

---

<a id="34-open-questions-for-further-design"></a>
## 34. Open Questions for Further Design

The following are known unknowns that the pilot will help resolve.

**How do we version the contract surface long-term?** v0.1 of the process / activity / manifest contracts will ship with the pilot, but the strategy for breaking versus non-breaking changes, deprecation windows, and host compatibility matrices is not yet defined. The pilot will surface what the right shape is based on what the exemplar work needs.

**What is the right capability granularity?** The capability classes named in section 13 are a reasonable first cut, but the pilot may surface that some are too coarse (e.g. "network" needs domain-level scoping, which we have started but not finalized) or too fine (e.g. distinguishing UI surfaces inside a single host might be unnecessary in practice).

**Where exactly do Tier 3 plugins emit activities from?** A Tier 3 plugin lives outside the host's process boundary; the right way for its activities to land in the host's activity emitter is not yet specified in detail. The Polis exemplar will force a concrete answer.

**How do we handle plugin upgrades inside a host?** When a plugin's manifest version changes, what is the host's responsibility to running instances of older versions? This is a future-proofing question more than an immediate one, but the answer will shape the manifest schema.

**What is the right alignment with DDS at the *transport* level?** As described in section 21, data-model alignment is achievable; transport alignment is harder. The pilot will not resolve this fully but will document where the line should fall.

**Which community-group venue should govern the long-term evolution of the Civic Process Plugin Specification?** Community-group governance is the intended model, not the open question — this specification is published as a working draft *for* community discussion and is expected to evolve through open meetings, just as the DDS Protocol is governed by the DDS Working Group, ActivityPub by the W3C Social Web Community Group, and the IDT interop ontology by the Metagov cohort. What remains open is the *venue*: convene a dedicated Civic.Social Working Group; fold the spec into one of the existing adjacent venues (the DDS WG, the Metagov IDT cohort, the DelibTech Network); or coordinate across multiple venues with overlapping membership. The pilot will surface which model works best based on which community groups engage most actively during the v0.1 → v1.0 evolution. Civic.Social's posture is to support the venue that has the most relevant participants and the lowest coordination overhead, even if that means handing off primary stewardship of the spec to an existing group.

**Who issues hosting certifications long-term?** Section 18 introduces an optional Tier 3 hosting certification layer. Civic.Social as the initial issuer is the obvious default for the pilot, but a healthier long-term model might be third-party certifiers, a standards body, or a multi-stakeholder governance structure. The pilot will scope the program design and surface the question; the answer needs broader ecosystem input before any program is operated at scale.

---

<a id="35-conclusion"></a>
## 35. Conclusion

The Civic Process Plugin Pilot is the work that turns the Civic Process Specification and the Civic Plugin Architecture into shippable infrastructure. It produces the formal Civic Process Plugin Specification, three plugin exemplars that exercise the full trust-tier range (`civic.vote`, `civic.proposal`, Polis), a reference host implementation, and the alignment work that lets the framework cohere with the broader deliberation-technology standardization effort.

The pilot's deliverables serve two audiences at once. For the Civic.Social ecosystem itself, the pilot delivers the integration layer that makes every other Civic.Social pilot stronger — Civic Hubs become a richer host, Civic Identity becomes the universal participation credential, the Civic Activity Feed becomes a coherent stream of civic participation across many tools, and surfaces beyond hubs (representative spaces, dashboards, external embeds) gain a uniform way to host civic processes. For the broader civic-tech ecosystem, the pilot delivers a concrete plugin specification that any deliberative or participatory tool can implement, and an honest alignment posture toward the Metagov IDT, DDS, and DelibTech efforts that the field is already coalescing around.

The strategic bet is straightforward. Civic technology's missing layer is interoperability, and the right way to build it is a plugin framework rooted in decentralized identity and a shared activity stream, with a trust model strong enough to absorb both small first-party processes and large external tools. The pilot is how Civic.Social tests that bet in practice — and, if it succeeds, it is how a fragmented landscape of independent civic tools begins to function as a single participation ecosystem.

---

<a id="appendix-a--civic-process-plugin-catalog-illustrative"></a>
## Appendix A — Civic Process Plugin Catalog (Illustrative)

This appendix is a thought exercise, not a commitment. The pilot itself delivers only the three exemplars in section 24 (`civic.vote`, `civic.proposal`, Polis). The catalog below illustrates the *kinds* of civic processes the plugin framework is intended to support, with examples of real-world tools in each category that could plausibly become plugins — by reaching out to the upstream organization, by wrapping their hosted service as a Tier 3 plugin, or by reimplementing the process type in-house.

Inclusion here is neither an endorsement, a partnership commitment, nor a statement that the tool's authors have agreed to participate. It is a survey of the landscape the plugin framework is designed to interoperate with.

### Opinion clustering and large-scale deliberation

- **Pol.is** — Real-time opinion mapping using statement-level voting and clustering. Covered in section 24 as the pilot's Tier 3 exemplar.
- **Remesh** — AI-assisted large-scale deliberation with real-time response clustering. Andrew Konya, co-founder, is in the DelibTech Network.
- **Talk to the City (T3C)** — Extracts areas of agreement and disagreement from large-scale text input. Funded participant in the Metagov IDT cohort.
- **Harmonica** — AI chatbot that synthesizes opinions from 1:1 dialogues into structured group output. Metagov IDT cohort.
- **Cortico / Constructive Communication (MIT)** — Story-driven small-group deliberation. Deb Roy (Cortico CEO, MIT CCC director) and Dimitra Dimitrakopoulou (MIT CCC) are in the DelibTech Network.

### Structured deliberation and citizen assemblies

- **Ethelo** — Multi-criteria decision-making platform for complex tradeoffs. Metagov IDT cohort.
- **DeliberAIde** — AI-assisted facilitation for deliberative processes. DelibTech Network member.
- **Dembrane** — Audio and transcript-based deliberation tooling. DelibTech Network member.
- **Frankly** — Open-source video platform for constructive dialogue, developed by Harvard's Applied Social Media Lab at the Berkman Klein Center, co-developed with Lawrence Lessig. Algorithmic group matching for diverse perspectives, embedded discussion prompts, hostless facilitation. AGPL, open-sourced April 2025. Already integrated into Bloom Project's CivicOS; deployed for The South Carolina Forum's statewide deliberation process.
- **Iswe Foundation platform** — Global community deliberation infrastructure. Metagov IDT cohort.
- **Stanford Deliberative Democracy Lab tools** — Research-driven deliberation tooling. Alice Siu is in the DelibTech Network.

### Advisory voting and ranking

- **Power Ranker** — Pairwise-preference probability distributions over item sets. Metagov IDT cohort, with a Slack interface.
- **Pairwise** — Pairwise off-chain voting platform (originated for RetroPGF distribution). Metagov IDT cohort.
- **Snapshot** — Off-chain voting widely used in DAO governance. Not civic-specific but architecturally relevant.

### Participatory budgeting

- **Stanford Participatory Budgeting Platform** — Voting-phase PB infrastructure used by municipalities. Metagov IDT cohort.
- **Decidim PB module** — PB component of the broader Decidim platform.
- **CONSUL Democracy** — Open-source participation platform with PB module; deployed by Madrid, NYC, Buenos Aires, and dozens of other municipalities.

### Proposal, amendment, and co-decision workflows

- **Decidim Proposals** — Citizen-submitted proposals with structured deliberation.
- **Loomio** — Group decision-making platform with proposal and agreement workflows. Long-standing open-source civic tooling.
- **Open Collective** — Transparent collective management with built-in governance primitives. Architecturally aligned with the federation model.

### Public comment, testimony, and consultation

- **MAPLE** — Massachusetts state legislature public testimony platform. Metagov IDT cohort.
- **Go Vocal (formerly CitizenLab)** — Consultation and engagement platform widely deployed by European municipalities. Metagov IDT cohort.
- **US federal e-rulemaking infrastructure** — `regulations.gov` and related federal public-comment infrastructure. More plausible as an integration target for a Civic Process Plugin than as a plugin source — but the inverse mapping (a federal comment process exposed as a plugin) is worth considering.

### Petitions and campaigns

- **Action Network** — Petition and campaign tooling used across US progressive civic infrastructure.

### Argumentation and structured debate

- **Swarmcheck** — Computer-assisted argumentation and Delphi-style consensus. Metagov IDT cohort.
- **Kialo** — Structured argument-tree platform.
- **Policy Craft** — Bottom-up policy development through case-grounded deliberation. Metagov IDT cohort.

### AI-assisted process design and facilitation

- **Moral Graph Elicitation** — Originally built for OpenAI's Democratic Inputs grant; now a standalone open-source tool. Metagov IDT cohort.
- **Constituency Listening** — Visualization and rigor tooling for voice-to-decision processes. Metagov IDT cohort.
- **Global Brain Algorithm** — Misinformation-debunking algorithm. Metagov IDT cohort.

### Co-operative governance and small-group decision support

- **Ize** — Collaborative workflows across tool and identity boundaries. Metagov IDT cohort.
- **Interoperable Co-operative Governance** — Repurposed collaborative tools into an interoperable democratic-decision dashboard. Metagov IDT cohort.
- **Evocracy decision-making protocol** — Hierarchical selection and integration protocol. Metagov IDT cohort.

### Whole-platform candidates (could host *or* contribute plugins)

Several platforms straddle the line between "host environment" and "source of plugins" — they could plausibly run the Civic Process Plugin Framework as a host while also contributing their existing participation tools as plugins.

- **Decidim** — The widely-deployed participatory democracy platform; part of the Metagov IDT cohort with an active CSV interoperability workstream. The strongest candidate for dual-role engagement.
- **CONSUL Democracy** — Open-source municipal participation platform.
- **Bonfire Networks** — Modular federated social platform with native ActivityPub support. Already named as a candidate hub-engine in the Civic Hubs Pilot.

### External catalog sources

For an actively-maintained view of the broader landscape, the following external catalogs are more comprehensive than anything this specification should attempt to maintain inline.

- **Civic Tech Field Guide** (`civictech.guide`) — Curated catalog of hundreds of civic-tech tools across all categories. Maintained by Matthew Stempeck (Evens Foundation), who is also in the DelibTech Network. The most exhaustive single source for civic-tech discovery.
- **Metagov Deliberative Tools Gallery** (`metagov.org/delib-tools`) — Metagov's curated list of deliberation-specific tools.
- **Metagov IDT cohort** (`metagov.github.io/interop`) — The funded teams implementing the IDT flatfile interoperability requirement. Each is a near-term candidate for plugin adaptation, since they are already conforming to an interoperability standard adjacent to the Civic Process Plugin Specification.
- **DelibTech Network member roster** (`demnext.org/projects/delibtech-network`) — Forty-plus member organizations spanning multiple continents, including practitioners as well as tool authors.

### How to read this catalog

The intent here is to make concrete what the plugin framework's reach could look like at maturity. The pilot itself stays disciplined to the three exemplars in section 24 — that scope discipline is what makes the pilot achievable. The broader landscape sketched above is the future the work is building toward, not a checklist to be worked through.

Any of these tools, and many that are not listed, could in principle become a Civic Process Plugin. Which ones actually do will depend on funder support, plugin-author interest, and what the pilot exemplars teach us about how forgiving (or unforgiving) the plugin specification turns out to be in practice.

---

*Civic.Social — civic.social | contact@civic.social*
