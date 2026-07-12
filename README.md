# Civic.Social Documentation

Canonical documentation for Civic.Social — open, federated infrastructure
connecting the pro-democracy ecosystem.

## What's here

This repository contains the authoritative specifications and reference
materials for Civic.Social. It's organized to reflect the structure of
the work:

- **`canon/`** — foundational reference material (terminology, shared
  definitions). Other documents in this repo refer to these definitions
  rather than restating them.

- **`case/`** — the case for Civic.Social: narrative and positioning
  material describing what Civic.Social is and why it matters. New readers
  should start here; **[Before & After](case/before-and-after.md)** is the
  recommended entry point.

- **`ecosystem/`** — the four canonical specifications that anchor
  Civic.Social —
  **[Civic Space](ecosystem/civic-space-spec.md)** (`civic-space-spec.md`),
  **[Civic Process](ecosystem/civic-process-spec.md)** (`civic-process-spec.md`),
  **[Civic Activity](ecosystem/civic-activity-spec.md)** (`civic-activity-spec.md`),
  and **[Civic Identity](ecosystem/civic-identity-spec.md)** (`civic-identity-spec.md`) —
  plus their companion documents: the plugin architecture, the discovery
  layer, the authorization model note, the architecture baseline, and the
  AI documents. These are the ideas everything else builds on.

- **`pilots/`** — specifications for the seven pilot programs. Three are
  fully specified: the **[Civic Identity Pilot](pilots/civic-identity/civic-identity-pilot-spec.md)**,
  the **[Civic Hubs Pilot](pilots/civic-hubs/civic-hubs-pilot-spec.md)**,
  and the **[Civic Process Plugin Pilot](pilots/civic-process/civic-process-pilot-spec.md)**.
  Three more are newly drafted — Civic Activity Feed & Discovery, Citizen
  Dashboard, and Civic Credentialing — and their specs will be added as
  they reach publishable state. The seventh, the
  **[Civic.Social Governance Pilot](pilots/civic-governance/civic-governance-pilot-spec.md)**,
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
