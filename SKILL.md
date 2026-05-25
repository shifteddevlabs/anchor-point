---
name: anchor-point
type: skill
version: 3
repo_intent: published
status: active
owner: knowledge-ops
last_reviewed: 2026-05-24
applies_to_projects: all
github_repo: https://github.com/shifteddevlabs/anchor-point
description: Model-agnostic documentation operating system. Initializes, reviews, updates, audits, and hardens project docs across sessions and AI tools. Enforces the 5-root-file v3 standard (README, AGENTS, STATUS, ROADMAP, LOOKUP) plus drift detection. Use when setting up docs for a new project, recovering context at session start, wrapping up a session, auditing doc health, or improving the workflow itself.
triggers:
  - anchor point
  - doc init
  - initialize docs
  - bootstrap docs
  - review docs
  - catch me up
  - where were we
  - update docs
  - session handoff
  - wrap up
  - doc audit
  - clean up docs
  - doc ratchet
  - project ratchet
  - doc workflow ratchet
---

# Anchor Point

Model-agnostic documentation methodology. Manages project documentation as infrastructure across the full lifecycle: initialize docs, recover context, update handoffs, audit structure, and extract reusable knowledge.

This is the unified operational skill. For human-readable orientation see `identity.md`; for worked examples see `examples.md`; for reference material see `reference/`.

This skill is model-agnostic. It prefers `AGENTS.md` as the AI bootstrap document for new shared infrastructure, while respecting existing `CLAUDE.md` or other platform-specific files already present in projects.

**Working inside a monorepo or capability harness?** If a file named `internal-overlay.md` sits alongside this `SKILL.md`, read it first; it binds the generic references in this skill (Layer 0 home, shared knowledge-ops paths, workflow-score script, migration trackers) to your monorepo's actual paths. Public clones won't have one; that's fine, the methodology stands alone.

## How This Skill Is Organized

This `SKILL.md` is the router. The 7 modes each have a procedural reference file under `reference/mode-N-*.md`; load only the one for the mode you're running. Routing tables, core rules, scoring rubric, naming rules, and output shapes stay inline here because multiple modes touch them.

## Reference Docs

Read only what is needed for the current mode.

| Source | Use For |
|---|---|
| `reference/mode-1-init.md` | Mode 1 full workflow |
| `reference/mode-2-review.md` | Mode 2 full workflow |
| `reference/mode-3-update.md` | Mode 3 full workflow |
| `reference/mode-4-audit.md` | Mode 4 full workflow |
| `reference/mode-5-inventory-extract.md` | Mode 5 full workflow |
| `reference/mode-6-project-ratchet.md` | Mode 6 full workflow + read-budget + timing table |
| `reference/mode-7-doc-workflow-ratchet.md` | Mode 7 full workflow |
| `reference/doc-architecture.md` | Canonical architecture spec (v3) |
| `reference/canonical-file-set.md` | File placement decision tree |
| `reference/anti-patterns.md` | Drift signatures (rationale for the codes used in `drift-checks.md`) |
| `reference/drift-checks.md` | Drift-check detection table (grep patterns + fix routing for Audit and Review modes) |
| `reference/naming-conventions.md` | Naming rules |
| `reference/doc-ratchet.md` | Project Ratchet (Mode 6) deeper background |
| `reference/workflow-autoresearch.md` | Doc Workflow Ratchet (Mode 7) deeper background |

## Components

Use components as internal building blocks. Do not expose them as extra commands unless the user explicitly asks for that piece.

| Component | Use When |
|---|---|
| `components/snapshot.md` | Capture current state and recent changes |
| `components/verify.md` | Confirm volatile or high-risk claims before writing them as facts |
| `components/promote-memory.md` | Move durable learnings to the right memory layer |
| `components/harvest.md` | Extract reusable process from project-local work |
| `components/security-preflight.md` | Flag sensitive docs before quoting, moving, or extracting |

## Component Chains

| User-Facing Workflow | Component Chain |
|---|---|
| `doc-init` | verify → snapshot |
| `doc-review` | snapshot → verify only for high-impact claims |
| `doc-update` | snapshot → security-preflight if docs changed → verify → promote-memory → harvest if reusable learning appeared |
| `doc-audit` | security-preflight → snapshot → verify → promote-memory → harvest if reusable material is found |
| `project-ratchet` | security-preflight → snapshot → doc-audit baseline → duplicate cleanup attempts → verify → promote-memory if reusable learning appeared |
| `inventory and extract` | security-preflight → snapshot → harvest → promote-memory |

## Core Rules

### Layer 0 Hard Rules (cross-project baseline)

These apply to EVERY workflow, every session. Derived from cross-project incidents that have repeated 3+ times across real projects. Inherit them as-is; never edit or paraphrase.

1. **Verify before claiming.** Read the file, run the check, or ask the user. Never answer state-of-the-system questions from memory.
2. **Root-cause, not symptoms.** When something that worked is broken, identify what changed FIRST. Don't layer patches over a broken state.
3. **Quality gate every change.** Whatever the user's commit/push process is, follow it. Never skip review steps to save time.
4. **Delegate to focused investigation** when a task needs to read >3 unrelated files. Protects context.
5. **Trust documented state.** If a verified-state doc says X, use X. If state has drifted, UPDATE the doc, don't ask the user to re-verify what they already documented.
6. **Hypothesis vs Fact.** Label confidence. Correlation is not causation. Don't pattern-match training data into confident claims without verification.
7. **No session editorializing.** Report what changed and what's next. No "ready to ship," no commentary on session length, effort, or pacing. The user decides when work is done.

### Operational Rules (specific to this skill)

1. Verify before claiming. Read the docs or run the scan.
2. Keep project-specific facts in the project that owns them.
3. Extract reusable process, not local facts.
4. Do not read, copy, summarize, or move secret values.
5. Do not move, rename, archive, or restructure files without an explicit plan and approval.
6. When routing behavior changes, update any in-project routing or inventory tables before relying on the new path.
7. Keep routing docs short. Detail belongs in playbooks, reference files, or dev docs.
8. Prefer `AGENTS.md` for new model-agnostic infrastructure. Respect existing `CLAUDE.md` files where they already define project behavior.

### Format preferences (every output)

- **Tables for structured data** (env vars, tech stack, decisions, file maps)
- **Bullet lists for steps and rules**
- **Numbered lists for sequences**
- **Code fences for filenames, paths, commands, and exact content**
- **Bold for emphasis** (sparingly)
- **No em-dashes.** Use commas or parentheses.
- **Length:** as long as needed; as short as possible. No filler.

## Canonical Project Surfaces (v3)

For new model-agnostic project docs, use these 5 root files:

| Surface | Purpose |
|---|---|
| `README.md` | Public overview and quick start |
| `AGENTS.md` | AI bootstrap. Owns Identity, Current Phase (absorbed CONTEXT.md), Routing-by-task, Where new files go, Project Rules, Tech Stack, Gotchas, Inherits-from pointer. |
| `STATUS.md` | Latest delta + next pickup. Pure-function output of Mode 3. |
| `ROADMAP.md` | Priorities. Decisions split out to `docs/decisions/`. |
| `LOOKUP.md` | Topic-driven lookup hub (SOT registry, playbooks index, API constraints, external docs). |

`CLAUDE.md` (and other vendor-specific bootstrap files) become 1-line stubs pointing to `AGENTS.md` when present. If a project already uses a full `CLAUDE.md`, do not delete or rename it automatically; classify and migrate per Mode 1 refresh.

If both `AGENTS.md` and `CLAUDE.md` exist, classify the relationship before scoring drift:

- Legacy bridge: `AGENTS.md` clearly points to `CLAUDE.md` as current authority. Accept this, then suggest a fuller model-agnostic bridge only if the project needs it.
- Split authority: both files contain overlapping rules without saying which one wins. Flag as drift.
- Model-agnostic authority: `AGENTS.md` owns shared rules and platform-specific docs only add adapters. Prefer this for new infrastructure.

## Standard `docs/` Layout

**Opinionated bootstrap structure.** All 9 canonical subfolders are created at bootstrap by Mode 1, each with a `README.md` explaining what goes there. Administrative folders (`_archive/`, `_private/`) appear when content arrives. Every project has the same shape; agents always know where to look.

```text
docs/
  DOCS-INDEX.md            (created when docs/ exceeds 10 files)
  status-history/          (rotated STATUS.md backups; dated files)
  roadmap-history/         (rotated ROADMAP.md backups; dated files)
  decisions/               (one file per decision, dated; supersedes ROADMAP Decision Log)
  reference/               (verified-state SOT files)
  playbooks/               (process runbooks)
  dev/                     (architecture, schemas, API design)
  release/                 (release notes, migration steps)
  reviews/                 (code review, security audit)
  research/                (experiments, exploration)
  _archive/                (historical batches; appears on demand)
  _private/                (gitignored sensitive content; appears on demand)
```

`status-history/` and `roadmap-history/` follow the same convention: when the live file exceeds its length budget (STATUS > 200 lines, ROADMAP > 300 lines), rotate the oldest 50% to a dated file (`YYYY-MM-DD-NN.md`) before the next write. Both folders carry a `README.md` documenting the rotation policy.

Use `docs/DOCS-INDEX.md` when `docs/` has more than 10 docs.

## Modes

Each mode summary below is a 1-paragraph identifier. The full procedural workflow, rules, and any mode-specific tables live in the linked reference file.

### Mode 1, Init

Use when starting docs for a new project or bringing a thin project up to baseline. Reads existing root docs and manifests, identifies new vs legacy vs already-instrumented projects, creates or proposes the 5 canonical surfaces, prefers `AGENTS.md` for new infrastructure while preserving an existing `CLAUDE.md`, never invents stack/commands/credentials, and uses verify + snapshot components to seed initial handoff state. Outputs the canonical root docs, the initial docs folder map, and the first STATUS + ROADMAP.

**Full procedure:** [Mode 1, Init](reference/mode-1-init.md)

### Mode 2, Review

Use at session start or when the user asks for status. Reads the present authoritative bootstrap doc (AGENTS or CLAUDE), the v3 root files (CONTEXT, STATUS, ROADMAP, LOOKUP, README), and the docs index if present; runs a cheap drift check; summarizes current state, active work, blockers, and suggested next actions using the snapshot component. Read-only; never writes files or restructures. If drift exists, recommends running Update or Audit.

**Full procedure:** [Mode 2, Review](reference/mode-2-review.md)

### Mode 3, Update

Use at session end or after meaningful work. Captures the session delta with snapshot, runs security-preflight when docs were copied/moved/generated, verifies volatile claims, promotes durable learnings to the right memory layer, harvests reusable cross-project process when it appears, then updates STATUS + ROADMAP + LOOKUP + (sparingly) AGENTS, adds dated decision files, and rotates content when the STATUS or ROADMAP length budget is exceeded. Content-only; does not move or rename files.

**Full procedure:** [Mode 3, Update](reference/mode-3-update.md)

### Mode 4, Audit

Use when the user asks to clean up, reorganize, or check doc health. Runs security-preflight, inventories all Markdown docs, classifies canonical/ecosystem/orphan/private/archive/generated, verifies stale claims, scores health against the rubric below, detects anti-patterns via `reference/drift-checks.md`, proposes moves/renames/merges/archives + index updates, and waits for approval before executing any structural change. Never deletes (archives instead); executes approved changes atomically.

**Full procedure:** [Mode 4, Audit](reference/mode-4-audit.md)

### Mode 5, Inventory And Extract

Use when scanning a project for reusable learning. This mode owns the old `project-inventory` workflow; treat `../project-inventory/` as a trigger adapter. Runs security-preflight, reads bootstrap + index, inventories docs/playbooks/scripts/skills/templates/routines, classifies each item (reusable / project-specific / sensitive / stale-archive / unclear), uses harvest to separate reusable process from project facts, writes inventory to the shared knowledge-ops location, updates the scan-register, promotes durable learnings, and proposes extraction targets. Updates Layer 0 routing only after the reusable target exists. Does not move files during inventory.

**Full procedure:** [Mode 5, Inventory And Extract](reference/mode-5-inventory-extract.md)

### Mode 6, Project Ratchet

Use when the user wants iterative cleanup of **one project's docs** with scoring, usually on a duplicate before touching the real project. Targets project docs against the existing spec (does not modify the spec; that is Mode 7). Duplicate-first into a safe `test-runs/` fixture, run Audit for baseline, apply one cleanup strategy per attempt, re-run Audit, keep only if score improves and context preservation passes, repeat to target/max/stop, score the reusable workflow when a structural miss surfaces, write a delta report, and require approval before any change touches the real project. Enforces a bounded read-budget (rubric v2.0 D2) and a bounded timing target (rubric v2.0 D6); output must be deterministic per the sort + ISO-8601 rules; routing decisions follow `reference/drift-checks.md`, not per-finding judgment.

**Full procedure:** [Mode 6, Project Ratchet](reference/mode-6-project-ratchet.md) (includes the read-budget priority table, timing budget table, and Routing accuracy test-case reference)

### Mode 7, Doc Workflow Ratchet

Use when the user wants to evaluate the rules and workflows themselves, usually after two or three Project Ratchet runs. Targets **the doc workflow** (SKILL, rubric, components, reference files); uses evidence from Mode 6 runs as input. Reads evidence reports, scores project docs against the audit rubric, scores the reusable workflow via `components/workflow-score.md`, identifies structural misses the workflow should have caught earlier, changes one rule/rubric/component/routine at a time, runs the workflow-score script, keeps changes that improve readiness or close a hard gate without adding noise, defers project-specific or overlay-bound changes, and writes a workflow-autoresearch report. Never modifies real project folders. A workflow score of 95+ is required before scheduling.

**Full procedure:** [Mode 7, Doc Workflow Ratchet](reference/mode-7-doc-workflow-ratchet.md)

## Drift Checks

The deterministic detection table (anti-pattern code → grep signature → fix routing) lives in `reference/drift-checks.md`. Load it for Review (Mode 2) and Audit (Mode 4) workflows. The table also carries the `RT-01` through `RT-12` routing-accuracy test cases used to self-verify Mode 6 output.

For the rationale behind each anti-pattern code, see `reference/anti-patterns.md`.

## Audit Scoring Rubric

Use this 100-point rubric for doc-audit and test runs. Apply hard gates first, then score categories. Mode 4 owns this rubric; Mode 6 reuses it for baselines; Mode 7 scores the workflow alongside it.

### Hard Gates

These caps make the score harder to game:

| Condition | Score Cap |
|---|---:|
| Strict secret-pattern hit in active docs | 69 |
| Sensitive value appears in historical docs without redaction plan | 79 |
| `STATUS.md` over 200 lines | 84 |
| `ROADMAP.md` over 300 lines without extracted history | 84 |
| Missing `docs/DOCS-INDEX.md` when `docs/` has more than 10 files | 84 |
| Split authority between `AGENTS.md` and platform-specific AI docs | 79 |
| Broken links in current/root docs | 84 |
| Current-state claims are stale and lack verification notes | 79 |
| No history/index path for completed work | 84 |
| Current docs link into excluded private paths as normal navigation | 84 |

For scoped duplicate test runs, apply broken-link gates after source-aware resolution. A link is not broken if it points to a non-protected project path that exists in the source project but was intentionally omitted from the duplicate fixture. Report those fixture fallbacks separately.

| Category | Points | Checks |
|---|---:|---|
| Current-state accuracy | 15 | Active claims are current, verified, or explicitly marked for verification |
| Handoff and next-action quality | 15 | Live handoff is short, specific, and points to deeper history |
| Roadmap and priority hygiene | 15 | Roadmap shows current priorities without becoming a completed-work archive |
| Routing and discoverability | 15 | Indexes, references, and bootstrap docs route agents to the right places |
| Drift, duplication, and link integrity | 15 | No duplicate active/archive docs, no broken current-doc links, no split authority |
| Security and privacy hygiene | 15 | Sensitive docs are excluded, redacted, or watchlisted, and values are never quoted |
| History and decision promotion | 10 | Completed work goes to history, decisions are stored lower, active results are promoted higher |

Health bands:

- 90-100: strong, schedule-ready
- 85-89: healthy, but not automation-ready unless all hard gates are clear
- 75-84: improved but still blocked by at least one meaningful cleanup issue
- 60-74: useful but needs cleanup before routine automation
- 40-59: fragile, audit before relying on it
- 0-39: rebuild or deep cleanup needed

## Naming Rules

- Root canonical docs: `ALL-CAPS-KEBAB.md`
- `docs/` files: `lowercase-kebab.md`
- `docs/DOCS-INDEX.md` is the exception inside `docs/`
- Sort-to-bottom folders: `_archive/`, `_private/`
- Dates: `YYYY-MM-DD`
- Avoid spaces, camelCase, and underscores for word separation

## Where New Docs Go

| Type | Destination | Naming |
|---|---|---|
| Architecture, schema, API design | `docs/dev/` | `lowercase-kebab.md` |
| Verified config or source of truth | `docs/reference/` | `lowercase-kebab.md` |
| Repeatable runbook | `docs/playbooks/` | `<topic>-playbook.md` |
| Release notes or migration notes | `docs/release/` | `vN-notes.md` |
| Code review or audit report | `docs/reviews/` | `YYYY-MM-DD-<scope>.md` |
| Research or exploration | `docs/research/` | `lowercase-kebab.md` |
| Decision rationale (one per decision) | `docs/decisions/` | `YYYY-MM-DD-<topic>.md` |
| Rotated STATUS.md content | `docs/status-history/` | `YYYY-MM-DD-NN.md` |
| Rotated ROADMAP.md content | `docs/roadmap-history/` | `YYYY-MM-DD-NN.md` |
| Doc-system test reports | Shared `docs/test-runs/` location | `YYYY-MM-DD-<run>.md` |
| Sensitive docs | `docs/_private/` | `lowercase-kebab.md` |
| Historical/archive-only docs | `docs/_archive/` | `YYYY-MM-DD-batch/` |

## Output Shapes

### Review Output

```text
Summary
Current State
Active Work
Blockers
Suggested Next Actions
Drift Flags
```

### Inventory Output

```text
Summary
Source
Scanned Docs
Reusable Candidates
Project-Specific Docs
Sensitive Docs
Extraction Plan
Router Updates Needed
```

### Audit Output

```text
Summary
Health Score
Findings
Proposed Changes
Approval Items
Risk Notes
```

### Project Ratchet Output

```text
Summary
Baseline Score
Best Score
Score Delta
Attempts
Kept Strategies
Discarded Strategies
Context Preservation Notes
Workflow Readiness Score
Recommended Real-Project Changes
Approval Items
```

### Doc Workflow Ratchet Output

```text
Summary
Evidence Runs
Baseline Workflow Score
Final Workflow Score
Structural Misses Found
Experiments
Kept Rules
Rejected or Deferred Rules
Project Doc Score Impact
Workflow Score Impact
Real-Project Approval Checklist
Next Hardening Target
```

## Files This Skill May Update

When operating inside a target project, only that project's docs, and only in the active mode's allowed scope:

- The 5 root files: `README.md`, `AGENTS.md`, `STATUS.md`, `ROADMAP.md`, `LOOKUP.md`
- `CLAUDE.md` only as a 1-line stub if migrating from v1.x
- `docs/DOCS-INDEX.md` (regenerated when files move)
- `docs/decisions/*.md` (one file per decision, dated)
- `docs/status-history/*.md` (rotated STATUS backups)
- `docs/roadmap-history/*.md` (rotated ROADMAP backups)
- `docs/reference/*.md`, `docs/playbooks/*.md`, `docs/dev/*.md`, `docs/release/*.md`, `docs/reviews/*.md`, `docs/research/*.md`

When operating in shared knowledge-ops mode (e.g., a vantage-point or similar capability monorepo), the skill may additionally update inventories, scan-registers, test-runs, and routines under that shared location. See `internal-overlay.md` (if present) for monorepo-specific bindings.

## Safety Notes

- Any credential-bearing doc belongs on the security watchlist before extraction.
- Sensitive top-level folders (e.g., `+` prefixed in some monorepo conventions) are indexed in place until migration is approved.
- A scheduled routine should not exist until the manual routine has been run successfully.
