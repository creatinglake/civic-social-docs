---
status: stable
last-reviewed: 2026-04-19
owners: [adam]
version: 0.1
---

# Civic.Social Terminology Glossary — Canonical Reference (v1.0)

## Core Architecture Terms

**Civic Hub**
A digital civic space operated by a community, jurisdiction, or organization. Hubs host civic processes, enable community participation, and publish civic events. Each hub is independently operated while interoperating through shared identity and federation protocols.

**Civic Process**
A structured, stateful civic interaction that enables participation (e.g., advisory voting, citizen assemblies, participatory budgeting, petitions, public consultations). Formerly referred to as "Civic Capability" in earlier documents. This is the canonical term.

**Civic Process Plugin**
A modular implementation of a Civic Process that can be installed and run across multiple civic interfaces (hubs, dashboards, external websites). Formerly "Civic Capability Plugin."

**Civic Event**
A standardized data object representing an action or lifecycle transition within the ecosystem. Events are the distribution layer.

**Civic Activity Feed**
The aggregation and distribution layer that surfaces civic events to citizens. Formerly referred to as "Civic Feed," "Civic News Feed," or "Event Feed" in earlier documents.

**Civic Identity**
Decentralized identity infrastructure based on DIDs and Verifiable Credentials enabling trusted participation across the ecosystem.

**Civic Credentialing**
The system for issuing, displaying, and verifying civic credentials (badges). Badges are a publicly-displayed type of credential. Formerly "Civic Badging" in some documents.

**Citizen Dashboard**
A personal civic interface where citizens access tools, view their activity feed, manage hub subscriptions, and discover civic opportunities.

**Civic Process Descriptor**
A machine-readable description of how a process integrates with the ecosystem (actions, required credentials, endpoints).

## Lifecycle Terms

**Process States**
The standard state progression for all Civic Processes: `draft` → `scheduled` → `active` → `closed` → `finalized`

## Identity Terms

**DID (Decentralized Identifier)**
A globally unique, self-sovereign identifier that does not rely on any centralized registry or authority.

**Verifiable Credential (VC)**
A cryptographically verifiable digital credential issued by a trusted issuer that can be independently verified by any other party.

**Citizen Node**
The fundamental identity primitive: a DID + credentials + ability to authenticate and act within the Civic.Social ecosystem.

**Personal Data Store (PDS)**
A decentralized data store that holds user social graph and preferences, separate from the identity layer itself. Enables portability and user control.

## Architecture Layers

The Civic.Social architecture is organized into six canonical layers:

1. **Identity Layer** — Decentralized identity, DIDs, verifiable credentials, and authentication
2. **Civic Hub Layer** — Community-operated hubs that host processes and emit events
3. **Civic Process Layer** — Modular, pluggable participation tools (formerly "Plugin Framework" or "Civic Capability Layer")
4. **Civic Activity Feed Layer** — Aggregation, distribution, and discovery of civic events (formerly "Civic Feed Layer")
5. **Discovery Layer** — Hub and process discovery, indexing, and search
6. **Citizen Dashboard** — Personal interfaces for citizens to manage participation and credentials

## Deprecated Terms (Do Not Use)

| Deprecated Term | Replacement |
|---|---|
| "Civic Capability" | "Civic Process" |
| "Civic Capability Plugin" | "Civic Process Plugin" |
| "Civic Capability Plugin Framework" | "Civic Process Plugin Framework" |
| "Elements" (as architecture term) | "Civic Processes" |
| "Civic Badging" | "Civic Credentialing" |
| "Civic Feed" / "Civic News Feed" / "Event Feed" | "Civic Activity Feed" |

---

**Last updated:** April 2, 2026
**Status:** Canonical terminology reference for all Civic.Social documents
**Contact:** contact@civic.social
