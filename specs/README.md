# The Civic.Social Specifications

These four specifications are the **canonical foundation** of Civic.Social.
Everything else in this repository — architecture notes, pilot specs,
briefs — builds on them, and where any other document conflicts with
them, these specifications govern.

| Specification | Defines |
| --- | --- |
| **[Civic Space](civic-space-spec.md)** | The group primitive: community-governed civic spaces — hubs, representative spaces, dashboards — their scopes, governance, and interoperability contract. |
| **[Civic Process](civic-process-spec.md)** | The interactive unit: structured civic processes (voting, budgeting, deliberation) with lifecycles, eligibility, and action contracts. |
| **[Civic Activity](civic-activity-spec.md)** | The distribution layer: the standardized activity model every process and space emits, designed for ActivityStreams 2.0 representability. |
| **[Civic Identity](civic-identity-spec.md)** | The identity layer: portable, participant-owned identity and credentials built on open standards (DIDs, Verifiable Credentials, the OpenID4VC family). |

## Status

The specifications are working drafts (v0.1–v0.2) under active development.
They are refined through pilot implementations — see
[`../pilots/`](../pilots/) — and the phasing of the broader program is
described in [`../canon/phasing.md`](../canon/phasing.md).

Supporting architecture and design documents (the conceptual baseline,
plugin architecture, discovery layer, and related notes) live in
[`../ecosystem/`](../ecosystem/).

## Feedback

Questions and proposals are welcome — open an issue, or see
[`../CONTRIBUTING.md`](../CONTRIBUTING.md).
