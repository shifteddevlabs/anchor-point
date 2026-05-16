# Changelog

## v3.2.0 — 2026-05-16

v3.2 codifies how `docs/` subfolders, DOCS-INDEX.md, and CONTEXT.md naming interact. Origin: 2026-05-16 evaluation against Jake Van Cleef's CONTEXT.md proposal and Codex's progressive-disclosure framing surfaced three unaddressed gaps in v3.1's subfolder shape. No root file changes; v3.2 is purely additive rules + 5 new anti-patterns + a worked example.

### Added

- **Subfolder discipline section (6 rules):**
  - **Rule 1:** Subfolder READMEs contain Purpose + What goes here + Out of scope only. No file lists (DOCS-INDEX owns inventory). Skip the README if <3 files.
  - **Rule 2:** DOCS-INDEX.md is location-only. No Quick Start / task-routing table (AGENTS.md §3 owns that).
  - **Rule 3:** CONTEXT.md is reserved for agent-only working folders that a parent file explicitly routes to. Not a default file type. README.md is the convention for human-visible folders.
  - **Rule 4:** AGENTS.md sizing uses a hot-vs-warm test. HOT sections (Identity, Current Phase, Routing, Where files go, Project Rules, **Gotchas**, Inherits-from) stay regardless of length — gotchas are proactive warnings the agent must absorb at session start. WARM extracts first (Tech Stack → docs/reference/, code-pattern sections → docs/dev/, file-structure → docs/dev/file-structure.md).
  - **Rule 5:** Project-level ROUTER.md split is justified only when routing table >25 rows AND ≥3 distinct task families AND warm-content extraction is exhausted. Workspace roots are exempt.
  - **Rule 6:** Workspace cheat-sheet ↔ ROUTER.md must not duplicate task→load routing.
- **Five new anti-patterns:** A14 (subfolder README lists files), A15 (CONTEXT.md in human folder), A16 (patterns in AGENTS.md), A17 (DOCS-INDEX has routing table), A18 (premature ROUTER.md split).
- **Worked-example appendix** in `reference/doc-architecture.md`: extracting an over-cap AGENTS.md using overdrive-lab (552 lines → projected ~160-180) as the case. Includes "what NOT to do" — explicitly: do not move Gotchas to reference, do not split routing prematurely.

### Why gotchas stay hot (the design moment that shaped v3.2)

An early draft of Rule 4 proposed extracting Gotchas to `docs/reference/gotchas.md`. That was wrong: the agent doesn't open `gotchas.md` proactively because they don't know they need to. Gotchas only function if they're in context at session start so the agent recognizes the hazard before stepping on it. v3.2's hot-vs-warm test is the explicit framework that produces the right answer here.

### Not changed

- 5 root files (README, AGENTS, STATUS, ROADMAP, LOOKUP) — same shape
- 9 docs/ subfolders at bootstrap — same set
- 8 AGENTS.md sections — same schema
- AGENTS.md 200-line cap — same number; Rule 4 governs how to stay under it
- Cold-start token budget — still ~4.5K when projects conform

### Migration impact

- **smg-ep** and **split-sheet** (the two projects already on v3.1): need subfolder README trim (Rule 1) + DOCS-INDEX Quick Start strip (Rule 2). Small surface cleanup; no structural changes.
- **overdrive-lab** (552-line AGENTS.md): the worked-example appendix in `reference/doc-architecture.md` is its extraction recipe. Track as separate task.
- **quicksinq** (12-line stub): complete the v3.0/3.1 migration first; v3.2 rules apply naturally.
- **ai-health-export, essential, lyric-sheet** (not yet on v3.x): bootstrap directly to v3.2; the Init template will already conform.

### Workspace-level follow-up flagged

`+vantage-point/AGENTS.md` "Common Tasks Cheat Sheet" overlaps with `+vantage-point/ROUTER.md`. Rule 6 mandates resolution. Tracked as a separate task; not handled in this spec patch.

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
