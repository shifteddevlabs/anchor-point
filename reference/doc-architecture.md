# Doc Architecture — The 7-Bucket System

Canonical specification for how AI-friendly project docs are organized.

This is the source of truth that the Anchor Point specialist reads from. It defines what files exist, what each contains, and how the six workflows interact with them.

---

## TL;DR

Every project has 7 buckets of doc content. Each bucket lives in exactly ONE file or folder. No drift, no duplication, no "where does this go?" hesitation.

| # | Bucket (plain English) | Lives in |
|---|------------------------|----------|
| 1 | "What is this?" (public face) | `README.md` |
| 2 | "Who am I, what are the rules?" (AI bootstrap) | `AGENTS.md` |
| 3 | "What we ARE doing" (current phase) | `CONTEXT.md` |
| 4 | "What we just did + what's next" (latest delta) | `SESSION-HANDOFF.md` |
| 5 | "What we NEED to do" (priorities) | `ROADMAP.md` |
| 6 | "Everything else doc-related" (the docs ecosystem) | `docs/` (with `DOCS-INDEX.md` as navigation aid) |
| 7 | "Where do I look stuff up?" (topic-driven lookup index) | `REFERENCES.md` |

**6 root files** (README, AGENTS, CONTEXT, SESSION-HANDOFF, ROADMAP, REFERENCES). Five are AI-facing; README is for humans.

**Everything else lives under `docs/`** — history (completed work, old handoffs, retired roadmap detail), reference (verified-state SOT files), playbooks (process runbooks), dev (architecture docs), release (release notes), reviews (code reviews), research (experiments / exploration), `_archive/` (historical), `_private/` (gitignored sensitive). `docs/DOCS-INDEX.md` is the navigation aid that maps the folder.

**REFERENCES.md vs DOCS-INDEX.md** are complementary, not duplicates: REFERENCES.md (root) is organized by TOPIC; DOCS-INDEX.md (in `docs/`) is organized by LOCATION. Small projects (≤10 docs in `docs/`) can OMIT DOCS-INDEX.md; large projects require it.

**Tool-specific adapters** are wrappers, not sources of truth. If a platform expects `CLAUDE.md`, a slash command, a scheduler entry, or a skill manifest, the adapter points back to `AGENTS.md` and the Anchor Point capability.

---

## The buckets in detail

### 1. `README.md` — "What is this?"

**Audience:** GitHub visitors, contributors, public.
**Updates:** Release cuts (rare).
**Length:** 50-100 lines.
**Contains:** project tagline, badges, quick start (3-5 steps), features (high level), pricing (if applicable), tech stack (high level, no version pins), doc index pointer, license.
**Does NOT contain:** internal gotchas, session-specific state, roadmap details, AI-agent-specific instructions.

### 2. `AGENTS.md` — "Who am I, what are the rules?"

**Audience:** AI tools (every session start).
**Updates:** Architecture changes (rare). New gotchas added immediately.
**Length:** 100-200 lines max.
**Contains (in order):**
1. Identity (1-2 paragraphs)
2. Hard Rules — Inherited Baseline (the 7 universal rules verbatim)
3. Project-specific Rules (Burned: format for incident-driven rules)
4. Folder Structure (top-level + key subdirs)
5. Routing-by-task (where to look for X)
6. Project Overview
7. Tech Stack (versioned table with Source column)
8. Tool-usage rules (consult library docs first; verify env vars; never assume SaaS UI; SOT > summaries)
9. Pricing (if applicable)
10. Environment Variables (names only, never values)
11. Development Commands
12. Important Notes / Gotchas

**Does NOT contain:** external URLs (those go in REFERENCES.md), lookup tables (REFERENCES.md), current session state (CONTEXT.md or SESSION-HANDOFF.md), priorities (ROADMAP.md).

### 3. `CONTEXT.md` — "What we ARE doing"

**Audience:** AI tools.
**Updates:** Phase boundaries (~monthly). Most sessions skip it entirely.
**Length:** 30-50 lines. Tight. Forward-facing.
**Contains:**
- What we're building (the current phase's goal, one paragraph)
- What good looks like (success criteria)
- What's out of bounds (don't-touch list)
- What I will not assume this phase (verification table)

**Test for any line:** "Will this still be true 3 sessions from now?" Yes → CONTEXT.md. No → SESSION-HANDOFF.md.

### 4. `SESSION-HANDOFF.md` — "What we just did + what's next"

**Audience:** AI tools (context recovery after compaction or session restart).
**Updates:** Every session end (Update workflow).
**Length:** 50-200 lines. If exceeds 200, rotate older content out.
**Contains:**
- Current State (TL;DR) — 2-4 sentences
- Last Session — What Shipped (5-15 bullets)
- Next Session — Execution Plan (specific: paths, function names, exact steps)
- Active Blockers (waiting on external action)
- Active Gotchas (session-specific hazards, NOT persistent ones)
- Debugging Playbook (if X breaks again) — proven recipes from prior debugging
- Asks the docs could have answered — questions the agent had to ask the user that should have been findable in AGENTS.md / REFERENCES.md / a SOT file (drives doc improvement over time)

**Rotation rule:** when SESSION-HANDOFF exceeds 200 lines or old completed detail stops helping the next session, move completed work to `docs/history/<YYYY-MM-DD>-session-<NN>.md` and update `docs/history/README.md`.

### 5. `ROADMAP.md` — "What we NEED to do"

**Audience:** Humans + AI.
**Updates:** Every session that closes/opens a roadmap item.
**Contains:**
- Top: red (blockers) / yellow (recommended) / green (nice-to-haves) for current version
- Middle: vNext backlog
- Future: vN+1
- Completed (dated)
- Decision Log (date | decision | alternatives | why we picked it)
- Backlog / Ideas (unscoped)
- Iceboxed (tracked but explicitly not active)

### 6. `docs/` — "Everything else doc-related"

**Layout:**

```
docs/
├── DOCS-INDEX.md                     # navigation aid (REQUIRED for >10 docs)
├── history/                  # completed work, old handoffs, retired roadmap detail
│   ├── README.md                     # session log index
│   └── YYYY-MM-DD-session-NN.md
├── reference/                        # verified-state SOT files
├── playbooks/                        # process runbooks
├── dev/                              # architecture / feature design
├── release/                          # release notes
├── reviews/                          # code review reports
├── research/                         # experiments + exploration drops
├── _archive/                         # historical (dated batches)
└── _private/                         # gitignored sensitive content
```

**Standard subfolders (created on demand):**

| Subfolder | Holds | Created when |
|---|---|---|
| `history/` | Completed work, old handoffs, retired roadmap detail | First real history item to preserve |
| `reference/` | Verified-state SOT files (config truths, capability matrices) | First incident where summary went stale |
| `playbooks/` | Process runbooks ("when X happens, do Y") | First reusable process |
| `dev/` | Architecture docs, schema docs, feature design | Internal architecture worth documenting |
| `release/` | Release notes, migration steps | First versioned release |
| `reviews/` | Code review reports, security audits | First external review |
| `research/` | Experiments, exploration | First research drop |
| `_archive/` | Historical material | First archival action |
| `_private/` | Gitignored sensitive content | First sensitive drop |

#### `docs/DOCS-INDEX.md` — the navigation aid

**Required for:** any project with >10 docs in `docs/`. OMIT for greenfield.
**Contains:** tree view of `docs/`, per-subfolder description, per-file row in each subfolder (filename + 1-line purpose), "Recently added" + "Recently archived" sections.

**REFERENCES.md vs DOCS-INDEX.md (key distinction):**
- REFERENCES.md (root) — TOPIC-driven ("where do I look up SSO config?")
- DOCS-INDEX.md (in docs/) — LOCATION-driven ("what files exist in `docs/dev/`?")

#### `docs/history/` — completed-work history

**Filename:** `YYYY-MM-DD-session-NN.md`
**Length:** 50-150 lines per file
**Contains:** date + session number or topic, one-line summary, what was completed, what was committed (commit hashes if available), what was deferred, key gotchas surfaced, and links back to current docs if relevant.
**File integrity rule:** files don't get edited post-write except for link repair or sensitive-data redaction. Record those exceptions in the index README.

#### `docs/_archive/` and `docs/_private/`

- Archive batches use dated folders: `_archive/YYYY-MM-DD-<description>/` with an `ARCHIVE-README.md`
- Private folder is for gitignored business docs, internal lists, sensitive notes
- Both live INSIDE `docs/` (legacy `zzz-archive/` and `zzz-private/` at root get migrated by Audit)

### 7. `REFERENCES.md` — "Where do I look stuff up?"

**Audience:** AI tools (mostly).
**Updates:** Rare. New external URL or doc path added.
**Length:** 80-150 lines.
**Contains:**
- Source-of-truth files (SOT) — table for topics where summaries have drifted
- Playbooks index — table indexing every `docs/playbooks/<topic>-playbook.md`
- Gotchas & API immutables — fields that can't be PATCHed, actions that don't cascade, env vars that need `.trim()`, refresh-token rotation rules
- Project documentation — table mapping every doc in `docs/` to its purpose
- External docs (URLs for SDK/framework docs)
- External infrastructure (IDs only, never secrets)
- Naming conventions
- Capability ecosystem and tool-specific adapters (if applicable)

**Does NOT contain:** persistent rules (those go in AGENTS.md), current state (CONTEXT.md), anything that duplicates AGENTS.md.

---

## The routing tree

```
README.md  ─────────────►  Visitors arrive here, then flow inward

AGENTS.md  ─────────────►  AI session bootstrap (Hard Rules + folder map + routing)
   │
   ├──►  CONTEXT.md            "What we ARE doing" (current phase)
   │       │
   │       ├──►  SESSION-HANDOFF.md   "What we just did + next"
   │       │        │
   │       │        └──►  docs/history/    "What we completed" (old detail)
   │       │
   │       └──►  ROADMAP.md            "What we NEED to do" (priorities + Decision Log)
   │               │
   │               └──►  docs/release/, docs/dev/, etc.   (details)
   │
   └──►  REFERENCES.md          "Where to look it up" (topic-driven)
            │
            ├──►  docs/reference/      (verified-state SOT files; "Trust this doc")
            ├──►  docs/playbooks/      (process runbooks)
            └──►  external URLs / library docs

docs/DOCS-INDEX.md  ────►  Navigation aid: "what files exist in docs/?" (location-driven)
```

---

## The 5 rules that make this work

1. **Each file has ONE job.** If you're tempted to add a "Quick Reference" section to ROADMAP.md, don't — that goes in REFERENCES.md.

2. **Concise top, details below.** Top-level files SUMMARIZE and POINT. Implementation details live in `docs/release/`, `docs/dev/`, `docs/reviews/`, etc.

3. **Time-scale discipline.** Every line answers "will this still be true in 3 sessions?"
   - Yes (persistent) → AGENTS.md, REFERENCES.md, CONTEXT.md (phase-stable)
   - No (volatile) → SESSION-HANDOFF.md

4. **History is preserved, not deleted.** Completed work, old handoffs, and retired roadmap detail move to `docs/history/`. ROADMAP.md keeps concise current priorities plus dated completed summaries. Files don't get truncated mid-session.

5. **No file points "inward" past one layer.** AGENTS.md points to CONTEXT.md, but not to a specific `docs/release/` file. The tree stays shallow and predictable.

## Hardened Lessons From Ratchet Runs

These rules came from duplicate-only cleanup tests across different project shapes:

- **Private paths are not context sources for public docs.** Current docs may name where private material lives, but they should not depend on private files to be understandable.
- **Product/content Markdown is not automatically operating-doc clutter.** Marketing copy, email sequences, blog drafts, and embedded app content should be scored separately unless the project defines them as operating docs.
- **Archive moves must preserve links.** If a snapshot moves into `_archive/`, repair relative links inside the snapshot or record why they are intentionally stale.
- **Scoped duplicate tests need source-aware link checks.** Missing links to intentionally omitted non-private code files are fixture gaps. Links to protected/private files are still failures.
- **History is a compression tool, not a dumping ground.** Current docs should point to history only when a future agent would reasonably need the completed detail.

---

## The agent's mental model

When an AI agent picks up a repo cold, it reads in this order:

1. **AGENTS.md** — identity + rules + folder map (always)
2. **CONTEXT.md** — current phase (every session)
3. **SESSION-HANDOFF.md** — what just happened (every session)
4. **ROADMAP.md** — if picking up a planned task, find it here
5. **REFERENCES.md** — when looking something up by topic
6. **`docs/DOCS-INDEX.md`** — when looking up by file location
7. **`docs/history/`** — only when investigating "when did X change?" or "what happened to that completed detail?"

Six files cover 95% of agent needs. README is for humans visiting the repo.

---

## Stress test: where does each kind of content go?

| Content | File |
|---------|------|
| "We use library X v2.3" | AGENTS.md (tech stack) + README.md (public version) |
| "Current focus: ship v2.0 to production" | CONTEXT.md |
| "Last session: doc cleanup. Next: ship feature X." | SESSION-HANDOFF.md |
| "Feature X + Feature Y + bugfix Z" (priority list) | ROADMAP.md |
| "Bundle ID header required in 7 places or 403s" | AGENTS.md (gotchas) |
| "Library X official docs URL" | REFERENCES.md |
| "Session 30 fixed F1+F2 in DataLoader" | docs/history/2026-05-01-session-30.md |
| "Detailed v2.0 deep code review handoff" | docs/reviews/v2.0-deep-code-review.md |
| "API key for service Y is in vault path Z" | REFERENCES.md (path/location only, never values) |
| "What's the v2.1 metrics plan?" | ROADMAP.md → links to docs/dev/v2.1-metrics-plan.md |
| "Why we picked thinkingBudget=1024" | docs/dev/thinking-budget-decision.md (detail), pointed to from AGENTS.md gotchas + REFERENCES.md |
| "Naming convention for HealthKit metric IDs" | REFERENCES.md |

Every piece of content has exactly ONE home.

---

## See also

- `naming-conventions.md` — full naming spec + detection signatures
- `anti-patterns.md` — A1-A12 drift catalog
- `canonical-file-set.md` — the 9 canonical surfaces + decision tree for new files
- `doc-ratchet.md` — duplicate-only improvement loop
- `workflow-autoresearch.md` — workflow hardening loop
- `workflow-score.md` — scoring rubric for the reusable workflow itself
- `templates/` — drop-in templates for the 6 root files
