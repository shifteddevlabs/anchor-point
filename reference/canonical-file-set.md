---
type: reference
status: active
owner: knowledge-ops
last_reviewed: 2026-05-18
applies_to: anchor-point Init + Audit modes
description: Canonical 5-root-file v3 doc set plus the docs/ ecosystem decision tree. Routes any new file to the right home.
---

# Canonical File Set + Where-Things-Go Decision Tree

The 5 canonical root files plus the docs/ ecosystem and a complete decision tree for "where does this new file go?"

---

## The canonical 5 root files (v4)

Every project should have these.

| File | Purpose | Length budget |
|---|---|---|
| `README.md` | Public face — what is this, quick start, tech stack | 50-100 lines |
| `AGENTS.md` | AI bootstrap — Identity, Current Phase, Routing-by-task, Where new files go, Project Rules, Tech Stack, Gotchas, Inherits-from pointer | 200 lines max |
| `SESSION-HANDOFF.md` | Latest delta — TL;DR, Last shipped, Next session, Blockers, Debugging Playbook | 150 lines max |
| `ROADMAP.md` | Priorities — current red/yellow/green, vNext backlog, future. Decision Log split out to `docs/decisions/`. | scales (rotate at 300) |
| `REFERENCES.md` | Topic-driven lookup — SOT registry, playbooks index, API constraints, external docs, external infra | 150 lines max |

`CLAUDE.md` (and other vendor-specific bootstrap files like `.cursorrules`) become 1-line stubs pointing to `AGENTS.md` when present, kept only for tool compatibility.

---

## The `docs/` ecosystem (9 canonical subfolders)

All 9 subfolders are created at bootstrap by Init Mode, each with a `README.md` explaining what goes there. Administrative folders (`_archive/`, `_private/`) appear when content arrives.

| Path | Purpose | Created |
|---|---|---|
| `docs/DOCS-INDEX.md` | Location-driven file inventory | When `docs/` exceeds 10 files |
| `docs/handoff-history/` + `README.md` | Rotated SESSION-HANDOFF.md backups; dated files (`YYYY-MM-DD-NN.md`) | At bootstrap (or first SESSION-HANDOFF rotation) |
| `docs/roadmap-history/` + `README.md` | Rotated ROADMAP.md backups; dated files (`YYYY-MM-DD-NN.md`) | At bootstrap (or first ROADMAP rotation) |
| `docs/decisions/` + `README.md` | One file per decision, dated (`YYYY-MM-DD-<topic>.md`); supersedes ROADMAP Decision Log | At bootstrap |
| `docs/reference/` | Verified-state SOT files | At bootstrap |
| `docs/playbooks/` | Process runbooks | At bootstrap |
| `docs/dev/` | Architecture, schemas, API design | At bootstrap |
| `docs/release/` | Release notes, migration steps | At bootstrap |
| `docs/reviews/` | Code review reports, security audits | At bootstrap |
| `docs/research/` | Experiments, exploration drops | At bootstrap |
| `docs/_archive/` | Historical batches (dated subfolders) | On first archival action |
| `docs/_private/` | Gitignored sensitive content | On first sensitive drop |

Empty bootstrap folders carry signal (the README defines purpose), not noise.

---

## status-history vs roadmap-history vs decisions

These three subfolders all hold "older" content but follow different conventions:

| Folder | Triggered when | File naming | Owner of rotation |
|---|---|---|---|
| `handoff-history/` | SESSION-HANDOFF.md > 200 lines | `YYYY-MM-DD-NN.md` (whole-file rotation, oldest 50%) | Update mode (auto) |
| `roadmap-history/` | ROADMAP.md > 300 lines (completed-priority entries pile up) | `YYYY-MM-DD-NN.md` (rotation of completed sections) | Update mode (auto) |
| `decisions/` | A decision is made (any time) | `YYYY-MM-DD-<topic>.md` (one file per decision) | Per-decision (manual or Update-mode auto) |

The first two are LIFECYCLE folders that grow on rotation. The third is a CATALOG that grows per decision, regardless of file size.

---

## The decision tree — where does a new file go?

When a new file needs to be created, walk this table top-to-bottom. First match wins.

| If the file is... | Goes to | Naming | Also do |
|---|---|---|---|
| One of the 5 canonical root docs | Project root | `ALL-CAPS-KEBAB.md` | None — already canonical |
| Architecture / API design / schema doc | `docs/dev/` (or `docs/dev/<tech>/` if 3+ exist for one tech) | `lowercase-kebab.md` | None |
| Release notes / changelog | `docs/release/` | `vN-notes.md` | None |
| Migration plan / per-migration decisions | `docs/decisions/` | `YYYY-MM-DD-<migration>.md` | None |
| Migration runbook / rollback procedure | `docs/playbooks/` | `<topic>-playbook.md` | Add row to LOOKUP.md Playbooks index |
| Migration notes tied to a release | `docs/release/` | `vN-migration-notes.md` | None |
| Code review / security audit | `docs/reviews/` | `YYYY-MM-DD-<scope>.md` | None |
| Decision rationale (one per decision) | `docs/decisions/` | `YYYY-MM-DD-<topic>.md` | Reduce ROADMAP Decision Log to a 1-line pointer |
| Verified-state config (OAuth IDs, env vars, exact values) | `docs/reference/` (or `docs/reference/<tech>/` if 3+) | `lowercase-kebab.md` | Add row to LOOKUP.md SOT registry |
| Process runbook ("when X happens, do Y") | `docs/playbooks/` (or `docs/playbooks/<tech>/` if 3+) | `<topic>-playbook.md` | Add row to LOOKUP.md Playbooks index |
| Research / experiment / exploration | `docs/research/` | `lowercase-kebab.md` | None |
| Rotated SESSION-HANDOFF.md content | `docs/handoff-history/` | `YYYY-MM-DD-NN.md` | None (auto by Update mode) |
| Rotated ROADMAP.md content | `docs/roadmap-history/` | `YYYY-MM-DD-NN.md` | None (auto by Update mode) |
| Sensitive (secrets, internal numbers, business docs) | `docs/_private/` | `lowercase-kebab.md` | None |
| Don't know where it goes | Drop in `docs/research/` and let Audit relocate later, or ask before creating | `lowercase-kebab.md` | Flag for Audit |

---

## Grouping rule (3+ docs about same topic)

When a topic accumulates 3+ files, group them into a topic-named subfolder under the appropriate type folder:

- 3+ Supabase architecture docs → `docs/dev/supabase/`
- 3+ Stripe runbooks → `docs/playbooks/stripe/`
- 3+ verified-state files for one integration → `docs/reference/<integration>/`

When fewer than 3 exist, leave them flat (e.g., `docs/dev/auth-flow.md` is fine if it's the only auth doc).

Audit enforces this — when 3+ files share a topic prefix or repeated keyword, it proposes creating a subfolder and moving them together.

---

## What gets auto-updated when files move

Audit never just moves files in isolation. After every approved structural change, it auto-syncs:

1. **AGENTS.md "Where new files go" table** — reflects the new layout
2. **`docs/DOCS-INDEX.md`** — regenerated with the new file map
3. **All cross-references** — scans every doc for pointers to old paths and updates them (link rot prevention)
4. **LOOKUP.md** — refreshes SOT registry / Playbooks index rows if structure changed

This is what makes Audit safe — moves are atomic AND fully reconciled across the doc tree.

---

## Why this organization works

**Predictability:** an agent or human can find any file by walking the tree. Architecture docs? `docs/dev/`. Verified config? `docs/reference/`. Decisions? `docs/decisions/`. Rotated history? `docs/handoff-history/` or `docs/roadmap-history/`. No guessing.

**Compounds over time:** as the project grows, the structure scales. Three migrations? `docs/decisions/`. Three Stripe docs? `docs/playbooks/stripe/`. Audit handles consolidation when thresholds are crossed.

**Easy to audit:** the Audit workflow knows exactly what's expected at each location. Anything unexpected is an orphan that needs relocating.

**Survives team handoffs:** a contractor or new developer can clone the repo and navigate it without onboarding. The conventions are written into the file tree itself.

**Tool-agnostic:** the architecture works whether you use Claude, Codex, Cursor, Cowork, or no AI tool at all. The structure is what matters.

**Adapter-friendly:** tool-specific commands, installed skills, and scheduled jobs should point back to this capability instead of becoming separate sources of truth.

---

## Migration from v3 (5-file with STATUS/LOOKUP) projects

Projects that adopted the v3 5-file shape used `STATUS.md` and `LOOKUP.md` at the root. The v4 rename adopts canonical names that match natural LLM routing language:

| v3 file/folder | v4 destination | Action |
|---|---|---|
| `STATUS.md` | `SESSION-HANDOFF.md` | `git mv STATUS.md SESSION-HANDOFF.md` |
| `LOOKUP.md` | `REFERENCES.md` | `git mv LOOKUP.md REFERENCES.md` |
| `docs/status-history/` | `docs/handoff-history/` | `git mv docs/status-history docs/handoff-history` |

Audit Mode 4 detects via MG5 and proposes migration atomically.

---

## Migration from v1.x (6-file) projects

Projects that still use the v1.x shape (6 root files: README, CLAUDE, CONTEXT, REFERENCES, SESSION-HANDOFF, ROADMAP) migrate as follows:

| v1.x file | v3.0 destination | Action |
|---|---|---|
| README.md | README.md | Keep |
| CLAUDE.md | AGENTS.md (new) + CLAUDE.md (1-line stub) | Migrate Layer 0 baseline content; replace with `## Inherits from` pointer if duplicated |
| CONTEXT.md | AGENTS.md "Current Phase" section | Merge into AGENTS, delete CONTEXT.md |
| REFERENCES.md | LOOKUP.md (rename) | Strip any DOCS-INDEX-style file inventory rows (A10) |
| SESSION-HANDOFF.md | STATUS.md (rename) | Strip parallel priority lists (A9); add pre-routed Asks convention (A12) |
| ROADMAP.md | ROADMAP.md | Extract Decision Log to `docs/decisions/`; rotate completed sections to `docs/roadmap-history/` if > 300 lines |

Audit Mode 4 detects all v1.x patterns and proposes migrations atomically.
