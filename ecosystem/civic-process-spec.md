---
status: review
last-reviewed: 2026-07-26
owners: [adam]
version: 0.2
---

# Civic Process Specification v0.2

## Purpose

Define a standard, interoperable model for **Civic Processes** within the Civic.Social ecosystem.

A Civic Process is a structured, stateful interaction that enables participation between citizens, organizations, and institutions.

This specification defines:
- Process lifecycle (canonical vocabulary + lifecycle profiles)
- Identity, eligibility, and disclosure requirements
- Activity model integration (per the Civic Activity Specification)
- Interface and integration contracts (creation, descriptor, actions)

**Relationship to plugins.** A Civic Process *type* is implemented and distributed as a **Civic Process Plugin**: a handler registered with the host space's process registry, packaged and trusted per the **Civic Plugin Architecture**. This specification defines the process contract a plugin implements; the plugin architecture defines how it is packaged, scoped, and installed. The two documents are companions and cross-reference each other.

> **Which horizon is this?** This document specifies what can be built and verified **today**; where it defers a capability, limits a guarantee, or declines to require something the wider Civic.Social material describes, that is a schedule, not a retreat. **[Civic.Social Horizons](../canon/phasing.md)** maps each of those deferrals to the pilot that closes it and to the destination it is headed toward.

---

## Notation and Conformance Language

**Notation.** The key words MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, SHOULD NOT, RECOMMENDED, MAY, and OPTIONAL in this document are to be interpreted as described in RFC 2119 and RFC 8174 when, and only when, they appear in all capitals. The same words in lower case carry their ordinary English meaning and impose no requirement.

---

## How to Read This Specification

Sections 1–14 are binding: they define what a conformant Civic Process is, using the notation above. Sections 8.3–8.5, 11.5, 17, and the Summary are rationale — they explain why the requirements are shaped the way they are, and are marked non-normative where they begin.

If you are implementing a minimal process runtime, start with **Section 4.2** (which lifecycle profile your process type declares — this determines nearly everything else you owe), then **Section 12** (the descriptor and endpoints you must expose), then **Section 14** (the per-profile conformance table, which lists your obligations in one place). Sections 5–6 describe the full deliberative phase model; if your profile is not `deliberative`, you run only the subset your states support.

---

## 1. Definition

A **Civic Process** is:

> A time-bound or stateful civic interaction that allows participants to take actions (e.g., vote, deliberate, comment) under defined rules and eligibility requirements.

### Examples
- Citizen Assembly
- Advisory Vote
- Participatory Budgeting
- Petition
- Public Consultation

---

## 2. Core Properties

Every Civic Process MUST define:

### 2.1 Metadata
- `id` (UUID)
- `type` (e.g., civic.vote, civic.assembly)
- `title`
- `description`
- `jurisdiction`
- `created_by` (DID or organization ID)
- `created_at`

### 2.2 Lifecycle
- `status` — drawn from the canonical state vocabulary (draft | scheduled | active | closed | finalized), as constrained by the process type's **lifecycle profile** (Section 4). A profile MAY substitute one type-specific terminal alias (such as `archived`) for a canonical terminal state; the alias is not added to this enum, and it exports as its canonical equivalent per Section 4.2.
- `lifecycle_profile` — the profile the process type declares (Section 4.2)
- `starts_at`
- `ends_at`

### 2.3 Participation Rules
- `eligibility_requirements`
  - list of required credentials (e.g., vc:resident:us-va-floyd)
  - MAY be given inline as that list, or by reference to an **Identity Policy Object** (Civic Identity Specification §8) — in which case its content is that object's `assurance_requirements` field. They are the same concept at two levels: what a participant must hold to be allowed to act.
- `participation_mode`
  - open | gated | invitation-only
- `interaction_type`
  - an open identifier describing the primary interaction (canonical values: vote | comment | deliberate | propose | allocate | signal | hybrid). A process type MAY define additional values.
- `canonical_interaction_type`
  - REQUIRED when `interaction_type` is not one of the canonical values: a process type declaring a non-canonical `interaction_type` MUST also set `canonical_interaction_type` to one of the canonical values above. This is the value consumers, feeds, and exports classify on, so a custom interaction is still legible to a stranger. When `interaction_type` is already canonical the field is omitted and consumers treat the two as equal.
- `disclosure_policy`
  - governs what participant data may be linked and published. It takes **either** one of the canonical shorthand strings **or** a reference to a policy object:
    - `public` — participant identity and content are published together (on-the-record participation). This value shares a spelling with `visibility: public` below and means something different: `disclosure_policy: public` is about what the record says about the participant, `visibility: public` is about who may read the record. A descriptor may legitimately carry both.
    - `pseudonymous` — content is published under a handle that is not linked to the participant's real-world identity
    - `anonymous` — content is published with no participant identifier attached
    - `secret` — participation may be public, but the participant's choice or content is never linked to their identity in stored ledgers or public activities (the secret-ballot case)
    - an object of the form `{"policy_id": "<id>"}`, referencing an **Identity Policy Object** defined in the Civic Identity Specification §8, for disclosure terms the four shorthands do not express. In a descriptor the object form reads: `"disclosure_policy": { "policy_id": "floyd-secret-ballot-v2" }`
    - **What these settings do and do not hide.** All four values govern what is *published* — what other participants, the public, and the activity stream can see. None of them hides the participant from the space that is running the process. To take part, a participant proves who they are to that space, and in v0.1 that space can therefore link the action to them. `anonymous` means "no identifier is attached to the published record," not "nobody knows." A space SHOULD tell participants this in plain words before they act. The limit and the two remedies under consideration are stated in Civic Identity Specification §7.4.
  - The policy MUST be declared before participation begins and MUST be published in the descriptor, so participants know the disclosure terms before acting. See the Civic Activity Specification §7 for how disclosure constrains activity payloads.
- `visibility`
  - who the process intends its results and activity record for. One of:
    - `public` — readable by anyone
    - `participants-only` — readable by those who participated
    - `jurisdiction-only` — readable by those who hold a credential for the process's jurisdiction
  - `visibility` and `disclosure_policy` answer different questions and are set independently: `visibility` is *who may read the record*, `disclosure_policy` is *what the record says about a participant*. A process can be `public` and `secret` at once — anyone may read the tally, nobody may read who voted which way.
  - `visibility` is the **policy** level of a two-level decision. The wire level is the Civic Activity Specification's `meta.visibility`, which has only two values; Section 5 (Phase 6) defines the normative mapping between them.

### 2.4 Actions

Defines what participants can do. Each action MUST be declared using the action form published in the process descriptor (Section 12.1). That form is canonical; it is the only action declaration in this specification:

```json
{
  "name": "submit_vote",
  "input_schema": {
    "type": "object",
    "properties": { "option_id": { "type": "string" } },
    "required": ["option_id"]
  },
  "requires": { "credentials": ["vc:resident:us-va-floyd"] },
  "emits": ["civic.process.vote_submitted"]
}
```

Each action MUST define:
- `name` — the action identifier a caller passes to the action endpoint (Section 12.2)
- `input_schema` — a **JSON Schema (draft 2020-12)** object describing the accepted `input` payload. Input that does not validate against it is rejected with `400`.
- `requires` — the credentials (and any other preconditions) a caller must satisfy for the action to be accepted
- `emits` — the activity types the action emits (Section 7.5)

Validation rules and resulting state changes beyond schema conformance are implemented by the process type's handler (Section 2.5) rather than declared in the descriptor, because they are type-specific logic, not data a consumer can act on.

**Mapping from the earlier `{action, input, output}` vocabulary.** `action` is declared as `name` in the descriptor (the request field a caller sends to the endpoint is still called `action`, and its value is that `name`); `input` is an instance conforming to `input_schema`; `output` has no descriptor field, and an action does not declare its own response shape. Responses are defined once by the action endpoint contract in Section 12.2 — the success body and the shared error model — so that every action on every process type answers in the same way and a consumer needs no per-action knowledge to handle a response.

### 2.5 Process Types, the Registry, and Handlers

Every process instance has a `type` (e.g., `civic.vote`). A process **type** is defined by a **handler** registered with the host space's **process registry**:

- The registry maps type identifiers to handler implementations. Type identifiers use reverse-namespace form (`civic.*` for canonical types; vendors use their own namespace).
- A handler implements, at minimum: state initialization for a new instance, action handling (validate → apply → emit), a read model — a read-only view of the process's current state that a consumer can fetch without understanding the process type's internals — for consumers, and close/terminal-transition behavior for its lifecycle profile.
- The service layer that hosts processes MUST remain type-agnostic: all process-specific logic lives in handlers; creation, dispatch, lifecycle checks, and feeds operate on the registry contract, never on hardcoded type checks.
- Handlers are packaged as **Civic Process Plugins** (Civic Plugin Architecture): a plugin ships a manifest declaring its type(s), trust tier, capabilities, and targeted contract versions. A host installs the plugin, which registers its handler(s).

This registry/handler contract is what makes the process set extensible: a third party adds a process type by implementing the handler contract and shipping a conformant plugin — no changes to the host's core.

---

## 3. Identity Integration

Civic Processes MUST integrate with decentralized identity systems.

### 3.1 Authentication
- Users authenticate via DID-based login (target state)
- Session established at the space or interface level
- During early phases, a host space MAY satisfy this through a stub identity adapter at a **declared assurance level** (Civic Space Specification §7.3); the actor recorded on activities is always the authenticated identity, never taken from the request body

### 3.2 Credential Requirements
Processes define required credentials:

Examples:
- Proof of personhood
- Residency credential
- Organization membership

### 3.3 Credential Verification
- Credentials MUST be:
  - cryptographically valid
  - issued by trusted issuers
  - not expired or revoked

### 3.4 Access Control
- Participation is granted only if credential requirements are met

---

## 4. Lifecycle Model and Lifecycle Profiles

### 4.1 Canonical State Vocabulary

The canonical lifecycle states are:

1. Draft
2. Scheduled
3. Active
4. Closed
5. Finalized

The five-state sequence (Draft → Scheduled → Active → Closed → Finalized) is a **canonical vocabulary, not a mandatory sequence for every process type**. Not every civic interaction is deliberation-shaped: announcements publish and archive; continuous processes stay open until closed by an administrator; ephemeral processes are born active.

### 4.2 Lifecycle Profiles

Every process type declares its **lifecycle profile** in its descriptor. A profile is an ordered subset of the canonical vocabulary plus its transition rules. Named profiles:

| Profile | States | Exports as | Typical use |
|---|---|---|---|
| `deliberative` | draft → scheduled → active → closed → finalized | (canonical throughout) | Votes, assemblies, budgets — the full model (`scheduled` optional where activation is manual) |
| `continuous` | draft → active → closed | (canonical throughout) | Standing conversations, idea boards — closed manually by an administrator |
| `publish` | draft → active → archived | `archived` → `closed` | Announcements, reports — published, later archived |
| `ephemeral` | active → closed | (canonical throughout) | Lightweight interactions born active |

Process types MAY define additional profiles, using only canonical state names plus at most one type-specific terminal alias (like `archived`), which MUST map to a canonical terminal state (`closed` or `finalized`) for export and cross-space consumption. The "Exports as" column above states that mapping for the named profiles: a `publish` process in `archived` is reported as `closed` to any consumer reading the canonical vocabulary, and `archived` is not itself a value of the canonical `status` enum (Section 6.1).

A terminal alias does not get its own activity type. The transition into a profile-specific terminal state MUST emit `civic.process.ended` — the same activity every other terminal transition emits — carrying the alias in its `data` (for example, `"data": { "process": { "type": "civic.announcement" }, "terminal_state": "archived" }`). This is what lets a consumer detect the end of any process without knowing the profile in advance, while still seeing the local name for it.

### 4.3 Minimal Shared Lifecycle Requirements

Regardless of profile, every process type MUST:

- Support a pre-publication state (`draft` or equivalent) unless the profile is `ephemeral`
- Be closable or archivable by an authorized administrator at any time
- Emit a lifecycle activity on **every** terminal transition (no silent archiving)
- Publish its lifecycle profile in the process descriptor (Section 12.1), so consumers know which transitions to expect

### 4.4 State Transitions

Transitions follow the declared profile and are irreversible under normal operation. Each transition MUST emit the corresponding lifecycle activity (Section 7). (Suspension of an in-flight process is an open question — see Section 16.)

The phase model in Sections 5–6 describes the **`deliberative` profile** in full; other profiles execute the subset of phases their states support (e.g., a `publish` profile performs Initiation, Framing, Activation, and Publication only).

---

## 5. Full Civic Process Lifecycle Model

The lifecycle state machine defined in Section 4 describes the system-level states a process moves through. This section defines a richer conceptual lifecycle model that describes the phases of civic work that occur across those states. Together, the state machine and lifecycle phases provide a complete picture: states describe where the process is in the system, and phases describe what is happening in the civic workflow.

The full lifecycle consists of eight phases. Phases 0–7 describe the `deliberative` profile in full. The phases a process MUST support are exactly those permitted by its declared lifecycle profile (Section 4.2); the obligations of each profile are summarised in the conformance table in Section 14. A process on a shorter profile does not skip phases it owes — it never had them. Phase 7 (Feedback / Continuity) arises only on the `deliberative` profile, and there it is RECOMMENDED for process types that produce advisory or binding outcomes and OPTIONAL otherwise (Section 14.1). It is deliberately not a MUST: the phase tracks what happens to an outcome in the world after the process closes, which is often outside the emitting body's control, and a requirement no implementer can reliably satisfy would weaken the requirements that surround it.

### Phase 0: Initiation

**Purpose:** A process is proposed and instantiated. This is the point at which a civic need is identified and a process object is created.

**Key Actions:**
- An authorized actor (government body, organization, or citizen with appropriate credentials) creates a new process instance
- The process type is selected from the process registry (e.g., `civic.vote`, `civic.assembly`, `civic.budget`)
- A unique process ID is assigned

**Required Data:**
- `type` — the process type from the registry
- `created_by` — DID or organization ID of the initiating actor
- `jurisdiction` — the geographic or institutional scope
- `title` and `description` — human-readable identification

**Identity Relationship:** The initiating actor MUST be authenticated via DID. The process records the creator's DID as provenance. If the process is gated or institutional, the creator's organizational credential may also be required.

**Expected Outputs:**
- A process object in `draft` status with metadata populated
- A `civic.process.created` activity emitted

---

### Phase 1: Framing

**Purpose:** The process is configured with rules, options, participation criteria, and timeline. This phase represents the design and scoping of the civic interaction before it becomes available to participants.

**Key Actions:**
- Define participation rules (eligibility requirements, participation mode, interaction type)
- Define available actions and their input/output contracts
- Set the timeline (`starts_at`, `ends_at`)
- Configure options, ballot items, budget categories, or discussion prompts depending on process type
- Set visibility and result publication rules
- Optionally define aggregation method (e.g., plurality, ranked choice, deliberative synthesis)

**Required Data:**
- `eligibility_requirements` — list of required Verifiable Credentials
- `participation_mode` — open, gated, or invitation-only
- `actions` — array of action contracts with schemas
- `starts_at` / `ends_at` — ISO 8601 timestamps
- `aggregation_method` (optional, defaults to process type default)
- `visibility` — `public`, `participants-only`, or `jurisdiction-only` (Section 2.3)
- `disclosure_policy` — a canonical shorthand or a policy reference (Section 2.3)

**Identity Relationship:** Framing is performed by the process creator or delegated administrators. Credential requirements for participants are defined during this phase but not yet enforced.

**Expected Outputs:**
- Fully configured process descriptor ready for activation
- A `civic.process.framed` activity emitted (captures the configuration snapshot)
- Process remains in `draft` status until activation

---

### Phase 2: Activation

**Purpose:** The process transitions from configuration to live availability. Participants can now discover and begin interacting with the process.

**Key Actions:**
- Validate that all required configuration is complete (actions defined, eligibility set, timeline valid)
- Transition process status from `draft` or `scheduled` to `active`
- Publish the process descriptor to the Civic Activity Feed
- Make the process discoverable via the host space's `/.well-known/civic.json` manifest and process listing endpoints

**Required Data:**
- All framing data MUST be finalized
- The current timestamp MUST be at or after `starts_at` (or activation may be manual)

**Identity Relationship:** The activating actor MUST have administrative authority over the process. For scheduled processes, activation may be performed by the system clock, in which case the actor is recorded as the space itself.

**Expected Outputs:**
- Process status set to `active`
- A `civic.process.started` activity emitted
- Process appears in space process listings and Civic Activity Feed

---

### Phase 3: Participation

**Purpose:** Citizens and eligible participants interact with the process by performing defined actions. This is the core engagement phase where civic input is collected.

**Key Actions:**
- Participants authenticate and present required credentials
- Participants perform actions defined by the process (vote, comment, deliberate, propose, allocate budget)
- The host space validates each action against the action contract (input schema, credential requirements, validation rules)
- Each valid action is recorded and emits an event

**Required Data:**
- Participant DID and session token
- Required Verifiable Credentials per the process eligibility rules
- Action input conforming to the defined input schema

**Identity Relationship:** This is the primary phase where identity verification is enforced. Every participating actor MUST be authenticated via DID. Credential requirements MUST be verified before any action is accepted. Depending on process type and privacy configuration, participation may be pseudonymous (DID verified but not publicly linked to action) or transparent (actor DID published with event).

**Expected Outputs:**
- Recorded actions (votes, comments, proposals, etc.)
- Participation events emitted for each action (e.g., `civic.process.vote_submitted`, `civic.process.comment_added`, `civic.process.proposal_created`)
- Running state updated (e.g., vote tallies, comment threads, budget allocations)

---

### Phase 4: Aggregation

**Purpose:** After participation closes, raw inputs are processed into structured results. Aggregation transforms individual actions into collective outputs.

**Key Actions:**
- Close the process to new participation (transition status from `active` to `closed`)
- Apply the defined aggregation method to collected inputs:
  - **Tallying** — for voting processes: count votes, apply weighting if defined, compute margins
  - **Summarization** — for comment or consultation processes: extract themes, cluster positions, identify consensus and dissent
  - **Synthesis** — for deliberative processes: produce a structured summary of deliberation outcomes, areas of agreement, and unresolved tensions
  - **Allocation** — for budgeting processes: compute final budget distributions based on participant input
- Validate aggregation outputs against expected schemas
- Record aggregation metadata (method used, timestamp, any anomalies)

**Required Data:**
- Complete set of recorded participant actions
- Aggregation method configuration from framing phase
- Process type handler's aggregation logic

**Identity Relationship:** Aggregation is typically a system-level operation performed by the host space or a designated aggregation service. The aggregating actor (system or authorized administrator) is recorded for auditability. Individual participant identities are not exposed during aggregation unless the process explicitly requires transparent tallying.

**Expected Outputs:**
- Structured aggregation result (format defined by process type)
- A `civic.process.aggregation_completed` activity emitted with summary payload
- Process remains in `closed` status pending outcome recording

---

### Phase 5: Outcome / Decision

**Purpose:** The aggregated results are interpreted and an outcome is recorded. This phase distinguishes between the data produced by aggregation and the real-world meaning or decision that follows.

**Key Actions:**
- Record the process result as structured data (the direct output of aggregation)
- Optionally record an outcome interpretation:
  - For advisory processes: the recommendation that follows from results
  - For binding processes: the decision enacted
  - For informational processes: the summary or report produced
- Link the outcome to any authorizing body or decision-maker if applicable
- If the process outcome feeds into another system (e.g., a government decision pipeline), record that linkage

**Required Data:**
- Aggregation result from Phase 4
- Outcome type: `advisory`, `binding`, `informational`, or `input` (feeds another process)
- Outcome description (human-readable)
- Decision authority (DID of individual or organization, if applicable)
- Linked downstream process ID (if this outcome feeds into another process)

**Identity Relationship:** If the outcome involves a decision-maker (e.g., a city council ratifying advisory vote results), that actor's DID is recorded with the outcome. For advisory processes where no formal decision-maker exists, the outcome actor is the process itself (space-generated).

**Expected Outputs:**
- A structured outcome record attached to the process
- A `civic.process.outcome_recorded` activity emitted
- Process status transitions to `finalized`

(Publication of the result is Phase 6, not Phase 5: `civic.process.result_published` is emitted there. See Sections 6.2 and 7.6.)

---

### Phase 6: Publication

**Purpose:** Process results and outcomes are made available to participants, the public, and downstream systems. Publication ensures transparency and enables the Civic Activity Feed to distribute outcomes across the network.

**Key Actions:**
- Publish the result and outcome data according to the process's visibility rules
- Emit events to the Civic Activity Feed for network-wide distribution
- Make results available via the process descriptor endpoint (`GET /process/:id`)
- Optionally generate a human-readable result report or summary
- Notify participants (via feed events or space-level notification mechanisms)

**Required Data:**
- Finalized result and outcome from Phases 4–5
- Visibility configuration from framing phase (`public`, `participants-only`, `jurisdiction-only`)

**Process visibility and activity visibility are two levels of the same decision.** The `visibility` value above is the **policy** level: it says who the process intends its results for. The Civic Activity Specification's `meta.visibility` is the **wire** level, and it has exactly two values, `public` and `restricted`. When emitting an activity for a process, a space MUST set `meta.visibility` by this mapping:

| Process `visibility` (policy) | Activity `meta.visibility` (wire) |
|---|---|
| `public` | `public` |
| `participants-only` | `restricted` |
| `jurisdiction-only` | `restricted` |

The wire level is deliberately coarse: it is the only distinction every consumer across the network can be relied on to honour. Enforcing the finer policy distinction — deciding *which* restricted audience may read a given activity — is the emitting space's responsibility.

**Identity Relationship:** Publication respects the privacy configuration established during framing. If participation was pseudonymous, published results MUST NOT deanonymize participants. Result attribution follows the visibility rules defined in the process descriptor.

**Expected Outputs:**
- Publicly accessible result data at the process endpoint
- `civic.process.result_published` event distributed via Civic Activity Feed
- Results consumable by citizen interfaces, dashboards, and downstream processes

---

### Phase 7: Feedback / Continuity

**Purpose:** Track whether outcomes lead to real-world action, enable follow-up processes, and support the transition from episodic participation to continuous civic engagement. This phase closes the loop between civic input and civic impact.

**Key Actions:**
- Record implementation status of outcomes (e.g., was the advisory recommendation adopted? was the budget allocation executed?)
- Link follow-up processes to the original process (e.g., an implementation review vote linked to the original participatory budget)
- Accept structured feedback from participants on the process itself (was it fair? was it accessible? did the outcome reflect input?)
- Publish continuity updates as events to the Civic Activity Feed

**Required Data:**
- `outcome_status` — tracking field: `pending_action`, `in_progress`, `implemented`, `rejected`, `superseded`
- `follow_up_process_ids` — array of linked process IDs (if follow-up processes are created)
- `feedback` (optional) — structured participant feedback on process quality

**Identity Relationship:** Feedback submissions follow the same identity requirements as the original process. Implementation status updates may come from institutional actors (government bodies, organizations) whose authority is verified via organizational credentials. Follow-up process links are created by authorized actors with administrative access.

**Expected Outputs:**
- Updated outcome status on the process record
- `civic.process.feedback_received` events (optional, emitted per feedback submission)
- `civic.process.updated` events reflecting outcome status changes
- Links to follow-up processes visible in the process descriptor

This phase is essential for moving from episodic participation — where citizens vote or comment and never hear what happened — to continuous civic engagement, where every process outcome is tracked, accountable, and connected to what comes next.

---

## 6. Lifecycle Phases and State Machine Mapping

The lifecycle phases defined in Section 5 and the state machine defined in Section 4 operate at different levels of abstraction. The state machine describes discrete system states stored in the `status` field of a process record. The lifecycle phases describe the conceptual workflow — what civic work is being performed, regardless of how the system labels the current state.

### 6.1 Definitions

**Lifecycle States** (Section 4) are the values of the process `status` field: `draft`, `scheduled`, `active`, `closed`, `finalized`. They are mutually exclusive, machine-readable, and control what operations the system permits. A profile-specific terminal alias (such as the `publish` profile's `archived`) may appear locally in place of a canonical terminal state, but exports and cross-space consumers see the canonical value it maps to (Section 4.2).

**Lifecycle Phases** (Section 5) are the conceptual stages of the civic workflow: Initiation, Framing, Activation, Participation, Aggregation, Outcome / Decision, Publication, Feedback / Continuity. They describe purpose and activity. Multiple phases may occur within a single state, and phase boundaries may not align exactly with state transitions.

### 6.2 Phase-to-State Mapping

The mapping below is the `deliberative` profile, which is the only profile that runs all eight phases. A process on another profile occupies only the rows its declared states support, reading each row against its own states — a `publish` process, for instance, has rows 0, 1, 2 and 6 and no others, and its row 6 is reached in `archived` rather than `finalized`, because the profile ends at its terminal alias and never enters `finalized` (Section 4.2).

| Lifecycle Phase | Primary State(s) | Notes |
|---|---|---|
| 0. Initiation | `draft` | Process object is created. Status is set to `draft` upon creation. |
| 1. Framing | `draft` | Configuration and rule-setting occur while the process remains in `draft`. Framing may also continue briefly during `scheduled` if late configuration is permitted by the process type. |
| 2. Activation | `draft` → `active` or `scheduled` → `active` | The state transition itself is the activation event. For processes with a scheduled start, the `draft` → `scheduled` transition occurs during framing, and `scheduled` → `active` occurs at the designated start time. |
| 3. Participation | `active` | All participant actions occur while the process is in `active` state. The `active` state is defined by the participation window. |
| 4. Aggregation | `closed` | Aggregation begins when the process transitions from `active` to `closed`. It completes before the process moves to `finalized`. |
| 5. Outcome / Decision | `closed` → `finalized` | Outcome recording occurs in the `closed` state after aggregation. The transition to `finalized` marks the completion of outcome recording. |
| 6. Publication | `finalized` | Publication occurs after the process reaches `finalized` status. Results are made available and events are distributed. |
| 7. Feedback / Continuity | `finalized` | Feedback and continuity tracking occur after finalization. The process remains in `finalized` state — this phase does not introduce a new state, but adds ongoing activity to a completed process. |

### 6.3 Key Principles

Multiple phases may execute within a single state. For example, both Initiation and Framing occur during `draft`. The state machine does not need to be extended to accommodate every phase — phases represent workflow granularity, states represent system checkpoints.

State transitions are irreversible under normal operation. A process that has moved from `active` to `closed` cannot return to `active`. Lifecycle phases respect this constraint: aggregation cannot revert to participation.

Phase completion does not always trigger a state transition. Framing completes within `draft` without changing the state. Publication completes within `finalized` without changing the state. Only phases that correspond to a system checkpoint (Activation, Aggregation close, Outcome finalization) trigger state transitions.

The Feedback / Continuity phase is unique in that it operates on a `finalized` process. The state machine considers `finalized` a terminal state, but the lifecycle model recognizes that civic work continues after formal completion.

---

## 7. Activity Integration

All Civic Processes MUST publish standardized **Civic Activities** (Civic Activity Specification). The full activity envelope — including `id`, `version`, `source`, and `meta.visibility` — is defined there; this section defines which activities a process emits and when. (The v0.1 wire field is `event_type`; see Civic Activity Specification §14.)

### 7.1 Activity Contract

Each process action or lifecycle transition MUST result in one or more activities, emitted through the host space's single emission path — the one code path through which a space publishes activities, so that no action can change state without also being announced.

Every activity carries the full envelope of the Civic Activity Specification §2–3, including:
- process_id
- event_type
- timestamp
- actor (a system identifier such as `system:auto-close` for system-initiated transitions)
- jurisdiction
- action_url
- id, version, source, data (with `data.process.type`), meta.visibility

### 7.2 Required Lifecycle Activity Types
- `civic.process.created`
- `civic.process.updated`
- `civic.process.started`
- `civic.process.ended` — emitted on the transition into a terminal state (`active` → `closed`, or the profile's terminal transition)
- `civic.process.result_published` — the per-profile obligation for this type is given in Section 14.1. A `continuous` process has no single moment at which its results are complete, so it is not required to emit it.

### 7.3 Participation Activities

Every participant action MUST emit an activity (per 7.1). Where the action's semantics match a canonical participation type, that type MUST be used:
- `civic.process.vote_submitted`
- `civic.process.comment_added`
- `civic.process.proposal_created`

Actions with no matching canonical type emit `civic.process.action_taken` or a manifest-declared extension type (Civic Activity Specification §4.5). Participation activity **payloads** are constrained by the process's `disclosure_policy` (Section 2.3): a `secret` process never places the participant's choice or content in activities more visible than the policy allows. Where `disclosure_policy` is `anonymous` or `pseudonymous`, the constraint reaches the envelope as well as the payload: the emitting space SHOULD set `actor` to an identifier scoped to this process rather than the participant's DID, and MUST NOT publish the mapping between the two. This is the one case in which the Civic Activity Specification's "where the actor has a DID, the emitter MUST use it" rule is overridden, and the descriptor's declared `disclosure_policy` is what authorises the override.

### 7.4 Lifecycle Phase Events

The following event types correspond to the full lifecycle model defined in Section 5. These events extend the required event set. Each MUST be emitted by any process whose declared lifecycle profile includes the phase it belongs to; a process whose profile has no such phase never emits it (Section 7.6).

Every payload below follows the `data` namespacing rule of Civic Activity Specification §5: `data` carries the mandatory `process` discriminator plus at most one further key naming the subject of the activity (`framing`, `aggregation`, `outcome`, `feedback`). Phase-specific fields live inside that subject object, never at `data`'s top level.

#### `civic.process.framed`

Emitted when the framing phase is complete and the process configuration is finalized.

```json
{
  "id": "uuid",
  "version": "1.0",
  "event_type": "civic.process.framed",
  "process_id": "string",
  "timestamp": "ISO8601",
  "actor": "DID of framing actor",
  "jurisdiction": "string",
  "action_url": "string",
  "source": { "hub_id": "string", "hub_url": "url", "space_id": "did (recommended)" },
  "data": {
    "process": { "type": "civic.vote" },
    "framing": {
      "eligibility_requirements": ["array of credential types"],
      "participation_mode": "open | gated | invitation-only",
      "interaction_type": "vote | comment | deliberate | propose | allocate | signal | hybrid",
      "disclosure_policy": "public | pseudonymous | anonymous | secret | { policy_id }",
      "visibility": "public | participants-only | jurisdiction-only",
      "starts_at": "ISO8601",
      "ends_at": "ISO8601",
      "aggregation_method": "string (optional)",
      "action_count": "number of defined actions"
    }
  },
  "meta": { "visibility": "public" }
}
```

`hub_id` and `hub_url` are v0.1 wire names retained for compatibility. They carry the emitting **space**, whatever its scope — a Civic Hub, a Citizen Dashboard, or a Representative Space. `source.space_id` is the forward-looking field (Civic Activity Specification §2).

#### `civic.process.aggregation_completed`

Emitted when the aggregation phase has finished processing participant inputs into structured results.

```json
{
  "id": "uuid",
  "version": "1.0",
  "event_type": "civic.process.aggregation_completed",
  "process_id": "string",
  "timestamp": "ISO8601",
  "actor": "DID of aggregating actor or space system ID",
  "jurisdiction": "string",
  "action_url": "string",
  "source": { "hub_id": "string", "hub_url": "url", "space_id": "did (recommended)" },
  "data": {
    "process": { "type": "civic.vote" },
    "aggregation": {
      "method": "string",
      "participant_count": "number",
      "result_summary": "string (human-readable summary)",
      "result_type": "tally | summary | synthesis | allocation",
      "anomalies": "array of strings (optional)"
    }
  },
  "meta": { "visibility": "public" }
}
```

#### `civic.process.outcome_recorded`

Emitted when the outcome or decision has been formally recorded for the process.

```json
{
  "id": "uuid",
  "version": "1.0",
  "event_type": "civic.process.outcome_recorded",
  "process_id": "string",
  "timestamp": "ISO8601",
  "actor": "DID of outcome author or decision authority",
  "jurisdiction": "string",
  "action_url": "string",
  "source": { "hub_id": "string", "hub_url": "url", "space_id": "did (recommended)" },
  "data": {
    "process": { "type": "civic.vote" },
    "outcome": {
      "type": "advisory | binding | informational | input",
      "description": "string",
      "decision_authority": "DID (optional, for binding outcomes)",
      "linked_process_id": "string (optional, if outcome feeds another process)"
    }
  },
  "meta": { "visibility": "public" }
}
```

#### `civic.process.feedback_received`

Emitted when a participant submits feedback on a finalized process. A process that accepts participant feedback MUST emit this activity once per feedback submission (Section 11.4). A process whose profile has no Feedback / Continuity phase never emits it.

```json
{
  "id": "uuid",
  "version": "1.0",
  "event_type": "civic.process.feedback_received",
  "process_id": "string",
  "timestamp": "ISO8601",
  "actor": "DID of feedback submitter",
  "jurisdiction": "string",
  "action_url": "string",
  "source": { "hub_id": "string", "hub_url": "url", "space_id": "did (recommended)" },
  "data": {
    "process": { "type": "civic.vote" },
    "feedback": {
      "type": "process_quality | outcome_satisfaction | accessibility",
      "content": "string",
      "rating": "number (optional, 1-5 scale)"
    }
  },
  "meta": { "visibility": "public" }
}
```

### 7.5 Action → Event Mapping

Each defined action MUST specify which events it emits.

Example:

- `submit_vote` → emits `civic.process.vote_submitted`

### 7.6 Lifecycle Phase → Event Mapping

The table below maps the **`deliberative`** profile — the full eight-phase model of Sections 5–6 — to the activities each phase emits. Every phase a process *actually runs* MUST emit at least one activity; there are no silent phases.

| Lifecycle Phase | Required Activity/Activities |
|---|---|
| 0. Initiation | `civic.process.created` |
| 1. Framing | `civic.process.framed` |
| 2. Activation | `civic.process.started` |
| 3. Participation | At least one participation activity per action taken (e.g., `civic.process.vote_submitted`) |
| 4. Aggregation | `civic.process.ended` (on the `active` → `closed` transition) followed by `civic.process.aggregation_completed` |
| 5. Outcome / Decision | `civic.process.outcome_recorded` |
| 6. Publication | `civic.process.result_published` |
| 7. Feedback / Continuity | `civic.process.feedback_received` (per submission) and/or `civic.process.updated` (for outcome status changes) |

**Profiles other than `deliberative`.** Only the `deliberative` profile runs all eight phases. A process on any other profile (Section 4.2 — `continuous`, `publish`, `ephemeral`) emits activities only for the phases it actually runs: an announcement on the `publish` profile has no Aggregation or Feedback phase, so it never emits `civic.process.aggregation_completed`. Two emissions are common to every profile regardless of shape: `civic.process.created` when the instance comes into being, and a lifecycle activity on the terminal transition (`civic.process.ended` or its profile-specific equivalent, per Section 4.3). Those two are what let any consumer track any process from birth to end without knowing its profile in advance.

### 7.7 Distribution

Events SHOULD be:
- publishable via ActivityPub or equivalent
- consumable by feeds and dashboards

---

## 8. Process Observability

Civic processes derive their legitimacy from transparency. A process that cannot be observed cannot be trusted. This section defines the observability requirements that ensure every Civic Process is externally verifiable across its entire lifecycle.

### 8.1 Core Principle

Every lifecycle phase a process actually runs MUST produce at least one event or observable output. There MUST be no "dark" phases where the process transitions without producing externally visible evidence. (A phase the process's profile does not include is not a dark phase — it simply never happens; see Section 7.6.)

### 8.2 Observable Progression

Lifecycle progression MUST be externally visible through at least one of the following mechanisms:
- Events published to the Civic Activity Feed
- State changes reflected in the process descriptor at `GET /process/:id`
- Status updates available via the space's activity feed at `GET /events`

An external observer (another space, a citizen interface, a monitoring service) MUST be able to reconstruct the current lifecycle phase of any process by reading its event history and current state.

### 8.3 Observability and Transparency

*This section is non-normative.*

Civic Processes are accountable to their participants and jurisdictions. Observability supports transparency by ensuring that:
- Every phase transition is recorded and timestamped
- The rules under which a process operates (framing) are published before participation begins
- Aggregation methods and results are auditable
- Outcomes are publicly attributable to the data that produced them

### 8.4 Observability and Legitimacy

*This section is non-normative.*

Process legitimacy depends on verifiability. Participants, oversight bodies, and the public must be able to confirm that:
- The process followed its declared rules
- Participation was limited to eligible actors
- Aggregation was performed correctly
- The outcome faithfully reflects the aggregated input

Observability is the technical foundation for this verification. Without it, civic trust cannot be established.

### 8.5 Observability and Interoperability

*This section is non-normative.*

In a federated network of Civic Spaces, observability enables interoperability by providing a shared protocol for monitoring processes across jurisdictions. A space in one jurisdiction can observe the progress and results of processes in another jurisdiction by consuming their event streams. This enables:
- Cross-space dashboards and citizen interfaces
- Network-wide civic activity feeds
- Automated monitoring of process health and compliance
- Credential issuance triggered by process outcomes (e.g., participation credentials)

### 8.6 Minimum Observability Requirements (v0.2)

A compliant process MUST:
- Emit events for all state transitions (Sections 7.2 and 7.4)
- Reflect current status in the process descriptor endpoint
- Make the event history for the process available via the space's activity feed
- Not suppress or delay events for any phase that has completed

---

## 9. Aggregation Phase

This section provides detailed requirements for the Aggregation phase (Phase 4). Aggregation is required of every process whose lifecycle profile includes it — in practice the `deliberative` profile (Section 4.2). A profile with no Aggregation phase does not perform it and emits no aggregation activities.

### 9.1 Definition

Aggregation is the phase in which raw participant inputs are transformed into structured collective outputs. It is distinct from participation: participation collects individual actions, while aggregation produces a collective result.

### 9.2 Aggregation Methods

For a process that collects participant input, the aggregation method MUST be defined during the Framing phase and MUST be published as part of the process descriptor, so that participants know before acting how their input will be turned into a result. A process that collects no participant input has nothing to aggregate and declares no method, and the descriptor completeness rule of Section 12.1 does not call for one. The following aggregation methods are defined:

**Tallying** applies to processes where participants select from defined options (voting, polling, ranked choice). Tallying produces quantitative results: counts, percentages, rankings, or weighted scores.

**Summarization** applies to processes where participants contribute unstructured or semi-structured input (public consultations, comment periods). Summarization produces a structured representation of themes, positions, and frequency.

**Synthesis** applies to deliberative processes where the goal is not to count positions but to identify areas of agreement, disagreement, and emergent understanding. Synthesis produces a narrative or structured report capturing the state of deliberation.

**Allocation** applies to budgeting processes where participants distribute resources across categories. Allocation produces a final distribution based on participant input and the defined allocation algorithm.

### 9.3 Aggregation Outputs

Aggregation outputs MUST be structured and publishable. They MUST conform to a schema defined by the process type handler. At minimum, an aggregation output MUST include:
- `method` — the aggregation method used
- `participant_count` — the number of unique participants whose input was aggregated
- `timestamp` — when aggregation was performed
- `result` — the structured result data (format varies by method)

### 9.4 Aggregation Integrity

The aggregation phase MUST NOT alter, filter, or selectively exclude participant inputs unless the exclusion criteria were defined during framing (e.g., duplicate vote prevention, eligibility re-verification). Any exclusions MUST be recorded in the aggregation metadata and included in the `civic.process.aggregation_completed` event.

---

## 10. Outcome / Decision Layer

This section provides detailed requirements for the Outcome / Decision phase (Phase 5).

### 10.1 Process Result vs. Outcome

A **process result** is the structured data produced by aggregation — vote tallies, budget allocations, deliberation summaries. It is objective and machine-readable.

An **outcome** is the real-world interpretation or effect of that result — a recommendation adopted, a budget enacted, a policy changed, or a decision recorded. Outcomes may be determined by human decision-makers acting on process results.

Not every process produces a formal outcome. Some processes are purely informational, and their result is the outcome. But the distinction matters for accountability: when a process produces a result that a decision-maker then acts on (or ignores), both the result and the decision should be recorded.

### 10.2 Outcome Types

- `advisory` — the process produced a recommendation. A decision-maker may or may not act on it. The recommendation and any subsequent decision are tracked.
- `binding` — the process produced a decision that is enacted directly. The result is the outcome.
- `informational` — the process produced a report, summary, or dataset. No action is expected.
- `input` — the process produced output that feeds into another Civic Process (e.g., a proposal phase that feeds into a voting phase). The downstream process ID is recorded.

### 10.3 Outcome Tracking

Outcomes SHOULD include a status field that tracks real-world follow-through:
- `pending_action` — outcome recorded, awaiting implementation
- `in_progress` — implementation underway
- `implemented` — outcome fully enacted
- `rejected` — decision-maker chose not to act on the result
- `superseded` — a subsequent process or decision replaced this outcome

This status field is updated via `civic.process.updated` events throughout the Feedback / Continuity phase.

### 10.4 Decision Authority

For processes with `advisory` or `binding` outcome types, the decision authority SHOULD be recorded as a DID (individual or organizational). This creates an auditable link between civic input and institutional action.

---

## 11. Feedback / Continuity Phase

This section provides detailed requirements for the Feedback / Continuity phase (Phase 7).

### 11.1 Purpose

Most civic participation today is episodic: citizens vote, comment, or attend a hearing, and then hear nothing about what happened. The Feedback / Continuity phase ensures that Civic Processes do not end at publication but remain connected to their downstream effects.

### 11.2 Outcome Tracking

Every process with an `advisory`, `binding`, or `input` outcome type SHOULD maintain an `outcome_status` field (see Section 10.3) that is updated as real-world implementation progresses. Each status change MUST emit a `civic.process.updated` event.

### 11.3 Process Linking

A Civic Process MAY link to follow-up processes via the `follow_up_process_ids` field. This enables chains of civic activity:
- A participatory budget process → an implementation review process
- A public consultation → a revised proposal process → a final vote
- A citizen assembly → a recommendation report → a council ratification vote

Linked processes inherit the `jurisdiction` of the parent process unless explicitly overridden. The process descriptor at `GET /process/:id` MUST include links to follow-up processes when they exist.

### 11.4 Participant Feedback

Processes MAY accept structured feedback from participants after finalization. Feedback captures participants' assessment of the process itself — its fairness, accessibility, clarity, and perceived impact. Feedback submissions emit `civic.process.feedback_received` events and follow the same identity and credential requirements as the original process.

### 11.5 Continuity as a Design Principle

*This section is non-normative.*

This phase is not optional overhead. It is essential for the Civic.Social ecosystem's core value proposition: transforming civic participation from isolated events into connected, accountable governance workflows. Spaces that implement the full lifecycle model, including feedback and continuity, provide citizens with a fundamentally different experience — one where their input has visible, traceable impact.

---

## 12. Interface Contract

Civic Processes MUST expose interfaces for integration.

### 12.1 Process Descriptor

This object is the **Civic Process Descriptor** named in the Terminology Glossary; this specification calls it "the descriptor" throughout.

Each process MUST publish a descriptor at `GET /process/:id`. The descriptor is the self-contained, protocol-native representation of the process — everything a consumer needs to display it, verify eligibility, and route actions:

```json
{
  "id": "process-123",
  "type": "civic.vote",
  "title": "Library Expansion Vote",
  "description": "string",
  "jurisdiction": "us-va-floyd",
  "created_by": "did:example:creator",
  "status": "active",
  "lifecycle_profile": "deliberative",
  "starts_at": "ISO8601",
  "ends_at": "ISO8601",
  "participation_mode": "gated",
  "interaction_type": "vote",
  "canonical_interaction_type": "string (REQUIRED only when interaction_type is non-canonical, Section 2.3)",
  "disclosure_policy": "secret",
  "visibility": "public",
  "aggregation_method": "plurality",
  "actions": [
    {
      "name": "submit_vote",
      "input_schema": {
        "type": "object",
        "properties": { "option_id": { "type": "string" } },
        "required": ["option_id"]
      },
      "requires": { "credentials": ["vc:resident:us-va-floyd"] },
      "emits": ["civic.process.vote_submitted"]
    }
  ],
  "requires": {
    "credentials": ["vc:resident:us-va-floyd"]
  },
  "endpoints": {
    "view": "https://example.org/process/123",
    "action": "https://example.org/process/123/action"
  },
  "result": "object | null (populated per visibility rules after aggregation)",
  "outcome": "object | null (populated when recorded, Section 10)",
  "follow_up_process_ids": []
}
```

`input_schema` is a **JSON Schema (draft 2020-12)** object; that is the declared schema language for this specification, and it is what a consumer validates input against before calling the action endpoint (Section 2.4).

Descriptor completeness rule: **every property this specification requires a process to publish** — current status (8.2), lifecycle profile (4.3), disclosure policy (2.3), visibility (2.3), canonical interaction type where the declared interaction type is non-canonical (2.3), aggregation method (9.2), action contracts with emitted activity types (2.4, 7.5), and follow-up links (11.3) — appears in the descriptor. A consumer never needs out-of-band knowledge to interpret a process.

### 12.2 Required Endpoints

- `POST /process` — create a process instance. Request body: `{ "type": "civic.vote", "metadata": { title, description, jurisdiction }, "lifecycle": { starts_at, ends_at }, "participation": { eligibility_requirements, participation_mode, disclosure_policy }, ...type-specific configuration }`. The creating actor comes from the authenticated context. Response: the created process descriptor (status `draft`). Creation authority is a space policy (which actors may initiate which types, and whether creation passes through a review flow) enforced at the authorization seam — the single point where a space decides whether a caller may do a thing, kept separate from the process logic so the rule can change without touching any process type — review-vs-auto-approve is policy configuration, not a separate API.
- `GET /process/:id` — the descriptor above. Pre-publication processes (`draft`, in-review) MUST NOT be readable by non-privileged callers. The reason is the same one that governs restricted feeds: the existence of a draft process can itself disclose that a jurisdiction is considering something sensitive, before it has decided to.
- `POST /process/:id/action` — execute an action. Request body: `{ "action": "submit_vote", "input": { ...per the action's input schema } }`. The actor is the authenticated identity; a body-supplied actor MUST be rejected. Response on success: `{ "status": "success", "timestamp": "ISO8601", "data": { ...type-specific result of the action, may be empty } }`. Actions do not declare their own response shape (2.4); this contract is the response shape for every action on every process type. Error model: `400` invalid input (schema mismatch), `401` unauthenticated, `403` eligibility/credential failure or process not accepting actions in its current state, `404` unknown process, `409` duplicate where the action is once-per-participant. Error bodies: `{ "status": "error", "code": "string", "message": "string" }`.
- `GET /process` — list processes (read layer; supports filtering by type and status).

### 12.3 Deep Linking
- Web URL required
- Optional mobile deep links

---

## 13. Host Space Responsibilities

Civic Spaces integrating processes MUST:

- Authenticate users via the identity layer (through the identity adapter)
- Verify credentials before participation
- Host or embed process interfaces
- Publish process activities to the Civic Activity Feed layer through the single emission path
- Enforce the process's disclosure policy at the activity layer

---

## 14. Minimal Compliance Requirements

To be compliant with v0.2, a Civic Process MUST:

- Define metadata, lifecycle profile, and disclosure policy
- Integrate with identity for gated participation
- Emit standardized activities (full envelope, per Section 7)
- Provide a complete process descriptor (Section 12.1)
- Support the endpoints required by its declared profile (Section 12.2, Section 14.1)

### 14.1 Conformance by Lifecycle Profile

The individual requirements of this specification are stated where the concept they belong to is defined, which means they are spread across Sections 2, 4, 7, 8, 9, 10 and 12. The table below gathers them: find the column for the lifecycle profile your process type declares (Section 4.2) and read down it to see exactly what you owe.

**Required** means MUST. **Recommended** means SHOULD. **Optional** means MAY — permitted, and a conformant consumer MUST tolerate its absence. **N/A** means the profile has no such phase, so the requirement does not arise; a process is not non-conformant for never emitting it.

| Requirement | `deliberative` | `continuous` | `publish` | `ephemeral` | Section |
|---|---|---|---|---|---|
| Pre-publication state (`draft` or equivalent) | Required | Required | Required | N/A (born active) | 4.3 |
| Declare and publish lifecycle profile in descriptor | Required | Required | Required | Required | 4.3, 12.1 |
| Declare and publish `disclosure_policy` before participation | Required | Required | Required | Required | 2.3 |
| Declare `visibility` in the descriptor and map it to `meta.visibility` on every emission | Required | Required | Required | Required | 2.3, 5 (Phase 6) |
| Emit `civic.process.created` | Required | Required | Required | Required | 7.2, 7.6 |
| Emit `civic.process.framed` | Required | Required | Required | N/A | 7.4, 7.6 |
| Emit `civic.process.started` on activation | Required | Required | Required | Optional (`created` marks the birth into `active`) | 7.2, 7.6 |
| Emit a participation activity for every participant action | Required | Required | N/A (no participation phase) | Required | 7.3 |
| Emit lifecycle activity on the terminal transition (`civic.process.ended`, carrying any profile alias) | Required | Required | Required | Required | 4.2, 4.3, 7.2 |
| Aggregation with structured, publishable output + `civic.process.aggregation_completed` | Required | Optional | N/A | Optional | 7.4, 9 |
| Outcome record with outcome type + `civic.process.outcome_recorded` | Required for advisory/binding outcomes; Optional otherwise | Optional | N/A | Optional | 7.4, 10 |
| Result publication + `civic.process.result_published` | Required | Optional | Required | Optional | 7.2, 7.6 |
| Feedback / continuity: `outcome_status` updates and `civic.process.feedback_received` | Recommended for advisory/binding outcomes; Optional otherwise | N/A | N/A | N/A | 7.4, 11 |
| Publish a complete descriptor at `GET /process/:id` | Required | Required | Required | Required | 12.1 |
| Support `POST /process` (creation) | Required | Required | Required | Required | 12.2 |
| Support `POST /process/:id/action` (action endpoint) | Required | Required | N/A | Required | 12.2 |
| Identity integration: authenticate the acting party, and verify credentials where participation is gated | Required | Required | Required | Required | 3, 13 |
| Minimum observability: no dark phases, state in descriptor, history in the feed | Required | Required | Required | Required | 8.1, 8.6 |

Two rows of this table hold for every profile without exception, and consumers may rely on them; Section 7.6 states which two and why.

### 14.2 Full Lifecycle Compliance

Processes with the `deliberative` profile that produce advisory or binding outcomes MUST additionally:

- Support aggregation with structured, publishable outputs (Section 9)
- Record outcomes with type classification (Section 10)
- Meet the minimum observability requirements defined in Section 8.6

### 14.3 Conformance Phasing

Specifications in this ecosystem define the target state; reference implementations converge through the pilot program. The reference implementation currently registers all process types through a uniform registry with one canonical process record, one creation path, one type-agnostic close mechanism, and one feed classifier; the unified action dispatcher (all participation actions through `POST /process/:id/action`), full descriptor completeness, and DID-based identity are targeted through the Civic Process Plugin and Civic Identity pilots.

---

## 15. Out of Scope (v0.2)

The following are intentionally excluded:

- UI/UX design
- Moderation systems
- Ranking algorithms
- Messaging systems
- Advanced governance models

---

## 16. Future Extensions (post-v0.2)

- Delegative democracy (liquid voting)
- Reputation systems
- Advanced credential logic
- Multi-stage processes
- Cross-process composition
- **Suspension semantics (open question).** The canonical vocabulary has no `suspended` state and transitions are irreversible under normal operation, so a process halted mid-stream by external authority — a court order, a discovered eligibility defect — must today either remain `active` behind an internal flag or close and spawn a linked follow-up instance. Neither is specified. Pilot experience should determine whether suspension warrants a canonical state with the ecosystem's only reversible transition (`suspended` ⇄ `active`), or a documented pattern over the existing vocabulary.

---

## 17. Process as a First-Class Interoperable Object

*This section is non-normative.* It explains the design intent behind the requirements above; it introduces no new ones.

A Civic Process is not merely a data structure or an API endpoint. It is the fundamental unit of civic interaction in the Civic.Social ecosystem, and it is designed to operate as a first-class interoperable object across a federated network of spaces.

### 17.1 Identity-Aware

Every Civic Process is anchored in the decentralized identity layer. Creators, participants, and decision-makers are identified by DIDs. Eligibility is enforced through Verifiable Credentials. This means a process's provenance, participation record, and outcomes are cryptographically verifiable — not dependent on any single space's authority.

### 17.2 Lifecycle-Driven

Every process follows a defined lifecycle with observable phases and state transitions. The lifecycle model ensures that processes are not opaque black boxes but transparent, auditable sequences of civic work. Any observer can determine what phase a process is in, what has happened, and what is expected to happen next.

### 17.3 Event-Emitting

Every significant action and transition in a Civic Process produces a standardized event. These events are the distribution mechanism: they flow into the Civic Activity Feed, enabling citizen interfaces, dashboards, monitoring systems, and other spaces to track and respond to civic activity in real time. Events are the connective tissue of the federated network.

### 17.4 Portable Across Spaces

A Civic Process descriptor contains everything needed to understand and interact with the process: its type, rules, actions, endpoints, and credential requirements. This descriptor is self-contained and protocol-native. A space that receives a process descriptor from another space can display the process, verify credentials, and route participant actions without any out-of-band coordination. This portability is what makes federation possible — processes are not locked to the space that created them.

### 17.5 Composable

Through the Feedback / Continuity phase and process linking, Civic Processes can be composed into chains, sequences, and networks of civic activity. A proposal process can spawn a vote. A vote can trigger an implementation review. A budget allocation can link to quarterly audits. This composability transforms isolated civic acts into sustained governance workflows.

---

## Summary

*This section is non-normative.*

This specification defines Civic Processes as the core interactive unit of the Civic.Social ecosystem.

Processes are:
- Identity-aware
- Event-driven
- Interoperable

They follow a declared lifecycle profile — at its fullest, from initiation through framing, participation, aggregation, outcome recording, publication, and feedback — that ensures every civic interaction is transparent, observable, and connected to real-world impact.

They serve as the foundation for building a modular, federated civic participation infrastructure that supports not just individual civic events, but a continuous, accountable network of interoperable civic process lifecycles.
