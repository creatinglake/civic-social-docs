# Civic.Social Documentation

Canonical documentation for Civic.Social — open, federated infrastructure
connecting the pro-democracy ecosystem.

## What's here

This repository contains the authoritative specifications and reference
materials for Civic.Social. It's organized to reflect the structure of
the work:

- **`specs/`** — **the four canonical Civic.Social specifications**:
  **[Civic Space](specs/civic-space-spec.md)** ·
  **[Civic Process](specs/civic-process-spec.md)** ·
  **[Civic Activity](specs/civic-activity-spec.md)** ·
  **[Civic Identity](specs/civic-identity-spec.md)**.
  These are the foundation everything else builds on; where any other
  document conflicts with them, the specifications govern. Start with the
  **[specs index](specs/README.md)**.

- **`canon/`** — foundational reference material (terminology, shared
  definitions). Other documents in this repo refer to these definitions
  rather than restating them. Includes
  **[Civic.Social Horizons](canon/phasing.md)** — the single place that
  says which of these documents describe what exists today, what the
  pilots build, and where the project is headed. Worth reading before
  anything else if you have found a specification that seems to promise
  less than the case material does.

- **`case/`** — the case for Civic.Social: narrative and positioning
  material describing what Civic.Social is and why it matters. New readers
  should start here; **[Before & After](case/before-and-after.md)** is the
  recommended entry point.

- **`ecosystem/`** — supporting architecture and design documents: the
  conceptual baseline, the plugin architecture, the discovery layer, and
  related design notes. These elaborate on the canonical specs without
  superseding them.

- **`pilots/`** — specifications for the seven pilot programs. Three are
  fully specified: the **[Civic Identity Pilot](pilots/civic-identity/civic-identity-pilot-spec.md)**,
  the **[Civic Hubs Pilot](pilots/civic-hubs/civic-hubs-pilot-spec.md)**,
  and the **[Civic Process Plugin Pilot](pilots/civic-process/civic-process-pilot-spec.md)**.
  Three more are newly drafted: the
  **[Civic Activity Feed & Discovery Pilot](pilots/civic-activity-feed-discovery/civic-activity-feed-discovery-pilot-spec.md)**,
  the **[Citizen Dashboard Pilot](pilots/citizen-dashboard/citizen-dashboard-pilot-spec.md)**,
  and the **[Civic Credentialing Pilot](pilots/civic-credentialing/civic-credentialing-pilot-spec.md)**. The seventh, the
  **Civic.Social Governance Pilot** (in final review; publishing shortly),
  addresses how the specifications themselves are governed — the question
  the other six pilots each defer.

- **`positions/`** — public statements of where Civic.Social stands on
  questions of principle, setting out our commitments and the values that
  constrain the work. The first is
  **[Our Approach to AI](positions/our-approach-to-ai.md)** — how we think
  about building with AI, the tensions we carry, and what we commit to over
  time.

Other top-level directories may be added as the documentation grows.

## The case for Civic.Social

For funders, partners, and anyone new to the project:

- **[Before & After](case/before-and-after.md)** — what fragmentation
  feels like today, and what a connected civic ecosystem makes possible.
  The most accessible introduction.
- **[Positioning Memo](case/positioning-memo.md)** — the full
  positioning case: the problem, the insight (infrastructure before
  platform), what Civic.Social is and is not, and the governance approach.
- **[The Investment Case — Why This, Why Now, Why Us](case/why-this-why-now-why-us.md)** —
  a structured response to the four questions funders and partners ask
  about every civic infrastructure investment.
- **[Civic.Social Deck (Feb 2026)](case/civic-social-deck-2026-02-26.pdf)** —
  15-slide visual overview. Useful as a quick walkthrough for first
  conversations. PDF renders inline on GitHub.

These documents describe the destination. **[Civic.Social
Horizons](canon/phasing.md)** is the companion that says how much of it
exists today, what the pilots are scoped to build, and what remains
direction rather than schedule. Read it alongside the case material — and
before the specifications, which are written to the near horizon and will
otherwise read as though they promise less.

## Document status

Each document has YAML frontmatter indicating its current state:

- **`draft`** — actively being developed; expect substantial changes
- **`review`** — content is stable but awaiting review or feedback
- **`stable`** — current canonical version; changes will be versioned
  deliberately

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to suggest changes, whether
you're comfortable with GitHub or not.

## License

All documentation in this repository is licensed under
[CC-BY 4.0](LICENSE) — free to reuse and adapt with attribution.

## About Civic.Social

Civic.Social is a 501(c)(3) nonprofit civic technology initiative building
open, federated infrastructure to connect and amplify the pro-democracy
ecosystem. Learn more at civic.social.
