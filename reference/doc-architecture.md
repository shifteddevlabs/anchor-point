---
type: spec
repo_intent: published
status: active
owner: knowledge-ops
version: 3.2
last_reviewed: 2026-05-16
applies_to_projects: all
supersedes: v3.1 (added subfolder README discipline; AGENTS.md sizing rules via hot/warm test; CONTEXT.md reservation; project-level ROUTER.md thresholds; A14-A18; overdrive-lab extraction worked example)
---

# Doc Architecture v3.2

Canonical specification for how project docs are organized across all projects.
Tool-agnostic. Optimized for multi-LLM, multi-session, parallel-agent work.

## Why v3.0

v1.x described a 7-bucket system with separate CLAUDE.md, CONTEXT.md, and LOOKUP.md files. After cross-project review, three problems surfaced:

1. **Vendor-coupled bootstrap.** CLAUDE.md as the canonical agent file ties projects to one vendor; other agents (Codex, Cursor, Cowork) read it only by convention.
2. **Cross-cutting partitions.** CONTEXT.md, CLAUDE.md, and LOOKUP.md sliced content by different axes (time-scale, audience, topic). Agents faced 3-axis decisions to route content. Anti-patterns A1/A9/A10 in v1.2 existed because these splits were fuzzy.
3. **Session-end procedural skill.** doc-update was a 11-step procedure with judgment calls per step. Not idempotent. Different agents produced different outputs.

v3.0 fixes these by collapsing to 5 root files, making AGENTS.md the canonical bootstrap, treating doc-handoff as an idempotent pure function, and enforcing single-owner facts.

## Foundational principles

| Principle | Why |
|---|---|
| **Agent-agnostic** | Files are plain Markdown. No vendor-specific directives. Claude, Codex, Cursor, Gemini all parse identically. |
| **Multi-session safe** | Markdown is lock-free for reads. Writes are append-only or atomic-rewrite. No agent "owns" a file mid-session. |
| **Token-economical** | Hot files load every session (~3-5K tokens total). Warm files load on demand. Cold files (spec, archive) load only at design or audit time. |
| **Single-owner facts** | Every fact has ONE canonical home. Other appearances are pointers, not duplicates. |
| **Pure-function operations** | Session-end skill is idempotent. Same inputs → same outputs. No judgment at handoff time. |
| **Append-only or atomic writes** | Parallel-agent work doesn't require file locks. |

## Root files (5 canonical)

| File | Purpose | Audience | Length cap |
|---|---|---|---|
| README.md | Public face | Humans | 50-100 lines |
| AGENTS.md | Canonical agent bootstrap (rules, identity, current phase, routing, tech stack, gotchas) | All agents | 200 lines |
| STATUS.md | Latest delta and next-pickup pointer | All agents | 150 lines |
| ROADMAP.md | Priorities and decisions | Humans + agents | scales |
| LOOKUP.md | Topic-driven lookup (SOT files, playbooks, external docs) | All agents | 150 lines |

CLAUDE.md is a 1-line stub pointing to AGENTS.md, for Claude Code's expected filename. Other tool-specific bootstrap files (.cursorrules, etc.) can also be stubs pointing to AGENTS.md.

**Why 5, not 6.** v1.x had a separate CONTEXT.md for "current phase." In v3.0, current phase is a section inside AGENTS.md. Reducing root file count by one eliminates a routing decision agents made every session.

## docs/ ecosystem

```
docs/
  DOCS-INDEX.md            (or INDEX.md; navigation aid for >10 docs)
  status-history/          (rotated STATUS.md backups; dated files YYYY-MM-DD-NN.md)
  roadmap-history/         (rotated ROADMAP.md backups; dated files YYYY-MM-DD-NN.md)
  decisions/               (one file per decision, dated; supersedes ROADMAP Decision Log)
  reference/               (verified-state SOT files)
  playbooks/               (process runbooks)
  dev/                     (architecture, schemas, API design)
  release/                 (release notes, migration steps)
  reviews/                 (code review, security audit)
  research/                (experiments, exploration)
  _archive/                (historical batches, dated)
  _private/                (gitignored sensitive content)
```

**Opinionated bootstrap structure.** All 9 canonical subfolders (`status-history/`, `roadmap-history/`, `decisions/`, `reference/`, `playbooks/`, `dev/`, `release/`, `reviews/`, `research/`) are created at bootstrap, each with a `README.md` explaining what goes there. Administrative folders (`_archive/`, `_private/`) appear when content arrives. Every project has the same shape; zero variance; predictable navigation. Empty folders carry signal (the README defines purpose), not noise.

`status-history/` and `roadmap-history/` follow the same rotation convention: when the live file exceeds its length budget (STATUS > 200 lines, ROADMAP > 300 lines), rotate the oldest 50% (or completed-priority sections beyond the visible window) to a dated file (`YYYY-MM-DD-NN.md`) before the next write. doc-handoff (Mode 3) does this automatically. `decisions/` is the parallel for decision rationale, but uses per-decision filenames (`YYYY-MM-DD-<topic>.md`) instead of rotation-batch naming.

## The 8 AGENTS.md sections (fixed schema, in order)

| # | Section | What it contains | Length |
|---|---|---|---|
| 1 | Identity | Who this project is. One paragraph. | 5-10 lines |
| 2 | Current Phase | What we are building now. Success criteria. Out of bounds. (absorbed CONTEXT.md) | 10-20 lines |
| 3 | Routing-by-task | Table: "If you need X, open Y" with **direct file paths**, not pointers-to-a-registry. | 10-20 rows |
| 4 | Where new files go | Table: content type → folder + naming pattern. | 8-12 rows |
| 5 | Project Rules | Project-specific persistent rules. Use **Burned:** [incident] format for incident-driven rules. | 5-30 lines |
| 6 | Tech Stack | Single-owner table. Versioned. Source column cites where each fact came from. | 5-20 rows |
| 7 | Gotchas | Project-specific persistent warnings. Sentence-form discipline: imperative=rule, declarative=fact, conditional=recipe. | 5-30 lines |
| 8 | Inherits from | Pointer to <your Layer 0 home> for cross-project Hard Rules (e.g., `+vantage-point/AGENTS.md` in a vantage-point monorepo). NO duplication of Layer 0 rules in this file. | 2-3 lines |

**Routing-by-task table** is the v3.0 winning move. It replaces the v1.2 SOT registry in LOOKUP.md by holding direct file paths inline. Agent never needs an intermediate "look up which SOT file to consult" step. The agent reads AGENTS.md once at session start and has every routing decision pre-loaded.

## STATUS.md schema (pure-function output)

doc-handoff produces STATUS.md deterministically from these inputs:

```
INPUTS:
  - git log since last write
  - file diffs
  - pre-routed Asks captured during the session

OUTPUTS (fixed schema, in order):
  1. TL;DR                       (2 lines)
  2. Last session shipped        (computed from git log)
  3. Next session                (3 bulleted items; pointer to ROADMAP top 3 if exists)
  4. Active blockers
  5. Debugging Playbook          (conditional recipes that survived prior sessions)
```

**Idempotency rule.** Running doc-handoff twice on the same inputs produces an identical file. No accumulated state. No "append" mode that depends on previous SESSION-HANDOFF content. The previous version rotates to `docs/status-history/` BEFORE the new write.

**Pre-routed Asks.** When an agent mid-session captures an "ask the docs should have answered," it tags the destination home immediately (not deferred to session end). Example: `[AGENTS.md: never log raw HealthKit data — add as Burned rule]`. At session end, doc-handoff executes pre-tagged moves; it does not decide where each Ask goes.

## LOOKUP.md scope (tightened in v3.0)

LOOKUP.md is the topic-driven lookup hub. It contains tables of pointers, not original content:

```
- SOT registry             (table: topic → docs/reference/<file>.md)
- Playbooks index          (table: topic → docs/playbooks/<file>.md)
- API Constraints          (table: integration-specific facts only)
- External docs            (URLs for SDK/framework docs)
- External infrastructure  (GCP project IDs, billing IDs; NEVER secrets)
- Skill ecosystem          (which shared skills apply; path bound via `internal-overlay.md`)
```

**Single-owner discipline:**
- Persistent rules → AGENTS.md "Project Rules"
- Session-volatile hazards → STATUS.md
- Cross-project conventions → <your Layer 0 home> (inherited, never duplicated in LOOKUP.md; e.g., `+vantage-point/AGENTS.md` in a vantage-point monorepo)
- File inventory → docs/DOCS-INDEX.md (location-driven), NEVER duplicated in LOOKUP.md (anti-pattern A10)

## Routing decision: the agent's mental model

```
Agent has new content to file
  │
  ▼
Read row in AGENTS.md "Where new files go" table (pre-loaded)
  │
  ├── Architecture / API design / schema      ──► docs/dev/<topic>.md
  ├── Process runbook ("when X, do Y")        ──► docs/playbooks/<topic>-playbook.md
  │                                              + LOOKUP.md Playbooks row
  ├── Verified-state config (IDs, env, version)──► docs/reference/<topic>.md
  │                                              + LOOKUP.md SOT row
  ├── Code review / security audit            ──► docs/reviews/YYYY-MM-DD-<scope>.md
  ├── Release notes                           ──► docs/release/vX.Y-notes.md
  ├── Research / exploration                  ──► docs/research/<topic>.md
  ├── Decision rationale                      ──► docs/decisions/YYYY-MM-DD-<topic>.md
  └── Persistent rule for this project        ──► AGENTS.md "Project Rules"
                                                 (with Burned: [incident] format)
```

## Single-owner facts: imperative/declarative test

For any new piece of information, the home is determined by sentence form:

| Sentence form | Example | Home |
|---|---|---|
| Imperative ("do X" / "don't Y") | "Never log raw HealthKit data" | AGENTS.md "Project Rules" |
| Declarative ("X is Y") | "Gemini 3.1 Flash-Lite is the production model" | AGENTS.md Tech Stack OR docs/reference/ |
| Conditional ("if X breaks, do Y") | "If Meta OAuth fails, run vercel env pull first" | STATUS.md Debugging Playbook |

No judgment about "is this a rule or a constraint?" — it is whichever form the sentence takes.

## Cross-project inheritance: Layer 0 home

Cross-project rules (Layer 0 baseline, naming conventions, routing conventions, hard rules) live ONCE at your Layer 0 home (e.g., `+vantage-point/AGENTS.md` in a vantage-point monorepo; path bound via `internal-overlay.md`). Per-project AGENTS.md files inherit by reference:

```
## Inherits from
This project inherits the Hard Rules baseline + naming + routing conventions
from <your Layer 0 home>. See that file before applying project-specific
rules below.
```

No project re-pastes the 7 universal Hard Rules. Cross-tool agents (Claude, Codex, Cursor) read the inherited file when bootstrapping.

## Concurrency model (multi-agent, multi-session)

| File | Write pattern | Conflict risk |
|---|---|---|
| AGENTS.md | Edited rarely (audit time) | Very low |
| STATUS.md | Single writer at session end | None (sequential by definition) |
| ROADMAP.md | Single writer at session end | None |
| docs/decisions/ | **Append-only**, one file per decision | None (unique filenames) |
| docs/status-history/ | **Append-only**, one file per session | None |
| docs/reference/<topic>.md | One editor typical | Low; git merge if collision |

For truly parallel sessions: each agent writes its own decision + history files. STATUS-like state is the union of latest STATUS.md + decision files since.

No file locks needed. Git merge resolves edge collisions.

## Token tiers

| Tier | Files | Loaded when |
|---|---|---|
| HOT | AGENTS.md (~3K tokens), STATUS.md (~1.5K tokens) | Every session start |
| WARM | docs/reference/<topic>.md, docs/playbooks/<topic>-playbook.md | On demand, opened directly from AGENTS.md routing table |
| COLD | docs/status-history/, docs/_archive/, this spec | Only at audit time or design time |

**Token budget at cold start: ~4.5K tokens total.** v1.2 cold start was ~8-10K tokens with CLAUDE.md + CONTEXT.md + LOOKUP.md + DOCS-INDEX.md.

## Subfolder discipline (v3.2)

v3.2 codifies how `docs/` subfolders, DOCS-INDEX.md, and CONTEXT.md naming interact. Origin: 2026-05-16 evaluation against Jake Van Cleef's CONTEXT.md proposal and Codex's progressive-disclosure framing surfaced three unaddressed gaps in v3.1's subfolder shape.

### Rule 1 — Subfolder README content

A subfolder `README.md` (the one inside `docs/dev/`, `docs/decisions/`, etc.) contains ONLY:

- **Purpose** — 1 line
- **What goes here** — 3-5 bullets
- **Out of scope** — 1-2 bullets

It does NOT list individual files. `docs/DOCS-INDEX.md` owns file inventory; duplicating it inside subfolder READMEs is anti-pattern A14.

**Skip rule:** if a subfolder has fewer than 3 files AND its purpose is obvious from the folder name, skip the README entirely. DOCS-INDEX.md handles the navigation.

### Rule 2 — DOCS-INDEX.md is location-only

`docs/DOCS-INDEX.md` MUST NOT contain a "Quick Start" or task-routing table. That duplicates AGENTS.md §3 (Routing-by-task). Anti-pattern A17.

DOCS-INDEX.md answers "what files exist here?" only. Task-driven routing ("if you're about to do X, open Y") lives in exactly one place: AGENTS.md §3.

### Rule 3 — CONTEXT.md naming is reserved

CONTEXT.md is NOT a default file type in v3.x. The name is reserved for one narrow case: agent-only working folders (e.g., `docs/_private/`, `scripts/` working notes) where the file is genuinely "what you need in your head to operate in here," not folder documentation. Even then, it is only useful if a parent file (AGENTS.md, README.md) routes to it explicitly.

For all human-discoverable / GitHub-rendered folders, the convention is README.md.

**Why this matters:** v3.0 explicitly removed CONTEXT.md from root files because it added a routing axis. Reintroducing CONTEXT.md at subfolder level — even with good intent — recreates that friction. The spec closes the door so future agents don't re-propose it under a different framing. See anti-pattern A15.

### Rule 4 — AGENTS.md size discipline + extraction order

When AGENTS.md exceeds the 200-line cap, the FIRST move is NOT to split routing. The routing table is the highest-value hot content. **Gotchas also stay** — they're proactive warnings the agent must absorb at session start to avoid; moving them to a reference file defeats the purpose (the agent doesn't open `gotchas.md` until after the mistake).

**Hot-vs-warm test.** For any AGENTS.md section, ask: does the agent need this in context to AVOID a mistake (HOT), or will they QUERY it when relevant (WARM)?

- HOT → stays in AGENTS.md regardless of length
- WARM → extracts to `docs/`

Sections that MUST stay in AGENTS.md (HOT — proactive avoidance content):

| Section | Why hot |
|---|---|
| §1 Identity | Sets frame for every decision |
| §2 Current Phase | Constrains what's in/out of scope |
| §3 Routing-by-task | The agent's primary navigation table |
| §4 Where new files go | Prevents misfiling at write-time |
| §5 Project Rules | Imperatives ("never X", "always Y") — must be in head |
| §7 Gotchas | Declarative warnings — agent can't query them they don't know they exist |
| §8 Inherits-from | Pointer to baseline |

Extraction order when over cap (WARM content first):

1. **Tech Stack** → if >15 rows, extract to `docs/reference/tech-stack.md`. Warm: versions get queried when relevant; agent doesn't step on them. AGENTS.md keeps a 3-line pointer with file location + last-verified date.
2. **Code-pattern sections** (Supabase patterns, API patterns, React patterns, etc.) → `docs/dev/<topic>.md`. Warm: agent opens when writing in that area. AGENTS.md lists imperative rules, not pattern reference. See anti-pattern A16.
3. **Directory structure / file tree** → `docs/dev/file-structure.md`. Warm: opened when navigating, not when operating.
4. **Only after** above, if §3 Routing-by-task itself exceeds ~25 rows, consider splitting to a project-level `ROUTER.md` (rare — see Rule 5).

If §7 Gotchas alone exceeds ~40 lines, that's a signal to AUDIT (some "gotchas" may have decayed into stale facts or non-warnings) — prune, don't relocate.

### Rule 5 — Project-level ROUTER.md thresholds

A project-level `ROUTER.md` is NOT recommended by default. Routing belongs in AGENTS.md §3, inline.

A split is justified only when ALL of these are true:

- Routing-by-task table exceeds ~25 rows, AND
- Project has ≥3 distinct task families with their own routing tables (e.g., dev + ops + marketing), AND
- AGENTS.md extraction of Tech Stack / patterns / file-structure has already happened and isn't enough

Splitting routing prematurely re-introduces the v1.x "look up which routing file to consult" intermediate hop that v3.0 specifically eliminated. See anti-pattern A18.

**Workspace-level exception.** A workspace root (e.g., `+vantage-point/` in a monorepo) IS large enough to justify a dedicated ROUTER.md — its routing covers many departments + skills + routines. The project-level threshold doesn't apply.

### Rule 6 — Workspace cheat-sheet ↔ ROUTER.md must not duplicate

If a workspace root has both an AGENTS.md "Common Tasks Cheat Sheet" and a separate ROUTER.md, exactly one of them owns task→load routing. Resolution options:

- Collapse the cheat-sheet to a 1-line pointer ("see ROUTER.md for task routing"), OR
- Fold ROUTER.md back into AGENTS.md if its content is no longer scale-justified

This is the workspace-level analog of anti-pattern A10.

## Anti-patterns (drift signatures)

Carries forward A1-A10 from v1.2; reframed for v3.0. doc-audit detects each.

| Code | Anti-pattern | Fix |
|---|---|---|
| A1 | Duplicate rules in 2+ files | Single-owner: rules in AGENTS.md only; other files point. |
| A2 | Architecture hiding in STATUS.md | Move to docs/dev/. |
| A3 | Session-volatile content in CONTEXT.md | N/A in v3.0 (CONTEXT folded). If CONTEXT.md exists, migrate to AGENTS.md "Current Phase" + STATUS.md. |
| A4 | STATUS.md > 200 lines | Rotate oldest 50% to docs/status-history/<date>-<NN>.md. |
| A5 | Stale tech-stack claims | Cross-check AGENTS.md tech stack against actual code (manifest grep). |
| A6 | Pointer-only files | Add original summary content. |
| A7 | Detail leakage into top-level | Extract to docs/release/ or docs/dev/. |
| A8 | Hypothesis as fact | Mark as hypothesis; add verification path. |
| A9 | Parallel priority list in SESSION-HANDOFF | One forward-looking section pointing to ROADMAP top 3. |
| A10 | Project documentation duplicates DOCS-INDEX | DOCS-INDEX.md owns file inventory; LOOKUP.md owns topic lookup. |
| **A11** (new in v3.0) | Hard Rules baseline duplicated in project AGENTS.md instead of inherited | Use `## Inherits from` pointer; do not paste baseline. |
| **A12** (new in v3.0) | Asks accumulated in SESSION-HANDOFF without routing tags | Pre-route Asks at write-time, not session-end. |
| **A13** (new in v3.0) | doc-handoff output drifts when re-run on same inputs | Skill must be idempotent; same inputs → same outputs. |
| **A14** (new in v3.2) | Subfolder README lists files (duplicates DOCS-INDEX) | Strip file list; keep purpose + "what goes here" + "out of scope" only. Skip the README entirely if <3 files. |
| **A15** (new in v3.2) | CONTEXT.md used as folder documentation in a human-visible folder | Rename to README.md. CONTEXT.md is reserved for agent-only working folders that a parent file explicitly routes to. |
| **A16** (new in v3.2) | Pattern/architecture content in AGENTS.md (Supabase patterns, API patterns, React patterns) | Extract to `docs/dev/<topic>.md`. AGENTS.md links to it. Reference material is warm, not hot. |
| **A17** (new in v3.2) | DOCS-INDEX.md has a Quick Start / task-routing table | Strip it. AGENTS.md §3 is the only task→file routing table. DOCS-INDEX.md is location-only. |
| **A18** (new in v3.2) | Project-level ROUTER.md split when justification thresholds (Rule 5) not met | Fold back into AGENTS.md §3. Splits cost more in load decisions than they save in line count. |
| MG1 | Active doc → `_private/` routing | Move pointer to "Sensitive SOT register"; do not route as primary navigation. |
| **MG2** (expanded in v3.1) | ROADMAP > 300 lines without extracted decisions OR rotated history | Two-part fix: (a) extract Decision Log → `docs/decisions/`; (b) rotate completed-priority sections → `docs/roadmap-history/`. |
| MG3 | Broken link in current/root docs | Update link target. Suppress when link is also MG1 (precedence). |
| MG4 | Split authority between AGENTS.md and CLAUDE.md | AGENTS.md sole authority; CLAUDE.md = 1-line stub. |

## Workflow skills (operate on this spec)

This spec is consumed by `SKILL.md` in the anchor-point skill folder, which owns the 7 modes (Init, Review, Update, Audit, Inventory & Extract, Ratchet, Workflow Autoresearch). Mode definitions live in SKILL.md, not here. This document is the rationale + canonical schema.

## What changed v1.2 → v3.0 (changelog)

| Change | Reason |
|---|---|
| AGENTS.md is canonical; CLAUDE.md becomes 1-line stub | Agent-agnostic; cross-tool work without vendor lock-in |
| CONTEXT.md absorbed into AGENTS.md "Current Phase" section | Eliminates one root file; reduces routing decisions |
| 7 root files → 5 root files | Reduced cognitive surface area |
| Routing-by-task table in AGENTS.md holds DIRECT paths | Eliminates SOT-registry intermediate hop |
| Decision Log moves from ROADMAP.md to docs/decisions/<date>-<topic>.md (one file each) | ROADMAP focuses on "what's next"; decisions get their own home |
| doc-handoff redefined as idempotent pure function | Bulletproof session-end; deterministic outputs |
| Pre-routed Asks (decisions at write-time, not session-end) | Eliminates judgment calls during handoff |
| Single-owner facts (imperative/declarative test) | Removes rule-vs-fact ambiguity |
| Layer 0 Hard Rules inherited from Layer 0 home (e.g., `+vantage-point/AGENTS.md`), not duplicated | Removes 11 lines of ceremony per project |
| Red/yellow/green priority colors are optional, not required at bootstrap | Lower friction for new project adoption |
| `docs/` subfolders all created at bootstrap with README stubs (v3.0); 9 canonical subfolders by v3.1 | Predictable shape per project; README stubs document purpose so empty folders carry signal, not noise |
| Added anti-patterns A11, A12, A13 | Codify new drift modes specific to v3.0 |
| Token cost cold start: 8-10K → ~4.5K | Routing tables pre-loaded; fewer files needed |

## Migration from v1.2 projects

For projects already on v1.2 shape:

1. Run Anchor Point Mode 1 (Init) with `refresh` flag
2. CONTEXT.md content merges into AGENTS.md "Current Phase" section
3. Layer 0 rules in CLAUDE.md replaced by inherit pointer to <your Layer 0 home> (e.g., `+vantage-point/AGENTS.md`)
4. CLAUDE.md becomes 1-line stub if it remains (delete optional)
5. LOOKUP.md project-documentation section deleted (A10)
6. ROADMAP.md Decision Log entries split into individual files in docs/decisions/

doc-audit Mode 4 detects all v1.2 patterns and proposes migrations atomically.

## Migration from no-docs projects (greenfield)

Anchor Point Mode 1 (Init) creates the 5 root files plus the full docs/ ecosystem:

**Root files (all 5 created at bootstrap):**
- README.md
- AGENTS.md (with Identity, Current Phase, Routing, Where new files go, Tech Stack)
- STATUS.md (empty stub)
- ROADMAP.md (empty or seeded with current priorities)
- LOOKUP.md (topic-driven lookup hub; seed rows added as SOT/playbook files appear)

**docs/ subfolders (all 9 created at bootstrap, each with a README.md stub):**
`status-history/`, `roadmap-history/`, `decisions/`, `reference/`, `playbooks/`, `dev/`, `release/`, `reviews/`, `research/`.

`docs/DOCS-INDEX.md` is created when `docs/` accumulates more than ~10 content files (an index isn't useful before then). Real content files inside each subfolder are added on demand; the bootstrap stubs document what goes where so agents always know the right destination.

## Appendix: Worked example — extracting an over-cap AGENTS.md

Worked example for Rule 4. Real case: `overdrive-lab/AGENTS.md` measured at 552 lines on 2026-05-16 (2.75x the 200-line cap). 14 sections; most of the bloat was reference/pattern content stuffed inline.

**Before (552 lines):**

| Section | Lines | Hot or warm? |
|---|---|---|
| Before Writing Code | 10 | Hot (imperatives) |
| Project Identity | 10 | Hot |
| Tech Stack (ENFORCED) | 57 | Warm — versions queried, not stepped on |
| Directory Structure | 50 | Warm — opened when navigating |
| Supabase Patterns (CRITICAL) | 67 | Warm — pattern reference |
| API Route Patterns | 101 | Warm — pattern reference |
| Design System | 43 | Warm — pattern reference |
| TypeScript Rules | 31 | Warm — pattern reference |
| React/Next.js Patterns | 54 | Warm — pattern reference |
| AI Agent Behavior Rules | 29 | Hot (imperatives) |
| Agency Voice (REQUIRED) | 15 | Warm — referenced when writing copy |
| Anti-Patterns (FORBIDDEN) | 38 | **Hot** — gotchas/warnings agent must avoid |
| Key Reference Files | 15 | Hot (short pointer table) |
| Environment Variables | 15 | Hot (short pointer table) |

**Extraction map (apply Rule 4 in order):**

| From section | To file | Pointer left in AGENTS.md |
|---|---|---|
| Tech Stack | `docs/reference/tech-stack.md` | 3-line pointer (file + last-verified date + key versions) |
| Directory Structure | `docs/dev/file-structure.md` | 1-line pointer |
| Supabase Patterns | `docs/dev/supabase-patterns.md` | 1-line pointer in §3 Routing-by-task ("Supabase work → docs/dev/supabase-patterns.md") |
| API Route Patterns | `docs/dev/api-route-patterns.md` | 1-line pointer in §3 |
| Design System | `docs/dev/design-system.md` (or merge into existing brand voice file) | 1-line pointer in §3 |
| TypeScript Rules | `docs/dev/typescript-rules.md` | 1-line pointer in §3 |
| React/Next.js Patterns | `docs/dev/react-nextjs-patterns.md` | 1-line pointer in §3 |
| Agency Voice | `docs/reference/voice.md` (or existing voice file) | 1-line pointer in §3 |

**Sections that STAY in AGENTS.md:**

- Before Writing Code (imperatives → §5 Project Rules)
- Project Identity → §1 Identity
- AI Agent Behavior Rules → §5 Project Rules
- Anti-Patterns → §7 Gotchas (audit first; some may be stale and prunable)
- Key Reference Files + Env Vars (already short pointer tables)

**Projected after extraction:** ~160-180 lines, within the 200-line cap with gotchas preserved hot.

**Important — what NOT to do in this worked example:**

- Do NOT move Anti-Patterns (gotchas) to `docs/reference/gotchas.md`. The agent won't open that file proactively; gotchas exist to prevent mistakes the agent doesn't know they're about to make. Audit the 38 lines for staleness, then keep what survives.
- Do NOT split §3 Routing-by-task into a separate ROUTER.md. Routing is the highest-value hot content; Rule 5 thresholds aren't met here.
- Do NOT execute this extraction as part of writing the spec patch. The spec change comes first; per-project migrations follow as separate tasks.

## Future: autoresearch evaluation of Anchor Point itself

Anchor Point itself is subject to optimization via the Workflow Autoresearch mode. The Skill Performance Rubric (path bound via `internal-overlay.md`; e.g., `+vantage-point/docs/architecture/skill-performance-rubric.md` in a vantage-point monorepo) defines the metrics. Loop: run, score, mutate skill language, run again, keep improvements, repeat.

## Versioning

| Version | Date | Author | Notes |
|---|---|---|---|
| 1.0 | 2026-05-08 | knowledge-ops | Initial 7-bucket spec |
| 1.1 | 2026-05-08 | knowledge-ops | Layer 0 baseline, SOT registry, Debugging Playbook, A8 |
| 1.2 | 2026-05-09 | knowledge-ops | A9, A10; REFERENCES rename API Constraints |
| 3.0 | 2026-05-14 | knowledge-ops | AGENTS canonical; CONTEXT absorbed; 5 root files; pure-function doc-handoff; pre-routed Asks; single-owner facts; A11-A13 |
| 3.1 | 2026-05-15 | knowledge-ops | Added `docs/roadmap-history/` bucket (parallel to `status-history/`); expanded MG2 to two-part fix (decisions + history rotation); 9 canonical subfolders bootstrap; canonical home moved to `+vantage-point/skills/anchor-point/reference/doc-architecture.md` post-merger |
| 3.2 | 2026-05-16 | knowledge-ops | Subfolder discipline (6 rules: README content, DOCS-INDEX location-only, CONTEXT.md reservation, AGENTS.md hot/warm sizing + extraction order, project ROUTER thresholds, workspace cheat-sheet ↔ ROUTER deduplication); A14-A18 anti-patterns; overdrive-lab worked example. Origin: 2026-05-16 evaluation against Jake Van Cleef CONTEXT.md proposal + Codex progressive-disclosure framing. |

(Version 2.0 was a drafting checkpoint; never released.)
