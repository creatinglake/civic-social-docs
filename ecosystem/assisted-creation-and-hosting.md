---
status: draft
last-reviewed: 2026-07-24
owners: [adam]
version: 0.1
---

# Assisted Creation and Managed Hosting

*A concept for making ecosystem development radically accessible*

Once the four canonical specifications are hardened, the ecosystem has a precise, machine-checkable definition of what it means to be a Civic Space, a Civic Process, a Civic Activity, and a civic identity. This document describes what that precision makes possible: a system where anyone, not just software teams, can describe the civic tool their community needs, have an AI produce a spec-compliant implementation of it, and have that implementation hosted for them, live and interoperable with the entire ecosystem.

This is a concept document, not a pilot specification. It names a destination the existing architecture is already pointed toward, collects the pieces of it that are already designed, and identifies the preconditions and open questions that stand between here and there.

---

## 1. The Idea

The pattern sometimes called "vibe coding" (conversational, AI-assisted software creation, where a person describes what they want and iterates with an AI toward working software) has made building software accessible to people who would never have written it by hand. Applied naively to civic infrastructure, it would be reckless: civic tools carry trust obligations that casually generated code cannot meet.

But the Civic.Social architecture changes the calculus, because the ecosystem is defined by open specifications with machine-runnable conformance tests. Correctness is not a matter of opinion or review-board judgment; it is a test suite the generated code either passes or fails. That is the property that makes assisted creation viable here when it would be irresponsible elsewhere.

The full concept has three parts:

**Assisted creation.** A person describes the space, process, or plugin they need, in plain language. An AI-assisted builder, working against the published specifications and the plugin development harness, generates an implementation and iterates until the conformance suite passes.

**Managed hosting.** The resulting artifact deploys onto multi-tenant infrastructure operated by Civic.Social (or by any other conformant hosting provider) with no server administration required of the creator. Provisioning a new space mints its DID, binds its founding admin, and publishes its discovery manifest.

**Ecosystem citizenship.** Because the artifact conforms to the specifications, it is not an isolated app. It emits Civic Activities into the shared stream, participates in the identity layer, appears in the discovery layer, and can be installed, forked, or extended by others.

The goal is that the distance between "our town needs a participatory budgeting process with a youth-council track" and a running, interoperable, discoverable implementation of exactly that is measured in days, not procurement cycles.

---

## 2. The Pieces Already Exist

This concept is a composition of mechanisms the ecosystem documents have already committed to, not a new invention. Three are load-bearing.

**The plugin development harness.** The [Civic Process Plugin Pilot](../pilots/civic-process/civic-process-pilot-spec.md) (section 17) commits to shipping a conformance test suite, a scaffolding generator, reference plugin templates, and a tool-agnostic agent guidance file alongside the specification itself, on the explicit bet that a plugin author paired with an AI coding tool and the harness can produce a compliant plugin in hours rather than weeks. The harness is the engine of assisted creation. What this concept adds is a front end: today the harness assumes a developer driving their own AI coding tool; the builder described here drives the same harness on behalf of someone who is not a developer at all. The conformance suite becomes the feedback loop inside the generation process, not just a validation step after it.

**Hosting quality and certification.** The same pilot (section 18) designs an optional certification layer for externally hosted plugins: availability, latency, throughput, feed timeliness, and operational practices, surfaced as a visible signal rather than a gate, with certification classes (for example Basic, Production, and Civic-grade) matched to the stakes of the participation context. This concept slots generated artifacts directly into that framework. A newly generated process starts life uncertified or Basic; certification classes give communities an honest signal about what a given artifact has earned, without gatekeeping who may create.

**Multi-tenant provisioning.** The ecosystem architecture audit (July 2026) laid out the SaaS path for spaces: row-level tenancy, a provisioning flow that creates the space, mints its DID, binds its founding admin, and serves its per-space discovery manifest. The audit describes that flow as the self-serve "spin up your own space" product, and notes that SaaS provisioning and the Civic Space primitive are the same design done twice if not done together. That provisioning flow is the deployment target for everything the builder produces.

Two further threads point the same direction. The [AI Strategy and Agent Framework](ai-agent-framework.md) already names a **Civic Developer Agent** whose role is generating plugin scaffolds, assisting with integration, and validating interoperability; the builder described here is that agent, productized. And the Civic Space Specification's insistence that space types are an open, extensible set ("anyone can build new spaces, same protocol underneath") is precisely the promise this concept operationalizes. A protocol that permits anyone to build a new space type, but in practice requires an engineering team to do so, has kept the promise only on paper.

---

## 3. What It Would Look Like

Three tiers of accessibility, each building on the one below:

**Tier A: the harness path (already planned).** A developer uses their own AI coding tool against the published specs and harness. This exists as a pilot commitment and remains the path for sophisticated plugins. Nothing in this concept replaces it.

**Tier B: the hosted builder.** A guided, conversational environment operated as part of the ecosystem. A community organizer, clerk, teacher, or civic group describes what they need. The builder interviews them (scope, participants, lifecycle, credentials required, visibility rules), generates an implementation from the reference templates, runs the conformance suite continuously, and iterates until the artifact passes. The creator reviews the behavior of the tool, not its source code, through a working preview.

**Tier C: managed deployment and discovery.** One action takes the passing artifact live: hosted on managed multi-tenant infrastructure, registered in the discovery layer, emitting activities. From there it is subject to the same operational realities as anything else in the ecosystem: it can seek certification, publish updates, be forked by another community, or be retired with its data honoring the portability contract.

The creator owns the result. Generated artifacts are open source by default, portable off the managed infrastructure by construction (the same specifications that make them interoperable make them movable), and never locked to the builder that made them.

---

## 4. Why It Matters

**It collapses the procurement barrier.** The [use cases document](../use-cases/use-cases.md) describes a city clerk whose council has wanted participatory budgeting for years but could never justify a new vendor, a new contract, and a new line item. The plugin model already shrinks that decision; assisted creation shrinks it further, to the marginal case where the process a community needs does not exist yet in the plugin library. Today that case dead-ends in a custom build. Under this concept it becomes an afternoon with the builder.

**It serves the long tail.** The processes that matter most locally are often too specific for any vendor to build: a watershed district's comment process, a school board's budget consultation with student participation, a tribal government's consensus procedure. No commercial roadmap reaches these. A generation system whose marginal cost per new process type approaches zero does.

**It compounds the ecosystem.** Every generated artifact is a conformant, open, forkable addition to the commons. The library of reference implementations grows with use, which improves generation quality, which lowers the barrier further. This is the flywheel the harness bet on, extended from professional developers to everyone.

**It is the mission, restated.** Civic.Social exists to prevent civic infrastructure from being a gated market. An ecosystem where building requires an engineering budget is gated by capacity even if it is open by license. Assisted creation is how "open" becomes true in practice and not only in principle.

---

## 5. Hard Constraints

The existing documents impose real constraints on this concept, and the concept should be understood as accepting them rather than negotiating with them.

**The production boundary.** The [Civic Plugin Architecture](civic-plugin-architecture-spec.md) is unambiguous: a host must not run third-party plugin code in production civic processes before sandbox (Tier 2) enforcement exists for that code's tier. A builder that generates code from public prompts is a machine for producing third-party code, and its output must be treated with exactly that trust posture regardless of the conformance suite passing. The consequence is direct: **the Tier 2 WebAssembly sandbox, currently deferred until third parties want to ship plugins, becomes a hard prerequisite of the hosted builder.** Generated processes should target the sandbox by construction, or run as externally isolated (Tier 3) services. This concept is, in effect, the strongest known argument for scheduling the sandbox work.

**Conformance is necessary, not sufficient.** The conformance suite verifies protocol correctness: manifests, capability declarations, identity seams, activity emission. It cannot verify that a process is fair, comprehensible, or fit for its civic purpose. A generated "deliberation" could pass every test and still be a push poll. The certification layer, human review norms for higher-stakes classes, and the visibility of generated provenance (see below) are where fitness lives, and the concept must not present the green test suite as more than it is.

**The commitments in [Our Approach to AI](../positions/our-approach-to-ai.md) apply in full.** Generated artifacts must be labeled as AI-assisted, with their provenance visible to the communities that adopt them. Humans hold the authority: a person reviews and accepts what the builder produces, and the builder drafts rather than decides. And the model-agnostic, portability-first architecture applies to the builder itself, which must not bind the ecosystem to any single AI vendor.

**Hosting must be sustainable and non-captive.** Managed hosting at near-zero marginal cost is what makes the concept accessible, and it is also a recurring cost center and a potential center of gravity. The mitigations are the ones the ecosystem already uses: open specifications mean any provider can offer conformant hosting, the portability contract means any artifact can leave, and certification (not exclusivity) is the quality mechanism. Civic.Social operating the first builder and the first host must not make it the only one; the concept fails its own values if it quietly recreates a platform monopoly one layer up.

---

## 6. Open Questions

- **Review flow for generated artifacts.** What human review, if any, gates initial deployment (as opposed to certification, which gates trust signals)? Options range from none (publish freely, uncertified) to community-of-practice review for civic-grade contexts. Where is the line between quality assurance and gatekeeping?
- **Naming and namespace.** Generated process types need identifiers. Who administers the extension-type namespace when creation is high-volume, and how are collisions and squatting handled?
- **Abuse surface.** A free generation-and-hosting pipeline will attract spam, impersonation of jurisdictions, and manipulative process designs. Which existing mechanisms (identity requirements for creators, jurisdiction verification, moderation strategy) carry over, and what is new?
- **Scope of generation.** Processes and plugins are the clear first target because the harness exists for them. Whole space types are a larger leap (they carry the full host contract). Does the builder start processes-only, and treat space generation as configuration of the existing multi-tenant space product rather than code generation?
- **Cost model.** What does hosting cost at one thousand small artifacts, and who pays: philanthropy, a freemium hosting tier, jurisdictions? Certification classes may map naturally to hosting tiers.
- **Builder governance.** Is the builder itself an ecosystem service with published behavior (prompts, templates, refusal rules), subject to the same transparency norms as everything else?

---

## 7. Sequencing

This concept deliberately claims no timeline. Its preconditions are all existing commitments, and it inherits their order:

1. **Spec hardening.** The four canonical specifications complete enough that conformance is machine-checkable from the published documents alone (the gap the July 2026 audit identified).
2. **The harness ships.** Conformance suite, scaffolding, templates, and agent guidance file, per the Civic Process Plugin Pilot.
3. **Multi-tenant provisioning ships.** The self-serve space product, per the audit's SaaS path.
4. **The Tier 2 sandbox ships.** The production boundary makes this the last gate before generated code can serve real participants.

When those four exist, the hosted builder is an integration project, not a research project. That is the measure of how much of this concept the ecosystem has already built: the radical part is not any single component, but the decision to point them at everyone.

---

## Related Documents

- [Civic Process Plugin Pilot Specification](../pilots/civic-process/civic-process-pilot-spec.md), sections 17 (plugin development harness) and 18 (hosting quality and certification)
- [Civic Plugin Architecture](civic-plugin-architecture-spec.md), trust tiers, capability model, and the production boundary
- [AI Strategy and Agent Framework](ai-agent-framework.md), the Civic Developer Agent and ecosystem development use cases
- [Our Approach to AI](../positions/our-approach-to-ai.md), the commitments that constrain any AI-assisted creation
- [Use Cases](../use-cases/use-cases.md), the procurement-barrier scenario this concept addresses
- Ecosystem architecture audit (July 2026, internal), Deep Dive D on SaaS and multi-tenant readiness

---

*Civic.Social — civic.social | contact@civic.social*
