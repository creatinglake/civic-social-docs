---
status: review
last-reviewed: 2026-07-03
owners: [adam]
version: 0.1
---

# Civic Discovery Layer & Activity Feed Integration Spec (Draft v0.1)

## 1. Rationale

Civic.Social is not a platform competing for user attention. It is shared infrastructure designed to connect a fragmented civic ecosystem. Discovery is therefore not a standalone feature or product—it is a system-level function that determines whether the ecosystem actually becomes usable and interconnected.

Without effective discovery:

- Civic opportunities remain invisible
- Participation stays fragmented and episodic
- Network effects fail to materialize

With effective discovery:

- Citizens can find relevant civic opportunities without prior awareness
- Organizations gain visibility across the ecosystem
- Participation compounds across time and place

The key insight is:

**Discovery is not separate from the Civic Activity Feed—it is an extension of it.**

The feed aggregates civic activity. Discovery makes that activity navigable, searchable, and explorable.

---

## 2. Architectural Framing

The Civic.Social system can be understood as a set of interoperating layers:

- **Identity → access**\
  Citizens authenticate once and carry credentials across the ecosystem

- **Civic Spaces → environments**\
  Scoped environments where civic activity occurs — community-scoped Civic Hubs, individual-scoped Citizen Dashboards, entity-scoped Representative Spaces, and future space types (Civic Space Specification)

- **Civic Processes → functionality**\
  Modular components (process or information) that enable participation

- **Activity Feed → distribution**\
  Aggregation of civic events and updates across the ecosystem

- **Discovery → navigation**\
  Mechanisms that allow citizens to find, filter, and explore what exists

Discovery is therefore a cross-cutting layer that operates primarily through the Activity Feed and is surfaced through the Dashboard.

---

## 3. Discovery Model: Hybrid + Pluralistic Indexing

Civic.Social adopts a hybrid discovery model:

### 3.1 Federated Publication (Source Layer)

All civic activity originates from independent actors:

- Civic Spaces (hubs, representative spaces, dashboards)
- Civic Process providers
- Civic organizations
- Government systems

These actors publish:

- Civic events (via feeds)
- Metadata (via manifests)
- Civic Process descriptors

No central authority controls publication.

---

### 3.2 Indexing Layer (Pluralistic)

Multiple indexers can exist:

- Civic.Social reference index (default)
- Regional or jurisdictional indexes
- Issue-specific or organizational indexes
- Third-party applications

Each indexer:

- Ingests feeds and manifests
- Builds a searchable index
- Exposes query APIs

This creates a **shared discovery commons** rather than a single gatekeeper.

---

### 3.3 Interface Layer (Dashboard)

Citizen interfaces (e.g., the Dashboard) use:

- One or more indexes
- Personal subscriptions
- Civic identity context

To present:

- Feeds
- Search results
- Recommendations
- Geographic discovery

---

### 3.4 Social Overlay

Additional discovery signals may include:

- Follow relationships
- Participation signals
- Organizational affiliation

This layer enhances relevance but does not replace structured discovery.

---

## 4. Why Not Purely Centralized or Purely Federated?

### Centralized-only problems:

- Creates control points and governance risk
- Conflicts with public infrastructure principles

### Fully distributed-only problems:

- Weak search and discoverability
- Severe cold-start issues

### Hybrid model advantage:

- Preserves decentralization
- Enables strong user experience
- Allows competition and plurality in indexing

---

## 5. Discovery Within the Civic Activity Feed Pilot

Discovery is implemented as an extension of the Civic Activity Feed and Dashboard.

### Phase 1 (Pilot Scope)

Discovery is achieved through:

1. **Chronological Feed (default)**
2. **Geographic filtering and prioritization**
3. **Basic search across indexed entities**
4. **Structured browsing (by type and category)**

No algorithmic ranking beyond time and location is introduced in MVP.

---

### Phase 2 (Future)

- Relevance-based ranking
- Personalization
- Social graph signals

---

### Phase 3 (Future)

- User-selectable algorithms
- Multiple ranking providers
- Transparent ranking models

---

## 6. Core Discovery Entities

The discovery system indexes three primary entity types:

### 6.1 Civic Spaces

- Scoped civic environments (community, individual, entity, and future scopes), keyed by their **space DID** (Civic Space Specification §3.5) with the serving URL as a resolvable attribute

### 6.2 Civic Processes (Functionality)

- Participation processes (e.g., voting, assemblies)
- Information processes (e.g., data feeds, news) — note: "information process" here is a *discovery category*; only participation processes carry the full lifecycle contract of the Civic Process Specification

### 6.3 Civic Activities

- Time-bound civic opportunities or updates, per the Civic Activity Specification

These form the foundation of the discovery graph.

---

## 7. Minimal Discovery Spec (v0.1)

### 7.1 Required Publication Components

Each participating system should expose:

#### A. Discovery Manifest

Location:

```
/.well-known/civic.json
```

Fields:

- name
- space (`{ id: <space DID>, scope, type }`) — for Civic Spaces; non-space publishers (organizations, process providers) use `type` (organization | process provider) instead
- jurisdictions
- feeds (URLs)
- processes (optional)
- contact

Legacy manifests that serve a top-level `type: "hub"` remain readable for the life of v0.1; indexers accept both forms (Civic Space Specification §7.2.0).

---

#### B. Civic Activity Feed Endpoint

Provides structured Civic Activities conforming to the **Civic Activity Specification** (the full envelope: `id`, `version`, `event_type`, `timestamp`, `process_id`, `actor`, `jurisdiction`, `action_url`, `source`, `data`, `meta.visibility`). This spec defines no separate feed-item shape.

For discovery presentation, indexers derive display fields from the envelope: title and summary from `data`, the process type from `data.process.type`, time bounds from the referenced process descriptor, and the action link from `action_url`.

---

#### C. Civic Process Descriptor (for Civic Processes)

Participation processes publish the descriptor defined in the **Civic Process Specification §12.1** (id, type, title, status, lifecycle profile, actions, requirements, endpoints). Indexers map `title` → display name and `endpoints.view` → access URL.

Information processes (discovery category only) publish the minimal record:

- id
- name
- category (`information`)
- description
- provider
- jurisdictions (optional)
- access URL

---

### 7.2 Indexer Responsibilities

An indexer should:

- Ingest manifests and feeds
- Normalize data
- Store indexed entities, **keyed by space DID where the publisher is a Civic Space** (URLs are resolvable attributes, not identity)
- Expose query endpoints
- Honor the migration protocol (7.4): re-bind a space's index entry when the space moves

Example queries:

- `GET /events?jurisdiction=us-va-floyd`
- `GET /processes?category=process`
- `GET /hubs?location=virginia`

---

### 7.3 Ranking (MVP)

- Primary: chronological
- Secondary: geographic proximity

---

### 7.4 Space Migration & Re-Binding Protocol

Civic Spaces can migrate between engines, providers, or domains (Civic Space Specification §9). Discovery must survive that move:

1. **Identity is the DID, not the URL.** Indexers key space entries (and the provenance of their activities) on the space DID. The serving URL is re-resolved from the DID document.
2. **The migration signal.** On migration, the space emits `civic.space.migrated` as the final activity from its old location, carrying the new binding; indexers that consume the old feed re-bind on receipt.
3. **The tombstone.** For as long as the old domain remains under the community's control, the old `/.well-known/civic.json` SHOULD serve a `moved` marker pointing to the new binding: `{ "moved": { "space": "<space DID>", "url": "<new URL>" } }`. Indexers encountering a tombstone re-resolve and re-bind.
4. **No dangling entries.** After re-binding, previously indexed activities remain valid (their `source.space_id` is unchanged); only the resolvable location updates. Indexers SHOULD retain the old URL as a historical alias for inbound links.

---

## 8. Relationship to Civic.Social Pilots

### Civic Activity Feed Pilot (Primary)

- Core aggregation layer
- Event ingestion and distribution
- Foundation of discovery

### Citizen Dashboard Pilot (Primary)

- User interface for discovery
- Search, browsing, and filtering

### Civic Hubs Pilot

- Defines how hubs publish manifests and feeds
- Enables hub-level discoverability

### Civic Process Pilot

- Defines process descriptors
- Enables functionality discovery

### Civic Identity Pilot

- Enables personalization and access
- Not directly part of indexing, but shapes discovery context

---

## 9. Key Design Principles

- **Open publication** — anyone can publish into the network
- **Plural indexing** — multiple indexers can coexist
- **No gatekeeping** — indexing is reproducible and optional
- **Geography-first** — civic discovery is location-centric
- **Progressive complexity** — start simple, evolve ranking later

---

## 10. Summary

Discovery is not a separate system to be built later. It emerges from the combination of:

- Federated event publication
- Shared metadata standards
- Indexing (plural, not centralized)
- Citizen-facing interfaces

The Civic Activity Feed provides distribution. The Discovery Layer makes that distribution usable.

Together, they form the navigational layer of a connected civic ecosystem.
