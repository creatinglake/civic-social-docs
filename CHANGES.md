# Spec Consolidation v0.2 — Change Map & Review Guide

**Branch:** `spec-consolidation-v0.2` · **Date:** 2026-07-03
**Basis:** the ecosystem architecture audit (`decisions/audit-2026-07-02-ecosystem-architecture.md` in the meta-repo) and the decisions made on its open questions. Every change below cites the audit recommendation (R1–R15) it implements.

## How to review

**Review commit-by-commit, not the aggregate diff.** The two major transformations were split so GitHub renders them as tracked changes:

- `civic-event-spec.md → civic-activity-spec.md`: a **pure-rename commit** (100% similarity) followed by a **content commit** — read the content commit as an in-place diff.
- `civic-hub-spec.md → civic-space-spec.md`: same two-commit pattern.

**Read in full (new documents, no prior version):** `ecosystem/civic-identity-spec.md` · `pilots/civic-activity-feed-discovery/…` · `pilots/citizen-dashboard/…` · `pilots/civic-credentialing/…` · this file. Everything else is an in-place diff.

## The decisions this branch implements

| Decision (yours, 2026-07-02) | Where it landed |
|---|---|
| Event → Activity rebrand; wire keeps `event_type` + `GET /events` in v0.1, rename planned for v0.2 with the AS2 bridge | Activity spec §14 (ratified policy); terminology glossary; every doc's terminology pass keeps wire literals |
| Scope taxonomy `individual / entity / community`; "entity" not "office" | Space spec §1.4; terminology glossary |
| Citizen Account Provider + Badge/Credential Issuer are service-provider roles, **not** spaces | Space spec §1.5; terminology glossary; Identity spec §9; Credentialing pilot §6 |
| Portability = conformance ladder A/B/C; **pilots target Level B**; specs are the aspirational end state | Space spec §9.1 + §9.10 (round-trip test); hubs pilot §21/§25 |
| Ballot/participation disclosure is **per-process configurable** | Process spec §2.3 `disclosure_policy`; Activity spec §7 (visibility vs disclosure); Space spec §4.10; Identity Policy Object (Identity spec §8) |
| Identity stewardship keeps the **hedged** language — no binding decentralization triggers | Space spec §3.0 and Identity spec §10 preserve "may evolve" framing unchanged |
| Write **full canonical pilot specs** for the three missing pilots (drafts as source material only) | Three new pilot directories |
| Delivery: branch + PR, commit-per-change, this file | You're reading it |

## Commit-by-commit map

| Commit | Audit rec | What it does |
|---|---|---|
| terminology: ratify four-spec set… | R1, R4 | Civic Space + Civic Activity defined; scope taxonomy; infrastructure roles; five-layer canon; deprecation table extended |
| activity spec: rename (pure) + merge | R3 | Corrupted §2 schema repaired; one required-fields table; unified type registry (absorbs the process spec's phase activities; adjudicates `civic.process.ended` and `civic.outcome_delivered`); extension namespace + `data.process.type` rule; visibility/disclosure split; `source.space_id`; wire policy ratified |
| space spec: rename (pure) + generalize | R2, R5, R6 | ICHS → Civic Space Spec v0.2: primitive definition; open scope taxonomy; space DIDs; Space API Profile folded in from the meta-repo's thin hub spec; portability ladder + per-scope profiles; snapshot-vs-federation reconciled (§9.9); round-trip conformance (§9.10); editorial repairs (duplicate §4.x, orphaned primitives, §5.2 field list); Delegation defined |
| process spec v0.2 | R10 | Lifecycle profiles (ADR-003); `disclosure_policy`; registry/handler/plugin contract (§2.5); `POST /process` + action wire format + error model; descriptor completeness; §7.4 examples now full envelopes; `civic.process.ended` emission point fixed |
| plugin spec | R7, R8 | Capability JSON schema (§4.1); Tier-1 honest security statement; staged enforcement path (§5a) with the normative production boundary; Tier-3 external-service contract (§10a) from the Polis lessons; no manifest-less plugins |
| discovery spec | R5 | Space-general entities keyed by space DID; divergent feed-item/descriptor shapes replaced by references; migration re-binding protocol (§7.4) |
| companion docs | R4, R15 | Baseline version-stamp repair + five-layer rebuild + artifact removal; authz note plugin×actor join; AI docs light pass |
| NEW: Civic Credentialing pilot | (six-pilot program) | Recognition layer; mutual-consent display; issuer role contract |
| NEW: Civic Identity spec + canon/README | R9, R1 | The fourth canonical spec promoted from the pilot; canon counts finally consistent (4 specs · 5 layers · 6 pilots) |
| NEW: Citizen Dashboard pilot | (six-pilot program) | Individual-scoped space; minimal-PDS co-ownership resolves the orphaned-PDS finding; individual-scope compliance profile |
| existing pilots alignment | R6, R15 | Level B ladder; round-trip acceptance bar; spec-name unification; stale links fixed; promotion note in identity pilot |
| NEW: Activity Feed & Discovery pilot | (six-pilot program) | One engine five lenses; shared classifier; envelope validation at ingestion; reference indexer + migration drill |
| consistency sweep + CHANGES.md | R15 | Final terminology stragglers; this review guide |

## Deliberately unchanged

- **All source code** (civic-hub, representative-space, citizen-dashboard, shared) — code converges via the pilots, per the conformance-phasing notes now present in each spec.
- **Hedged stewardship language** — per your decision, no binding decentralization triggers were added anywhere.
- **case/ and positions/ narrative prose** — only explicit spec-name references were touched (one edit in use-cases; the rest had none).
- **The meta-repo's `/specs/*.md`** — different git repo, so not in this PR. Follow-up (two minutes): replace their bodies with pointers to the canonical successors, and `git add specs/civic-activity.md` so the previously untracked file's content is preserved in history before the pointer replaces it.

## Open items this branch surfaces (not blockers)

1. The published JSON-LD context (`https://civicsocial.org/ns/civic`) and the export schema/canonicalization profile are named as prerequisites in the Space and Activity specs — deliverables of the Civic Hubs pilot.
2. Activity signing is scheduled (Activity spec §13) and is the dependency for fully verifiable migrated/federated history and the entity-scope portability profile.
3. Space-type transitions (candidate → post-election) are flagged as an open design area (Space spec §12.5).
4. The DID-stability-across-provider-migration question is preserved as open in the Identity spec (§11.4) — it must be answered before the reference identity service mints at scale.
