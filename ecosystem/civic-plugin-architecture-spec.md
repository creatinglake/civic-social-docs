---
status: draft
last-reviewed: 2026-05-29
owners: [adam]
version: 0.1
---

# Civic.Social Plugin Architecture

*Working architectural reference for the layer above the Civic Process Specification. This document will be formalized into the public, versioned Civic Process Plugin Specification through the [Civic Process Plugin Pilot](../pilots/civic-process/civic-process-pilot-spec.md).*

---

## Purpose

The [Civic Process Specification](civic-process-spec.md) defines **what a Civic Process is** — its lifecycle, identity requirements, events, and action contracts. This document defines the layer *above* that: **how processes get packaged, trusted, scoped, and installed as plugins** across the host environments in the Civic.Social ecosystem. The Civic Process Specification is correct as-is; this document fills what it deliberately does not cover.

The goal is a WordPress-style plugin library — drop-in extensions for Civic Hubs, Representative Spaces, Citizen Dashboards, and other host environments — **without** WordPress's defining flaw, which is that plugins run with near-total access to the host. For civic infrastructure, where the integrity of a vote or a deliberation is the whole point, the trust boundary is the architecture.

This document is a working architectural draft. It will be formalized into the public, versioned Civic Process Plugin Specification through the Civic Process Plugin Pilot.

---

## 1. The Guiding Principle

**Least privilege.** Every plugin is assumed untrusted by default. It receives only the access it explicitly declares and the host explicitly grants. Nothing more.

When an architectural choice is ambiguous, this principle breaks the tie.

---

## 2. The Three Trust Tiers

A Civic Process Plugin is not one kind of thing. Different plugins live in different places depending on how much the host trusts the plugin's code and where that code actually runs. The architecture defines three tiers; every plugin declares which tier it runs as in its manifest, and the host enforces the boundary appropriate to that tier.

**Tier 1 — In-process handler.** First-party code that the host operator has written and vetted, running directly inside the host's own program via a handler registry. Fast, full access, simple — the plugin is effectively a built-in feature of the host that happens to be exposed through the plugin contract. The `civic.vote` and `civic.proposal` plugins in the Civic.Social Hub reference implementation are Tier 1. Most early implementations are expected to be Tier 1, and that is appropriate — but the architecture must not assume everything is Tier 1.

**Tier 2 — Sandboxed plugin.** Code the host wants to run but does not fully trust — typically a third-party plugin built by an author other than the host operator. The plugin runs inside a sandbox (likely a WebAssembly runtime such as Extism) that limits what it can read, write, and call out to. The sandbox runtime is **deliberately deferred** in the current pilot — it is the right architecture for future third-party plugins, but it is premature to build before third parties want to ship plugins the host operator did not write.

**Tier 3 — External service plugin.** A whole separate application running on its own infrastructure, with its own stack and database. The host does not embed the plugin's code at all; it launches the external service, hands off the citizen's identity at the boundary, and receives results back as Civic Activities. Polis is the canonical example — a full Clojure/Postgres/React application that the host integrates with rather than ingests.

The Civic Process Specification's required endpoints (`GET /process/:id`, `POST /process/:id/action`) describe a Tier 1 process talking the host's own interface. Tier 3 plugins do not speak that natively — they integrate through the identity-handoff and activity-emission seams instead. The architecture is built to accommodate this asymmetry: the *external* interface any host expects is identity-and-activity; the *internal* interface a process speaks is its own.

---

## 3. The Plugin Manifest

The Civic Process Specification's process descriptor describes a *running process instance*. A plugin library additionally needs a **manifest** that describes the *installable type* — the plugin itself, before any instance exists.

A plugin manifest declares:

- **Identity** — a human-readable name, a stable plugin id, and a version
- **What it provides** — which Civic Process type(s) it defines (e.g. `civic.vote`), conforming to the Civic Process Specification
- **Any host-specific requirements** — optional. A plugin is universal by default and works in any compliant host; this field is only present when the plugin genuinely depends on host-specific context (see section 7)
- **Which tier it runs as** — in-process, sandboxed, or external service
- **Capabilities it requests** — see section 4
- **Which contract version it targets** — the version of the process, action, and activity contracts the plugin was built against

The manifest answers "what is this plugin and what does it need?" The descriptor answers "what is this specific instance of it doing right now?" The two are intentionally distinct.

---

## 4. Capabilities and the Security Layer

The single most important addition the plugin layer makes over the bare Civic Process Specification is **capability declaration**.

The Civic Process Specification is thorough about what *participants* are allowed to do — which credentials a citizen needs in order to take part. That is eligibility, and it is about people. It says nothing about what the *plugin code itself* is allowed to do. These are two different questions, and the second one is the "don't become a WordPress security nightmare" concern.

A plugin declares the capabilities it needs, and the host grants them narrowly. Default deny. The relevant capability classes are:

- **Activities** — which activity types the plugin may emit, and which it may listen to
- **State** — which process state the plugin may read, and which it may write
- **Network** — which external domains, if any, the plugin may call out to. A Tier 3 plugin like Polis needs this; an in-process voting plugin does not
- **Identity** — which identity claims about a participant the plugin may see. A plugin should see the minimum needed to do its job, not the participant's full credential set
- **UI surfaces** — where in the host interface the plugin may render

Writing capabilities down — even before anything enforces them — forces every implementation to be honest about its blast radius. The declaration is cheap and shapes good habits; the enforcement engine can be built later.

---

## 5. The Integration Seams

Two integration seams are already well-defined in the Civic Process Specification, and plugins of every tier integrate through them rather than reaching into host internals:

- **Identity** — participants arrive at a plugin with DID-based identity and verifiable credentials, established by the host's identity layer. Plugins receive identity through this seam; they never manage their own accounts. Tier 3 external services that have their own native identity systems (such as Polis's XID) integrate by mapping host-provided identity into the plugin's internal identifier space at the boundary, never by asking the participant to re-authenticate
- **Activities** — every action and lifecycle transition emits a standardized Civic Activity through the single activity-emission path. Plugins coordinate by emitting and reacting to activities, not by calling each other directly. A Tier 1 plugin emits activities natively into the host's emitter; a Tier 3 plugin emits activities by posting them across the host-plugin boundary, which the host then forwards into its emitter. Either way, downstream consumers see a single coherent activity stream

This discipline is what allows a Tier 1 in-process handler and a Tier 3 external service to look the same from the host's point of view, and what makes the activity feed a trustworthy source-of-truth for civic participation.

---

## 6. Versioning

The process, action, activity, and descriptor contracts are effectively the public API that plugins are written against. They need versions, and the plugin manifest must declare which contract version it targets.

When a host installs a plugin, it inspects the manifest's declared contract version and compares it against its own supported versions. If the plugin targets a contract version the host cannot satisfy, the host may refuse the plugin, run it in a degraded mode, or flag it for the operator's attention. Without this discipline, every change to the underlying contracts risks silently breaking every plugin in the ecosystem.

---

## 7. Host Compatibility

A Civic Process Plugin is **universal by default**: it works in any compliant host environment. A plugin's manifest does not need to enumerate the host types it supports, and a host may install any plugin that satisfies its capability requirements. The same plugin runs unchanged across Civic Hubs, Representative Spaces, Citizen Dashboards, and external embeds — that portability is the central reason the plugin model exists.

A plugin author *may* declare host-specific requirements when the plugin genuinely depends on something only certain hosts provide — for example, access to a host-specific UI surface, host-provided context such as a representative's calendar, or trust assumptions that only certain host types satisfy. When such requirements are declared, the host enforces them at install time and refuses installations into hosts that cannot satisfy them. The default — no host requirements declared — means "this plugin works anywhere a compliant host exists."

This framing matters because the plugin model's reason for existing is portability. Asking every plugin author to enumerate where their plugin can run would invert that promise. The expected case is universal compatibility; the constrained case is the rare exception that the plugin author opts into.

---

## 8. Implementation Checklist

A compliant plugin should:

- Define its process type per the Civic Process Specification (lifecycle, actions, events)
- Ship a manifest declaring its id, version, and provided type(s); declare host-specific requirements only if the plugin genuinely needs them (plugins are universal by default)
- State which trust tier it runs as (in-process, sandboxed, or external service)
- Declare its capabilities — activities, state, network, identity, UI — even where enforcement is not yet in place
- Integrate only through identity and activities, never by reaching into host internals
- Carry a contract version it targets

What can be safely skipped at the implementation level today:

- Building the sandbox runtime — Tier 1 is fine for most early implementations
- Capability *enforcement* — the *declaration* is what matters now
- Multi-host surface rendering — most plugins are universal and do not need any host-specific declaration; the rendering machinery for plugins that legitimately constrain themselves can be built when such plugins emerge

---

## 9. Deliberately Deferred

The following are part of the architectural vision but are deliberately not in scope for the current pilot phase:

- **The Tier 2 sandbox runtime** (WebAssembly / Extism). Worth building once third parties want to ship process types the host operator did not write; premature before that
- **The capability enforcement engine.** The declarations are valuable now; enforcement can come later
- **The multi-host rendering machinery for plugins with host-specific requirements.** Most plugins are universal and do not need this; the rendering system can be built when host-specific plugins emerge

The pattern: **build the declarations now, because they are cheap and shape every implementation; defer the enforcement, because it is expensive and not yet needed.**

---

## 10. Polis as the First Tier 3 Plugin

Polis is the motivating real-world test of this architecture, and is the first Tier 3 external-service plugin the Civic.Social ecosystem will productize. Crossing that boundary once — launching a Polis conversation from a host, handing off identity, and receiving results back as Civic Activities — is what will teach the most about where identity, moderation, and data ownership actually get hard. Those lessons will flow back into the next revision of this document and into the formal Civic Process Plugin Specification.

---

## 11. Beyond the Specification — Tooling and Ecosystem Quality

A specification defines the contract a plugin must meet. Two adjacent concerns complement the specification but are not part of it, and are part of the broader Civic.Social plugin program nonetheless.

**Plugin development harness.** A specification is more useful when it ships with a conformance test suite, scaffolding, reference templates, and a tool-agnostic agent guidance file that any AI coding tool can read. The Civic Process Plugin Pilot will produce a first version of this **plugin development harness** alongside the specification itself, on the bet that good tooling — especially in an era of AI-assisted development — materially lowers the barrier to producing compliant plugins. The harness is not an AI; it is the structured set of project artifacts that AI coding tools work best against. See the [Civic Process Plugin Pilot](../pilots/civic-process/civic-process-pilot-spec.md) section 17 for details.

**Hosting quality for external-service plugins.** Tier 3 plugins introduce a quality dimension the specification cannot address directly — the host depends on the plugin operator's infrastructure for uptime, latency, throughput, and feed timeliness. An optional **hosting certification** layer can attest that a specific Tier 3 plugin hosting instance meets defined standards, surfaced as a visible signal (a verification badge) in plugin discovery interfaces. Certification is not gatekeeping — uncertified plugins remain installable — but it is the framework's answer to the ecosystem-performance gap that pure specification cannot close. See the [Civic Process Plugin Pilot](../pilots/civic-process/civic-process-pilot-spec.md) section 18 for the program design.

Both concerns will mature alongside the specification itself.

---

*Civic.Social — civic.social | contact@civic.social*
