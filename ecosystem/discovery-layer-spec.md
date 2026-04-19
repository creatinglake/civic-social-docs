---
status: review
last-reviewed: 2026-04-19
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

- **Hubs → spaces**\
  Community or jurisdiction-based environments where civic activity occurs

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

- Civic Hubs
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

### 6.1 Hubs (Spaces)

- Jurisdictional or organizational civic spaces

### 6.2 Civic Processes (Functionality)

- Process processes (e.g., voting, assemblies)
- Information processes (e.g., data feeds, news)

### 6.3 Events (Activity)

- Time-bound civic opportunities or updates

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
- type (hub, organization, process provider)
- jurisdictions
- feeds (URLs)
- processes (optional)
- contact

---

#### B. Civic Activity Feed Endpoint

Provides structured civic events.

Each event should include:

- id
- type (vote, assembly, meeting, etc.)
- title
- summary
- jurisdiction
- start/end time
- action URL
- category/tags

---

#### C. Civic Process Descriptor (for Civic Processes)

Fields:

- id
- name
- category (process | information)
- description
- provider
- jurisdictions (optional)
- access URL

---

### 7.2 Indexer Responsibilities

An indexer should:

- Ingest manifests and feeds
- Normalize data
- Store indexed entities
- Expose query endpoints

Example queries:

- `GET /events?jurisdiction=us-va-floyd`
- `GET /processes?category=process`
- `GET /hubs?location=virginia`

---

### 7.3 Ranking (MVP)

- Primary: chronological
- Secondary: geographic proximity

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
