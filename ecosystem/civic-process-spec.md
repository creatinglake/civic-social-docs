---
status: stable
last-reviewed: 2026-04-19
owners: [adam]
version: 0.1
---

# Civic Process Specification v0.1

## Purpose

Define a standard, interoperable model for **Civic Processes** within the Civic.Social ecosystem.

A Civic Process is a structured, stateful interaction that enables participation between citizens, organizations, and institutions.

This specification defines:
- Process lifecycle
- Identity requirements
- Event model integration
- Interface and integration contracts

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

Every Civic Process must define:

### 2.1 Metadata
- `id` (UUID)
- `type` (e.g., civic.vote, civic.assembly)
- `title`
- `description`
- `jurisdiction`
- `created_by` (DID or organization ID)
- `created_at`

### 2.2 Lifecycle
- `status` (draft | scheduled | active | closed | finalized)
- `starts_at`
- `ends_at`

### 2.3 Participation Rules
- `eligibility_requirements`
  - list of required credentials (e.g., vc:resident:us-va-floyd)
- `participation_mode`
  - open | gated | invitation-only
- `interaction_type`
  - vote | comment | deliberate | hybrid

### 2.4 Actions
Defines what participants can do:

Each action must follow a defined contract:

```json
{
  "action": "submit_vote",
  "input": {
    "option_id": "string"
  },
  "output": {
    "status": "success | error",
    "timestamp": "ISO8601"
  }
}
```

Each action must define:
- input schema
- output schema
- required credentials
- validation rules
- resulting state changes (if applicable)

---

## 3. Identity Integration

Civic Processes must integrate with decentralized identity systems.

### 3.1 Authentication
- Users authenticate via DID-based login
- Session established at hub or interface level

### 3.2 Credential Requirements
Processes define required credentials:

Examples:
- Proof of personhood
- Residency credential
- Organization membership

### 3.3 Credential Verification
- Credentials must be:
  - cryptographically valid
  - issued by trusted issuers
  - not expired or revoked

### 3.4 Access Control
- Participation is granted only if credential requirements are met

---

## 4. Lifecycle Model

A Civic Process follows this lifecycle:

1. Draft
2. Scheduled
3. Active
4. Closed
5. Finalized

### State Transitions
- Draft → Scheduled
- Scheduled → Active
- Active → Closed
- Closed → Finalized

Each transition may emit events.

---

## 5. Full Civic Process Lifecycle Model

The lifecycle state machine defined in Section 4 describes the system-level states a process moves through. This section defines a richer conceptual lifecycle model that describes the phases of civic work that occur across those states. Together, the state machine and lifecycle phases provide a complete picture: states describe where the process is in the system, and phases describe what is happening in the civic workflow.

The full lifecycle consists of eight phases. Every Civic Process MUST support at least Phases 0 through 6. Phase 7 (Feedback / Continuity) is strongly recommended and MUST be supported for any process type that produces advisory or binding outcomes.

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
- A `civic.process.created` event emitted

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

**Identity Relationship:** Framing is performed by the process creator or delegated administrators. Credential requirements for participants are defined during this phase but not yet enforced.

**Expected Outputs:**
- Fully configured process descriptor ready for activation
- A `civic.process.framed` event emitted (captures the configuration snapshot)
- Process remains in `draft` status until activation

---

### Phase 2: Activation

**Purpose:** The process transitions from configuration to live availability. Participants can now discover and begin interacting with the process.

**Key Actions:**
- Validate that all required configuration is complete (actions defined, eligibility set, timeline valid)
- Transition process status from `draft` or `scheduled` to `active`
- Publish the process descriptor to the Civic Activity Feed
- Make the process discoverable via the hub's `/.well-known/civic.json` manifest and process listing endpoints

**Required Data:**
- All framing data must be finalized
- The current timestamp must be at or after `starts_at` (or activation may be manual)

**Identity Relationship:** The activating actor must have administrative authority over the process. For scheduled processes, activation may be performed by the system clock, in which case the actor is recorded as the hub itself.

**Expected Outputs:**
- Process status set to `active`
- A `civic.process.started` event emitted
- Process appears in hub process listings and Civic Activity Feed

---

### Phase 3: Participation

**Purpose:** Citizens and eligible participants interact with the process by performing defined actions. This is the core engagement phase where civic input is collected.

**Key Actions:**
- Participants authenticate and present required credentials
- Participants perform actions defined by the process (vote, comment, deliberate, propose, allocate budget)
- The hub validates each action against the action contract (input schema, credential requirements, validation rules)
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

**Identity Relationship:** Aggregation is typically a system-level operation performed by the hub or a designated aggregation service. The aggregating actor (system or authorized administrator) is recorded for auditability. Individual participant identities are not exposed during aggregation unless the process explicitly requires transparent tallying.

**Expected Outputs:**
- Structured aggregation result (format defined by process type)
- A `civic.process.aggregation_completed` event emitted with summary payload
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

**Identity Relationship:** If the outcome involves a decision-maker (e.g., a city council ratifying advisory vote results), that actor's DID is recorded with the outcome. For advisory processes where no formal decision-maker exists, the outcome actor is the process itself (hub-generated).

**Expected Outputs:**
- A structured outcome record attached to the process
- A `civic.process.outcome_recorded` event emitted
- Process status transitions to `finalized`
- A `civic.process.result_published` event emitted when results are made publicly visible

---

### Phase 6: Publication

**Purpose:** Process results and outcomes are made available to participants, the public, and downstream systems. Publication ensures transparency and enables the Civic Activity Feed to distribute outcomes across the network.

**Key Actions:**
- Publish the result and outcome data according to the process's visibility rules
- Emit events to the Civic Activity Feed for network-wide distribution
- Make results available via the process descriptor endpoint (`GET /process/:id`)
- Optionally generate a human-readable result report or summary
- Notify participants (via feed events or hub-level notification mechanisms)

**Required Data:**
- Finalized result and outcome from Phases 4–5
- Visibility configuration from framing phase (`public`, `participants-only`, `jurisdiction-only`)

**Identity Relationship:** Publication respects the privacy configuration established during framing. If participation was pseudonymous, published results must not deanonymize participants. Result attribution follows the visibility rules defined in the process descriptor.

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

**Lifecycle States** (Section 4) are the values of the process `status` field: `draft`, `scheduled`, `active`, `closed`, `finalized`. They are mutually exclusive, machine-readable, and control what operations the system permits.

**Lifecycle Phases** (Section 5) are the conceptual stages of the civic workflow: Initiation, Framing, Activation, Participation, Aggregation, Outcome / Decision, Publication, Feedback / Continuity. They describe purpose and activity. Multiple phases may occur within a single state, and phase boundaries may not align exactly with state transitions.

### 6.2 Phase-to-State Mapping

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

## 7. Event Integration (Activity Layer)

All Civic Processes must publish standardized events.

### 7.1 Event Contract

Each process action or lifecycle transition MUST result in one or more events.

Events must include:
- process_id
- event_type
- timestamp
- actor (optional for system events)
- jurisdiction
- action_url

### 7.2 Required Event Types
- `civic.process.created`
- `civic.process.updated`
- `civic.process.started`
- `civic.process.ended`
- `civic.process.result_published`

### 7.3 Participation Events (Optional)
- `civic.process.vote_submitted`
- `civic.process.comment_added`
- `civic.process.proposal_created`

### 7.4 Lifecycle Phase Events

The following event types correspond to the full lifecycle model defined in Section 5. These events extend the required event set and MUST be emitted by processes that implement the full lifecycle model.

#### `civic.process.framed`

Emitted when the framing phase is complete and the process configuration is finalized.

```json
{
  "event_type": "civic.process.framed",
  "process_id": "string",
  "timestamp": "ISO8601",
  "actor": "DID of framing actor",
  "jurisdiction": "string",
  "action_url": "string",
  "data": {
    "eligibility_requirements": ["array of credential types"],
    "participation_mode": "open | gated | invitation-only",
    "interaction_type": "vote | comment | deliberate | hybrid",
    "starts_at": "ISO8601",
    "ends_at": "ISO8601",
    "aggregation_method": "string (optional)",
    "action_count": "number of defined actions"
  }
}
```

#### `civic.process.aggregation_completed`

Emitted when the aggregation phase has finished processing participant inputs into structured results.

```json
{
  "event_type": "civic.process.aggregation_completed",
  "process_id": "string",
  "timestamp": "ISO8601",
  "actor": "DID of aggregating actor or hub system ID",
  "jurisdiction": "string",
  "action_url": "string",
  "data": {
    "aggregation_method": "string",
    "participant_count": "number",
    "result_summary": "string (human-readable summary)",
    "result_type": "tally | summary | synthesis | allocation",
    "anomalies": "array of strings (optional)"
  }
}
```

#### `civic.process.outcome_recorded`

Emitted when the outcome or decision has been formally recorded for the process.

```json
{
  "event_type": "civic.process.outcome_recorded",
  "process_id": "string",
  "timestamp": "ISO8601",
  "actor": "DID of outcome author or decision authority",
  "jurisdiction": "string",
  "action_url": "string",
  "data": {
    "outcome_type": "advisory | binding | informational | input",
    "outcome_description": "string",
    "decision_authority": "DID (optional, for binding outcomes)",
    "linked_process_id": "string (optional, if outcome feeds another process)"
  }
}
```

#### `civic.process.feedback_received`

Emitted when a participant submits feedback on a finalized process. This event type is optional in v0.1 and is expected to become required in v0.2+.

```json
{
  "event_type": "civic.process.feedback_received",
  "process_id": "string",
  "timestamp": "ISO8601",
  "actor": "DID of feedback submitter",
  "jurisdiction": "string",
  "action_url": "string",
  "data": {
    "feedback_type": "process_quality | outcome_satisfaction | accessibility",
    "content": "string",
    "rating": "number (optional, 1-5 scale)"
  }
}
```

### 7.5 Action → Event Mapping

Each defined action MUST specify which events it emits.

Example:

- `submit_vote` → emits `civic.process.vote_submitted`

### 7.6 Lifecycle Phase → Event Mapping

Each lifecycle phase MUST emit at least one event. The following table defines the minimum event emissions per phase:

| Lifecycle Phase | Required Event(s) |
|---|---|
| 0. Initiation | `civic.process.created` |
| 1. Framing | `civic.process.framed` |
| 2. Activation | `civic.process.started` |
| 3. Participation | At least one participation event per action taken (e.g., `civic.process.vote_submitted`) |
| 4. Aggregation | `civic.process.aggregation_completed` |
| 5. Outcome / Decision | `civic.process.outcome_recorded` |
| 6. Publication | `civic.process.result_published` |
| 7. Feedback / Continuity | `civic.process.feedback_received` (per submission) and/or `civic.process.updated` (for outcome status changes) |

### 7.7 Distribution

Events should be:
- publishable via ActivityPub or equivalent
- consumable by feeds and dashboards

---

## 8. Process Observability

Civic processes derive their legitimacy from transparency. A process that cannot be observed cannot be trusted. This section defines the observability requirements that ensure every Civic Process is externally verifiable across its entire lifecycle.

### 8.1 Core Principle

Every lifecycle phase MUST produce at least one event or observable output. There must be no "dark" phases where the process transitions without producing externally visible evidence.

### 8.2 Observable Progression

Lifecycle progression MUST be externally visible through at least one of the following mechanisms:
- Events published to the Civic Activity Feed
- State changes reflected in the process descriptor at `GET /process/:id`
- Status updates available via the hub's event feed at `GET /events`

An external observer (another hub, a citizen interface, a monitoring service) MUST be able to reconstruct the current lifecycle phase of any process by reading its event history and current state.

### 8.3 Observability and Transparency

Civic Processes are accountable to their participants and jurisdictions. Observability supports transparency by ensuring that:
- Every phase transition is recorded and timestamped
- The rules under which a process operates (framing) are published before participation begins
- Aggregation methods and results are auditable
- Outcomes are publicly attributable to the data that produced them

### 8.4 Observability and Legitimacy

Process legitimacy depends on verifiability. Participants, oversight bodies, and the public must be able to confirm that:
- The process followed its declared rules
- Participation was limited to eligible actors
- Aggregation was performed correctly
- The outcome faithfully reflects the aggregated input

Observability is the technical foundation for this verification. Without it, civic trust cannot be established.

### 8.5 Observability and Interoperability

In a federated network of Civic Hubs, observability enables interoperability by providing a shared protocol for monitoring processes across jurisdictions. A hub in one jurisdiction can observe the progress and results of processes in another jurisdiction by consuming their event streams. This enables:
- Cross-hub dashboards and citizen interfaces
- Network-wide civic activity feeds
- Automated monitoring of process health and compliance
- Credential issuance triggered by process outcomes (e.g., participation credentials)

### 8.6 Minimum Observability Requirements (v0.1)

A compliant process MUST:
- Emit events for all state transitions (Sections 7.2 and 7.4)
- Reflect current status in the process descriptor endpoint
- Make the event history for the process available via the hub's event feed
- Not suppress or delay events for any phase that has completed

---

## 9. Aggregation Phase

This section provides detailed requirements for the Aggregation phase (Phase 4), which is a required component of the full lifecycle model.

### 9.1 Definition

Aggregation is the phase in which raw participant inputs are transformed into structured collective outputs. It is distinct from participation: participation collects individual actions, while aggregation produces a collective result.

### 9.2 Aggregation Methods

The aggregation method MUST be defined during the Framing phase and MUST be published as part of the process descriptor. The following aggregation methods are defined:

**Tallying** applies to processes where participants select from defined options (voting, polling, ranked choice). Tallying produces quantitative results: counts, percentages, rankings, or weighted scores.

**Summarization** applies to processes where participants contribute unstructured or semi-structured input (public consultations, comment periods). Summarization produces a structured representation of themes, positions, and frequency.

**Synthesis** applies to deliberative processes where the goal is not to count positions but to identify areas of agreement, disagreement, and emergent understanding. Synthesis produces a narrative or structured report capturing the state of deliberation.

**Allocation** applies to budgeting processes where participants distribute resources across categories. Allocation produces a final distribution based on participant input and the defined allocation algorithm.

### 9.3 Aggregation Outputs

Aggregation outputs MUST be structured and publishable. They MUST conform to a schema defined by the process type handler. At minimum, an aggregation output must include:
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

This phase is not optional overhead. It is essential for the Civic.Social ecosystem's core value proposition: transforming civic participation from isolated events into connected, accountable governance workflows. Hubs that implement the full lifecycle model, including feedback and continuity, provide citizens with a fundamentally different experience — one where their input has visible, traceable impact.

---

## 12. Interface Contract

Civic Processes must expose interfaces for integration.

### 12.1 Process Descriptor
Each process must publish a descriptor:

```json
{
  "id": "process-123",
  "type": "civic.vote",
  "title": "Library Expansion Vote",
  "jurisdiction": "us-va-floyd",
  "actions": ["submit_vote"],
  "requires": {
    "credentials": ["vc:resident:us-va-floyd"]
  },
  "endpoints": {
    "view": "https://example.org/process/123",
    "action": "https://example.org/process/123/vote"
  }
}
```

### 12.2 Required Endpoints
- `GET /process/:id`
- `POST /process/:id/action`

### 12.3 Deep Linking
- Web URL required
- Optional mobile deep links

---

## 13. Hub Responsibilities

Civic Hubs integrating processes must:

- Authenticate users via identity layer
- Verify credentials before participation
- Host or embed process interfaces
- Publish process events to Civic Activity Feed layer

---

## 14. Minimal Compliance Requirements

To be compliant with v0.1, a Civic Process must:

- Define metadata and lifecycle
- Integrate with identity for gated participation
- Emit standardized events
- Provide a process descriptor
- Support basic action endpoint

### 14.1 Full Lifecycle Compliance (Recommended)

Processes that implement the full lifecycle model defined in Section 5 must additionally:

- Emit lifecycle phase events as defined in Section 7.4
- Support aggregation with structured, publishable outputs (Section 9)
- Record outcomes with type classification (Section 10)
- Meet the minimum observability requirements defined in Section 8.6

Full lifecycle compliance is recommended for all production deployments and will become required in v0.2.

---

## 15. Out of Scope (v0.1)

The following are intentionally excluded:

- UI/UX design
- Moderation systems
- Ranking algorithms
- Messaging systems
- Advanced governance models

---

## 16. Future Extensions (v0.2+)

- Delegative democracy (liquid voting)
- Reputation systems
- Advanced credential logic
- Multi-stage processes
- Cross-process composition

---

## 17. Process as a First-Class Interoperable Object

A Civic Process is not merely a data structure or an API endpoint. It is the fundamental unit of civic interaction in the Civic.Social ecosystem, and it is designed to operate as a first-class interoperable object across a federated network of hubs.

### 17.1 Identity-Aware

Every Civic Process is anchored in the decentralized identity layer. Creators, participants, and decision-makers are identified by DIDs. Eligibility is enforced through Verifiable Credentials. This means a process's provenance, participation record, and outcomes are cryptographically verifiable — not dependent on any single hub's authority.

### 17.2 Lifecycle-Driven

Every process follows a defined lifecycle with observable phases and state transitions. The lifecycle model ensures that processes are not opaque black boxes but transparent, auditable sequences of civic work. Any observer can determine what phase a process is in, what has happened, and what is expected to happen next.

### 17.3 Event-Emitting

Every significant action and transition in a Civic Process produces a standardized event. These events are the distribution mechanism: they flow into the Civic Activity Feed, enabling citizen interfaces, dashboards, monitoring systems, and other hubs to track and respond to civic activity in real time. Events are the connective tissue of the federated network.

### 17.4 Portable Across Hubs

A Civic Process descriptor contains everything needed to understand and interact with the process: its type, rules, actions, endpoints, and credential requirements. This descriptor is self-contained and protocol-native. A hub that receives a process descriptor from another hub can display the process, verify credentials, and route participant actions without any out-of-band coordination. This portability is what makes federation possible — processes are not locked to the hub that created them.

### 17.5 Composable

Through the Feedback / Continuity phase and process linking, Civic Processes can be composed into chains, sequences, and networks of civic activity. A proposal process can spawn a vote. A vote can trigger an implementation review. A budget allocation can link to quarterly audits. This composability transforms isolated civic acts into sustained governance workflows.

---

## Summary

This specification defines Civic Processes as the core interactive unit of the Civic.Social ecosystem.

Processes are:
- Identity-aware
- Event-driven
- Interoperable

They follow a full lifecycle — from initiation through framing, participation, aggregation, outcome recording, publication, and feedback — that ensures every civic interaction is transparent, observable, and connected to real-world impact.

They serve as the foundation for building a modular, federated civic participation infrastructure that supports not just individual civic events, but a continuous, accountable network of interoperable civic process lifecycles.
