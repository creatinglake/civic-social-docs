# Contributing to Civic.Social Documentation

Civic.Social is an open ecosystem project, and we welcome contributions
to these documents — from small fixes like typos and clarifications to
substantive suggestions about the ideas themselves.

## Ways to contribute

### If you're comfortable with GitHub

1. Fork this repository to your GitHub account.
2. Create a branch for your changes (`git checkout -b your-branch-name`).
3. Make your edits.
4. Commit with a clear message explaining what and why.
5. Open a pull request back to this repository.
6. Respond to any review feedback; we'll merge when ready.

### If GitHub is new to you

The easiest path for small edits:

1. Navigate to the file you want to change on github.com.
2. Click the pencil icon in the upper right of the file view
   ("Edit this file").
3. Make your changes in the browser.
4. Scroll down, write a brief description of what you changed and why,
   and click "Propose changes."
5. GitHub will walk you through opening a pull request.

For larger suggestions, you're welcome to open an issue describing what
you'd like to see, and we can figure out the best way to collaborate.

### If GitHub isn't your thing at all

Email your suggestions to adam@civic.social. Include the document and
section you're commenting on, and your proposed change or concern. We'll
work it into the repo and credit you in the commit.

## What kind of change is this?

Civic.Social documentation includes two distinct kinds of content, and
contributions to each have different paths:

**Documentation fixes** — typos, clarifications, broken links,
formatting, factual corrections, terminology updates. Pull requests are
the right path; we'll review and merge.

**Substantive spec changes** — additions to spec contracts, new
capability classes or activity types, changes to lifecycle phases,
modifications to architectural commitments. These shape the underlying
standards the ecosystem builds on, and we want them discussed before
they land. The right path is to raise the proposal in the relevant
community-group venue (see below) or open a GitHub issue describing the
proposal and the reasoning, and let the conversation happen before any
PR. Once the change has rough consensus from the community group, a PR
follows naturally.

When in doubt, open an issue first — it's never wrong to ask "where
should this go?"

## Community-group governance

The Civic.Social specifications in `ecosystem/` and `pilots/` are
explicitly **working drafts published for community discussion**. They
are intended to evolve through open community-group meetings, not
unilaterally through Civic.Social. The long-term venue for governance
of each spec is itself an open question — see the Status and Governance
notes inside individual pilot specs.

Current venues we intend to coordinate with on overlapping work include:

- **DDS Working Group** (`github.com/dds-wg`) — Decentralized
  Deliberation Standard.
- **Metagov IDT cohort** (`metagov.github.io/interop`) — Interoperable
  Deliberative Tools.
- **DelibTech Network** (`demnext.org/projects/delibtech-network`) —
  Deliberation & Technology Network from DemocracyNext and the AI &
  Democracy Foundation.

Civic.Social's posture is to support whichever venue has the most
relevant participants and the lowest coordination overhead for any
given spec, including the possibility of folding a spec's stewardship
into one of these venues if that's where the contributors actually are.

## What we're looking for

- **Corrections** to factual, technical, or terminological errors
- **Clarifications** where documents are confusing or ambiguous
- **Additions** that strengthen the ideas, with clear reasoning
- **Challenges** to premises, with substantive argument — we'd rather
  know where we're wrong

## What we ask

- Reference the terminology canon for defined terms; propose changes
  to the canon if terminology itself needs work.
- Explain the *why* of your change in your PR description, not just the
  *what*.
- Be patient — this is a small team, and substantive changes warrant
  careful review.
- For specs below v1.0, expect that breaking changes between minor
  versions are part of the iteration model — your contribution may be
  folded into a different shape than you proposed if community
  discussion goes that way.

## Attribution

Contributors are credited in commit history and listed in
[AUTHORS.md](AUTHORS.md). All contributions are licensed under the
repository's [CC-BY 4.0 license](LICENSE).

## Questions

Open an issue, or reach out to adam@civic.social.
