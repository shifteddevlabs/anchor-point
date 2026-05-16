---
name: anchor-point
type: capability
version: 3.1
repo_intent: published
status: active
owner: knowledge-ops
last_reviewed: 2026-05-15
applies_to_projects: all
github_repo: https://github.com/shifteddevlabs/anchor-point
description: Model-agnostic documentation operating system. Initializes, reviews, updates, audits, and hardens project docs across sessions and AI tools. Enforces the 5-root-file v3.1 standard (README, AGENTS, STATUS, ROADMAP, LOOKUP) plus drift detection. Use when setting up docs for a new project, recovering context at session start, wrapping up a session, auditing doc health, or improving the workflow itself.
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
  - optimize docs
  - inventory this project
  - harvest reusable learning
  - extract reusable docs
---

# Anchor Point

Model-agnostic documentation methodology. Manages project documentation as infrastructure across the full lifecycle: initialize docs, recover context, update handoffs, audit structure, and extract reusable knowledge.

This is the unified operational skill. For human-readable orientation see `identity.md`; for worked examples see `examples.md`; for reference material see `reference/`.

This skill is model-agnostic. It prefers `AGENTS.md` as the AI bootstrap document for new shared infrastructure, while respecting existing `CLAUDE.md` or other platform-specific files already present in projects.

**Working inside a monorepo or capability harness?** If a file named `internal-overlay.md` sits alongside this `SKILL.md`, read it first — it binds the generic references in this skill (Layer 0 home, shared knowledge-ops paths, workflow-score script, migration trackers) to your monorepo's actual paths. Public clones won't have one; that's fine, the methodology stands alone.

## Reference Docs

Read only what is needed for the current mode.

| Source | Use For |
|---|---|
| `reference/doc-architecture.md` | Canonical architecture spec (v3.1) |
| `reference/canonical-file-set.md` | File placement decision tree |
| `reference/anti-patterns.md` | Drift signatures (rationale for the codes used in `drift-checks.md`) |
| `reference/drift-checks.md` | Drift-check detection table (grep patterns + fix routing for Audit and Review modes) |
| `reference/naming-conventions.md` | Naming rules |
| `reference/doc-ratchet.md` | Iterative duplicate-folder cleanup loop |
| `reference/workflow-autoresearch.md` | Workflow scoring + autoresearch loop |

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
| `doc-ratchet` | security-preflight → snapshot → doc-audit baseline → duplicate cleanup attempts → verify → promote-memory if reusable learning appeared |
| `inventory and extract` | security-preflight → snapshot → harvest → promote-memory |

## Core Rules

### Layer 0 Hard Rules (cross-project baseline)

These apply to EVERY workflow, every session. Derived from cross-project incidents that have repeated 3+ times across real projects. Inherit them as-is; never edit or paraphrase.

1. **Verify before claiming.** Read the file, run the check, or ask the user. Never answer state-of-the-system questions from memory.
2. **Root-cause, not symptoms.** When something that worked is broken, identify what changed FIRST. Don't layer patches over a broken state.
3. **Quality gate every change.** Whatever the user's commit/push process is, follow it. Never skip review steps to save time.
4. **Delegate to focused investigation** when a task needs to read >3 unrelated files. Protects context.
5. **Trust documented state.** If a verified-state doc says X, use X. If state has drifted, UPDATE the doc — don't ask the user to re-verify what they already documented.
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

## Canonical Project Surfaces (v3.1)

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

### Mode 1, Init

Use when starting docs for a new project or bringing a thin project up to baseline.

Workflow:

1. Read existing `README.md`, `AGENTS.md`, `CLAUDE.md`, package manifests, and visible folder structure.
2. Identify whether the project is new, legacy, or already has a docs system.
3. Create or propose the canonical surfaces.
4. For new model-agnostic infrastructure, create `AGENTS.md`.
5. For existing Claude-shaped projects, preserve `CLAUDE.md` and only add `AGENTS.md` if the user wants cross-model routing.
6. Do not invent stack, commands, rules, or credentials.
7. Use source citations in generated tech-stack or command sections when possible.
8. Use `components/verify.md` for stack, command, or service claims.
9. Use `components/snapshot.md` to seed initial handoff state.

Outputs:

- created or proposed root docs
- initial docs folder map
- first `STATUS.md`
- initial `ROADMAP.md`

### Mode 2, Review

Use at session start or when the user asks for status.

Workflow:

1. Read `AGENTS.md` or `CLAUDE.md`, whichever is present and authoritative.
2. Read `CONTEXT.md`, `STATUS.md`, `ROADMAP.md`, `LOOKUP.md`, and `README.md` when present.
3. Skim `docs/DOCS-INDEX.md` if present.
4. Run a cheap drift check.
5. Use `components/snapshot.md` to summarize current state, active work, blockers, and suggested next actions.
6. Use `components/verify.md` only for claims that affect the next action.

Rules:

- Read-only.
- Do not write files.
- Do not perform structural cleanup.
- If drift exists, recommend Update or Audit.

### Mode 3, Update

Use at session end or after meaningful work.

Workflow:

1. Identify what changed this session.
2. Use `components/snapshot.md` to capture the session delta.
3. Use `components/security-preflight.md` if docs were copied, moved, generated, or newly scanned.
4. Use `components/verify.md` for volatile or high-impact claims.
5. Use `components/promote-memory.md` to place durable learnings.
6. Use `components/harvest.md` only if reusable cross-project process appeared.
7. Update `STATUS.md`.
8. Sync `ROADMAP.md` for completed and newly surfaced work.
9. Add decision rows when decisions were made.
10. Update `LOOKUP.md` when new source-of-truth or playbook docs appear.
11. Add persistent rules to `AGENTS.md` or project-local bootstrap docs only when a durable lesson emerged.
12. Rotate when needed: STATUS.md content older than the latest delta → `docs/status-history/`; ROADMAP.md completed-priority entries beyond the visible window → `docs/roadmap-history/`. Decision Log rows → `docs/decisions/` (one file per decision).

Rules:

- Content-only.
- Do not move or rename files.
- Do not rewrite project history.
- Keep handoff concise and specific.

### Mode 4, Audit

Use when the user asks to clean up, reorganize, or check doc health.

Workflow:

1. Use `components/security-preflight.md` before reading or moving broad doc sets.
2. Inventory all project Markdown docs.
3. Classify canonical docs, docs ecosystem files, orphan files, private docs, archives, and generated noise.
4. Use `components/verify.md` for stale or volatile claims.
5. Score health across structure, freshness, no drift, pointers, length, and handoff hygiene.
6. Detect anti-patterns.
7. Use `components/harvest.md` if reusable material appears.
8. Propose moves, renames, merges, archives, and index updates.
9. Wait for approval before executing structural changes.
10. After approved moves, update folder maps, indexes, and cross-references.

Rules:

- Approval required before structure changes.
- Never delete, archive instead.
- Do not move secret-bearing files without a private migration plan.
- Execute changes atomically.

### Mode 5, Inventory And Extract

Use when scanning a project for reusable learning.

This mode owns the old `project-inventory` workflow. Treat `../project-inventory/` as a trigger adapter, not a separate source of truth.

Workflow:

1. Use `components/security-preflight.md`.
2. Read project bootstrap docs and docs index.
3. Use `components/snapshot.md` for current project context.
4. Inventory docs, playbooks, scripts, skills, templates, routines, and notable process notes.
5. Exclude archives, dependency folders, build output, and generated draft folders unless the user asks.
6. Classify each item:
   - reusable
   - project-specific
   - sensitive
   - stale/archive
   - unclear
7. Use `components/harvest.md` to separate reusable process from project facts.
8. Write or update an inventory in your shared knowledge-ops `inventories/` location.
9. Update the inventories scan-register.
10. Use `components/promote-memory.md` to place durable learnings.
11. Propose extraction targets.
12. Update the Layer 0 home's `AGENTS.md` "Routing-by-task" section (or, in the rare case a workspace ROUTER.md is justified per Rule 5, that file) only after the reusable target exists.

Rules:

- Do not move files during inventory.
- Do not promote local facts into global rules.
- Do not quote secrets.
- Prefer rewriting reusable process over copying project-local docs verbatim.

### Mode 6, Ratchet

Use when the user wants iterative doc cleanup with scoring, usually on a duplicate before touching the real project.

Read `reference/doc-ratchet.md` before running this mode.

Workflow:

1. Use `components/security-preflight.md` to define excluded paths.
2. Duplicate only docs and needed manifests into a safe `test-runs/` fixture location outside the source project.
3. Verify the duplicate contains no protected paths before reading or scoring copied files.
4. If a protected path was copied, delete the fixture, rebuild it, and record the setup failure as a harness learning.
5. Record whether the duplicate is full-project or scoped-docs-only.
6. Run Audit on the duplicate to establish a baseline.
7. If the duplicate is scoped, validate Markdown links with fallback to the source project for intentionally omitted non-protected paths.
8. Apply one cleanup strategy to the duplicate.
9. Re-run Audit and compare scores.
10. Keep the attempt only if score improves and context preservation passes.
11. Repeat until the target score, max attempts, or stop condition.
12. Score the reusable workflow with `components/workflow-score.md` when the run exposes a structural miss.
13. Write a delta report to your shared `docs/test-runs/` location.
14. Ask before applying any changes to the real project and include the real-project approval checklist.

**Mode 6 read-budget (bounded; enforced):**

To meet rubric v2.0 D2 (Mode 6 budget 15,000 tokens) and HG9 (no run exceeds 2x budget), the baseline audit MUST read no more than this file set, in priority order. Stop reading additional files once the budget is consumed.

| Priority | Files to read | Why |
|---|---|---|
| 1 (always) | The 5 root .md files at most (README, AGENTS, STATUS or SESSION-HANDOFF, ROADMAP, LOOKUP or REFERENCES, and CLAUDE if present) | These hold the v3.0 canonical content; every drift check applies to them |
| 2 (always) | CONTEXT.md if present | Legacy v1.x signal; triggers A3 |
| 3 (on demand) | `docs/DOCS-INDEX.md` if present | Required for A10 / index drift check |
| 4 (on demand) | `docs/playbooks/*.md` heads (first 30 lines each) | Required for A7 file-scope; full read only if a head suggests detail |
| 5 (on demand) | `docs/reference/*.md` heads (first 30 lines each) | Required for A8 file-scope |
| 6 (on demand) | `docs/dev/*.md` heads (first 30 lines each) | Required for A7 file-scope |
| 7 (never in baseline) | `_archive/` and `_private/` content | HG1 — never read these in any mode |

Mode 6 baseline does NOT read source-code directories (`src/`, `app/`, `web/`, etc.), `sequences/`, `node_modules/`, build output, or any content folder. If the project's Mode 4 hard gate "ROADMAP > 300 lines" applies, the skill confirms via `wc -l` only, not by reading the full ROADMAP.

For projects with > 30 .md files in scope: read root + DOCS-INDEX (always); sample up to 10 docs from playbooks/reference/dev (by recency × size heuristic); stop. Note in the audit Summary that sampling was used.

**Mode 6 timing target (bounded; enforced):**

To meet rubric v2.0 D6 (Mode 6 cold-start budget 30 min) and WG4 (no run exceeds 2x budget = 60 min), the baseline audit MUST complete within 10 min of wall-clock time on typical-sized projects (≤ 30 .md in scope) and 20 min on large projects (sampling discipline applies). Time decomposition target:

| Phase | Budget | Why |
|---|---|---|
| Skill load (read SKILL.md, components, drift table, routing table) | 30 sec | One-time per session |
| Security preflight (verify no protected paths) | 30 sec | Fast directory scan |
| Inventory + sample selection | 1 min | File listing + heuristic |
| Read priority-1 + priority-2 files (root + CONTEXT) | 1 min | 5-7 files, ~10K tokens total |
| Read priority-3 + sampled priority-4/5/6 files | 2 min | DOCS-INDEX + 10 sampled docs |
| Apply 16 Drift Check Detection criteria | 2 min | Mostly grep/structural, deterministic |
| Compute Health Score (Mode 4 rubric) | 30 sec | Formula, deterministic |
| Write Audit Output (per schema) | 2 min | ~2K-token markdown |
| **Total** | **~9.5 min** | Under 10-min typical target; under 30-min Mode 6 hard budget |

If any phase exceeds 1.5x its target, the skill SHOULD note the slowdown in the Risk Notes section so the cause (e.g., unusual project size, content density) is attributable.

Rules:

- Duplicate first.
- One cleanup strategy per attempt.
- Never optimize score by deleting useful context.
- Never apply ratchet changes to a real project without approval.
- Do not count a scoped-fixture missing source directory as a broken doc link if the same non-protected target exists in the source project.
- Always count links into protected/private paths as failures for current navigation, even if the target exists.
- Treat product content, email sequences, blog posts, and embedded project content as separate inventory categories, not docs clutter.
- When a file moves into history, repair its relative links or explicitly mark it as an unlinked snapshot.
- **Output must be deterministic** for identical fixture inputs. Sort the Findings list by anti-pattern code first (A1, A2, ..., A13, then MG1, MG2, ...), then alphabetically by file path within each code. Sort Proposed Changes by target folder path (lexicographic). Use ISO-8601 timestamps only in the report's frontmatter `run-date` field, never inside finding bodies or score tables. Score values are integers in `[0, 100]`; never use floating point in dimension scores. Two runs on identical fixture inputs MUST produce byte-equivalent output modulo frontmatter timestamps and run UUIDs. (Rubric v2.0 D4 + HG3.)
- **Routing decisions follow `reference/drift-checks.md`, not per-finding judgment.** For each Finding, the Proposed Change is fully determined by its code via the detection table. No ad-hoc routing; if a code is not in the table, that is a skill-extension request (file an issue), not a judgment call.
- **Self-verify against `reference/drift-checks.md` "Routing accuracy test cases" (RT-01 through RT-11).** Any drift from those expected outputs is a routing-accuracy defect.

### Mode 7, Workflow Autoresearch

Use when the user wants to evaluate the rules and workflows themselves, usually after two or three project ratchet runs.

Read `reference/workflow-autoresearch.md` and `components/workflow-score.md`.

Workflow:

1. Read the evidence run reports.
2. Score the project docs using the audit rubric.
3. Score the reusable workflow using `components/workflow-score.md`.
4. Identify structural misses that the workflow should have caught earlier.
5. Change one workflow rule, rubric, component, or routine at a time.
6. Run the workflow-score script (`scripts/score-doc-workflow.js` in your shared knowledge-ops location).
7. Keep the change if workflow readiness improves or closes a hard gate without adding noise.
8. Defer changes that are project-specific or belong in an overlay.
9. Write a workflow-autoresearch report in your shared `docs/test-runs/` location.

Rules:

- Do not modify real project folders in workflow-autoresearch mode.
- Do not promote a one-project fact into a global rule.
- Optimize for safer future runs, not just a higher score.
- A workflow score of 95+ is required before scheduling.

## Drift Checks

The deterministic detection table (anti-pattern code → grep signature → fix routing) lives in `reference/drift-checks.md`. Load it for Review (Mode 2) and Audit (Mode 4) workflows. The table also carries the `RT-01` through `RT-11` routing-accuracy test cases used to self-verify Mode 6 output.

For the rationale behind each anti-pattern code, see `reference/anti-patterns.md`.

## Audit Scoring Rubric

Use this 100-point rubric for doc-audit and test runs. Apply hard gates first, then score categories.

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
| Release notes or migration notes | `docs/release/` | `vX.Y-notes.md` |
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

### Ratchet Output

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

### Workflow Autoresearch Output

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
