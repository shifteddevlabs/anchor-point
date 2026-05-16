# Changelog

## v3.1.0 — 2026-05-15

Anchor Point v3.x is a foundational shape change. v1.x described a 6-root-file system (`README`, `CLAUDE`, `CONTEXT`, `SESSION-HANDOFF`, `ROADMAP`, `REFERENCES`); v3.x collapses that to 5 (`README`, `AGENTS`, `STATUS`, `ROADMAP`, `LOOKUP`) and treats session handoff as a pure-function operation. v3.1 is a refinement of v3.0 with one additive bucket and a packaging merger.

### Changed (v1.x → v3.0, also in v3.1)

- **5 root files instead of 6.** `CONTEXT.md` is absorbed into `AGENTS.md` as a "Current Phase" section. `SESSION-HANDOFF.md` renamed to `STATUS.md`. `REFERENCES.md` renamed to `LOOKUP.md`. `CLAUDE.md` becomes a 1-line stub pointing to `AGENTS.md`.
- **`AGENTS.md` is the canonical AI bootstrap.** Eight fixed sections in a fixed order: Identity, Current Phase, Routing-by-task, Where new files go, Project Rules, Tech Stack, Gotchas, Inherits from.
- **Routing-by-task table holds direct file paths.** Eliminates the v1.x "look up which SOT file to consult" intermediate hop.
- **`doc-update` is a pure function.** Given the same inputs (git log, file diffs, pre-routed Asks), it produces a byte-identical `STATUS.md`. No accumulated state, no judgment calls at handoff time.
- **Pre-routed Asks.** Mid-session asks are tagged with their destination home (`[AGENTS.md: never log raw HealthKit data]`) at write-time, not deferred to session end.
- **Decisions get their own folder.** `docs/decisions/<YYYY-MM-DD-topic>.md` per decision; ROADMAP keeps a 1-line pointer.
- **Layer 0 inheritance.** Cross-project Hard Rules live ONCE at a Layer 0 home (e.g., a monorepo's shared rules file); project AGENTS.md files use `## Inherits from` instead of duplicating the baseline. Workspace-specific binding lives in a gitignored `internal-overlay.md`.

### Added in v3.0

- **Three new anti-patterns:** A11 (Layer 0 duplication), A12 (Asks pile up without routing tags), A13 (handoff output drifts on rerun).
- **Single-owner facts via imperative/declarative test.** Sentence form determines home: imperative → Project Rules; declarative → Tech Stack or reference; conditional → Debugging Playbook.

### Added in v3.1

- **`docs/roadmap-history/` bucket.** Parallel to `status-history/` for rotated ROADMAP snapshots when ROADMAP exceeds 300 lines.
- **Expanded MG2 to a two-part fix:** ROADMAP > 300 lines now requires both (a) extracting Decision Log entries to `docs/decisions/` AND (b) rotating completed-priority sections to `docs/roadmap-history/`.
- **9 canonical `docs/` subfolders created at bootstrap with README stubs.** Empty folders carry signal (the README defines purpose), not noise. Resolves a v3.0 spec contradiction between "all at bootstrap" and "lazy on-demand."
- **Packaging merger.** The previously-separate `doc-system` workflow capability was merged into `anchor-point`. Anchor Point now owns all 7 modes (Init, Review, Update, Audit, Inventory & Extract, Ratchet, Workflow Autoresearch) instead of pointing at a separate spec.

### Migrated

The v1.x → v3.0 migration is automatable. `doc-audit` Mode 4 detects all v1.x patterns and proposes the atomic migration. `CONTEXT.md` → `AGENTS.md` §2, `SESSION-HANDOFF.md` → `STATUS.md` (with A9 priority-list cleanup), `REFERENCES.md` → `LOOKUP.md` (with A10 file-inventory strip), Decision Log entries → `docs/decisions/`.

### Token cost

Cold start dropped from ~8-10K tokens (v1.2) to ~4.5K tokens (v3.x): routing tables pre-load all paths so subsequent file lookups are direct.

## v1.1.0 — 2026-05-10

This release hardens Anchor Point from a Claude-centered documentation specialist into a repoed, model-agnostic documentation capability.

### Hardened

- Made `AGENTS.md` the canonical AI bootstrap file.
- Reframed tool-specific files (`CLAUDE.md`, slash commands, Codex skills, Cursor rules, schedulers) as adapters that point back to the capability.
- Expanded the lifecycle from 4 modes to 6 workflows: Init, Review, Update, Audit, Ratchet, Workflow Autoresearch.
- Added duplicate-only doc ratchet rules so messy projects are tested on fixtures before real project edits.
- Added workflow-autoresearch rules for improving the doc system itself from multiple project runs.
- Added a workflow score rubric to judge fixture safety, scoring integrity, context preservation, generalization, rule promotion, real-project readiness, and automation readiness.
- Expanded the drift catalog from A1-A8 to A1-A12.
- Added explicit checks for private-path dependency, product/content Markdown misclassification, archive/history link breakage, and ROADMAP-as-archive drift.
- Changed `docs/history/` semantics from rotated handoff backups to completed work, old handoffs, and retired roadmap detail.
- Added model-agnostic vocabulary: capability, workflow, routine, component, adapter, overlay.

### Versioning Notes

- `v1.0.x`: original Anchor Point package, centered on the 4-mode doc lifecycle.
- `v1.1.0`: hardened capability architecture, model-agnostic bootstrap, ratchet/autoresearch workflows.
- Future minor versions should add new reusable workflows or scoring criteria.
- Future patch versions should fix wording, templates, examples, or link issues without changing workflow shape.
