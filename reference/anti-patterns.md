---
type: reference
status: active
owner: knowledge-ops
last_reviewed: 2026-05-16
applies_to: anchor-point Audit + Review modes
description: Rationale catalog for the doc-architecture drift codes (A1-A19, MG1-MG4). For deterministic detection grep patterns, see drift-checks.md. This file explains WHY each code matters and what real-world incidents drove it into the catalog.
---

# Anti-Patterns — Doc Architecture Drift Catalog

Drift signatures that indicate the doc architecture has slipped. Each entry has a rationale and a fix. The deterministic detection grep/scan patterns live in `drift-checks.md`. Anchor Point uses both in Review (cheap detection) and Audit (deep scoring + fixing).

Codes prefixed `A` are content/structure anti-patterns. Codes prefixed `MG` are migration-grade issues that typically require structural moves.

---

## A1. Duplicate rules in multiple root docs

**Signature:** A "Hard rules" / "Important Notes" / "Behavioral reminders" section appears verbatim or nearly-verbatim in two root files (commonly AGENTS.md and REFERENCES.md, or legacy LOOKUP.md).

**Why it matters:** Two homes for rules means agents may follow different versions; updates drift.

**Fix:** Keep in AGENTS.md only. Replace duplicates with `## See: AGENTS.md "Project Rules"` pointer or delete entirely. The pointer form is acceptable; verbatim duplication is not.

---

## A2. Architecture hiding in SESSION-HANDOFF.md

**Signature:** SESSION-HANDOFF.md (or legacy STATUS.md) contains H2/H3 sections like "X Feature Architecture", "Y Routing Flow", "Z Database Schema" — content that is stable, not session-volatile.

**Why it happens:** Engineer wrote architecture during a session, dropped it in the handoff doc, never moved it.

**Fix:** Move to `docs/dev/<feature>.md`. Replace SESSION-HANDOFF section with one-line pointer.

---

## A3. Session-volatile content in CONTEXT.md (legacy v1.x signal)

**Signature:** CONTEXT.md exists at project root with phase-level content.

**Why it matters:** v3.0 absorbed CONTEXT.md into AGENTS.md "Current Phase" section. A standalone CONTEXT.md indicates the project is on the v1.x shape and has not migrated.

**Fix:** Migrate CONTEXT.md content to AGENTS.md "Current Phase" section. Delete CONTEXT.md.

---

## A4. SESSION-HANDOFF.md over 200 lines

**Signature:** Live handoff has accumulated multiple session-log entries beyond the latest delta.

**Why it matters:** SESSION-HANDOFF.md is a HOT file loaded every session. Length inflation costs tokens on every cold start.

**Fix:** Rotate sessions older than the latest delta to `docs/handoff-history/<YYYY-MM-DD>-NN.md`. Update the history README.

**Soft warning:** at >150 lines, suggest rotation. **Hard fix:** at >200 lines, rotate.

---

## A5. Stale tech-stack claims

**Signature:** AGENTS.md Tech Stack table lists a version number that doesn't match the manifest (`package.json`, `Cargo.toml`, `pyproject.toml`, etc.).

**Why it matters:** Docs are downstream of code. A drifted version claim corrupts every downstream decision.

**Fix:** Update AGENTS.md Tech Stack from manifest grep. Add a Source column citing the manifest path so future audits can re-verify.

---

## A6. Pointer-only root docs

**Signature:** A root file is 100% pointers to other docs with zero original content of its own.

**Likely culprits:** AGENTS.md missing rules, REFERENCES.md missing tables, README.md skipping the summary.

**Fix:** Add the missing summary content (≥ 100 words) before the pointer block. Top-level files SUMMARIZE before pointing. A pointer-only file means the summary work was skipped.

---

## A7. Detail leakage into top-level docs

**Signature:** A root file (or a docs/playbooks/ doc) has a section with > 50 lines, a fenced code block, an architecture diagram, or a file-tree listing.

**Why it matters:** Top-level files are summaries + pointers. Implementation detail belongs in `docs/dev/` (architecture), `docs/release/` (version-specific), or playbooks (process).

**Fix:** Extract detail to the right `docs/<bucket>/` location. Replace inline detail with one-line pointer.

---

## A8. Hypothesis stated as fact

**Signature:** A doc states X causes Y when only correlation has been observed. Look for present-tense assertive verbs (`is`, `uses`, `supports`, `runs`, `handles`) about runtime state without a Source citation OR a `Hypothesis:` / `Verified by:` annotation.

**Canonical incident:** A team noticed Facebook posts published from their app got 0-9 reach while manual posts got 500+. They claimed "Dev mode silently throttles reach" as fact in their docs. Meta's docs do not confirm it. Burned 4 hours debugging the metrics parser when the metrics were correct and the cause was unproven.

**Fix:** Wrap claim in `**Hypothesis:**` prefix; add `Verify by: <command/check>` line citing the manifest, dashboard, or test that confirms the claim. Promote to fact only after the test runs.

---

## A9. Parallel priority / next-action lists across docs

**Signature:** Two or more root docs (SESSION-HANDOFF, AGENTS, README, REFERENCES) have a section with a priority-like header (`Priorities`, `Top N`, `Next Steps`, `Active Work`, `In Progress`, `Upcoming`, `Backlog`, `Next Session`) AND those sections enumerate forward-looking items rather than just pointing to ROADMAP.

**Why it matters:** ROADMAP is the single owner of priorities. Parallel lists drift; agents read one and act on stale context.

**Fix:** Keep one section pointing to ROADMAP top 3. Reduce other instances to a 1-line pointer.

---

## A10. REFERENCES.md duplicating DOCS-INDEX inventory

**Signature:** REFERENCES.md (or legacy LOOKUP.md) has a section titled "Inventory", "File Map", "Documentation Index", "docs/ tree", or contains a table with rows that are mostly file paths under `docs/`.

**Why it matters:** REFERENCES.md owns TOPIC-driven lookup (SOT registry by topic, playbook index by topic). DOCS-INDEX.md owns LOCATION-driven inventory. Mixing them creates two homes for the file map.

**Fix:** REFERENCES owns topic lookup; DOCS-INDEX owns file inventory. Strip file-inventory rows from REFERENCES, leave only topic SOT and playbook tables.

---

## A11. Layer 0 Hard Rules pasted into project AGENTS.md (NEW in v3.0)

**Signature:** Project AGENTS.md contains a "Hard Rules" / "inviolable rules" / "universal rules" section AND lacks an `## Inherits from` pointer.

**Why it matters:** Layer 0 baseline rules are cross-project. Pasting them into every project means updates have to be propagated by hand; drift becomes inevitable.

**Fix:** Replace the duplicated baseline with:

```
## Inherits from

This project inherits the Hard Rules baseline + naming + routing conventions
from <Layer 0 home> (e.g., `+vantage-point/AGENTS.md` if using vantage-point pattern).
See that file before applying project-specific rules below.
```

---

## A12. Asks accumulated without routing tags (NEW in v3.0)

**Signature:** SESSION-HANDOFF.md or legacy STATUS.md (or ROADMAP.md "Open Questions" section) contains an "Asks" / "Open Questions" / "Decisions Needed" / "Questions for User" / "Pending Decisions" / "Need Input" section with entries that lack routing tags like `[AGENTS.md: notes]`, `[SESSION-HANDOFF.md: notes]`, etc.

**Why it matters:** Untagged Asks defer routing decisions to handoff time. doc-handoff (Mode 3) becomes a judgment-heavy procedure instead of an idempotent pure function.

**Fix:** Add `[DEST.md: notes]` pre-route convention to your SESSION-HANDOFF template. Migrate existing Asks to tagged form before next handoff. The agent that writes the Ask owns the routing decision; doc-handoff just executes the move.

---

## A13. doc-handoff output drifts when re-run on same inputs (NEW in v3.0)

**Signature:** Running Mode 3 (Update) or Mode 6 (Audit) twice on identical inputs produces different output (excluding frontmatter timestamps and run UUIDs).

**Why it matters:** Idempotency is the contract that makes the skill safe to automate. Drift means hidden judgment calls or non-deterministic ordering.

**Fix:** Apply the deterministic-output rule (sort findings by code then by file path; integer-only scores; ISO-8601 timestamps only in frontmatter `run-date`). Two runs on identical fixture inputs MUST produce byte-equivalent output modulo frontmatter timestamps and run UUIDs.

---

## MG1. Active doc routes through `_private/` (migration-grade)

**Signature:** AGENTS.md, REFERENCES.md, SESSION-HANDOFF.md, or README.md contains a markdown link to a path under `docs/_private/`.

**Why it matters:** Public/current docs become impossible to use in a repoed or shared context. Agents may try to read sensitive material unnecessarily. The private content may also be excluded from cloning, causing broken navigation.

**Fix:** Remove the pointer from active routing. If the target is needed for cross-reference, move the pointer to a separate "Sensitive SOT register" table in REFERENCES.md flagged "private; do not route as primary navigation".

---

## MG2. ROADMAP > 300 lines without extracted decisions or rotated history

**Signature:** ROADMAP.md exceeds 300 lines AND either `docs/decisions/` is empty/absent OR `docs/roadmap-history/` is empty/absent.

**Why it matters:** ROADMAP should help pick the next priority. When completed work AND decision rationale both pile up inline, the file becomes a completed-work archive instead of a forward-looking priority doc.

**Fix:** Two-part:
- (a) Extract Decision Log entries to `docs/decisions/YYYY-MM-DD-<topic>.md` (one file per decision). Replace inline Decision Log with a 1-line pointer.
- (b) Rotate completed-priority sections older than the visible window to `docs/roadmap-history/YYYY-MM-DD-NN.md`. Keep the live ROADMAP focused on red/yellow/green for the current and next versions.

`docs/roadmap-history/` is the parallel of `docs/handoff-history/` for ROADMAP rotation: same dated-file convention, same README documenting the rotation policy.

---

## MG3. Broken link in current/root docs

**Signature:** A relative markdown link in a root file or `docs/DOCS-INDEX.md` / `docs/playbooks/*` / `docs/reference/*` resolves to a nonexistent file.

**Why it matters:** Broken navigation breaks agents and humans equally; signals that recent moves were not reconciled.

**Precedence:** if the same link triggered MG1 (active doc → `_private/`), report only MG1; fixing MG1 implicitly resolves the broken-link defect by removing the pointer. MG3 fires only for broken links that are NOT MG1 cases.

**Fix:** Update link target. If target moved to history, repair relative path or mark as `(archived snapshot, see docs/_archive/...)`.

---

## MG4. Split authority between AGENTS.md and CLAUDE.md

**Signature:** Both AGENTS.md AND CLAUDE.md exist, both contain a "Rules" or "Hard Rules" heading, AND CLAUDE.md is > 5 lines (not a stub).

**Why it matters:** Models may follow different rules depending on which file they read first. v3.0 declares AGENTS.md the canonical bootstrap; CLAUDE.md should be a 1-line stub pointing to AGENTS.md (kept only for Claude Code's expected filename).

**Fix:** Either: (a) declare AGENTS.md sole authority and reduce CLAUDE.md to 1-line vendor stub (`See AGENTS.md`), or (b) make CLAUDE.md a legacy bridge that says "See AGENTS.md for canonical rules". (a) is preferred.

---

## A14. Subfolder README lists files (duplicates DOCS-INDEX)

**Signature:** A subfolder README.md inside `docs/` (e.g., `docs/decisions/README.md`, `docs/dev/README.md`) contains a table listing every file in that folder.

**Why it matters:** `docs/DOCS-INDEX.md` is the single owner of file inventory. Per-subfolder file tables duplicate that content, drift independently, and bloat token cost for an agent walking the tree. The subfolder README's actual job is to define purpose and "what goes here" — the inventory belongs at the index level.

**Fix:** Strip the file list. Keep three short sections only: Purpose (1 line), What goes here (3-5 bullets), Out of scope (1-2 bullets). If the subfolder has fewer than 3 files and an obvious purpose, delete the README entirely — DOCS-INDEX.md is sufficient.

**Real example:** `smg-ep/docs/decisions/README.md` (2026-05-16) contained a table of all 5 decision files plus a "How to add a decision" recipe. The recipe is fine; the file table duplicates `docs/DOCS-INDEX.md` lines 96-104.

---

## A15. CONTEXT.md used as folder documentation in a human-visible folder

**Signature:** A folder browsable by humans (or rendered by GitHub) contains a `CONTEXT.md` instead of (or in addition to) a `README.md`, with content that describes the folder's purpose rather than serving as agent-only working notes.

**Why it matters:** v3.0 explicitly removed CONTEXT.md from the root file set because it added a routing axis (agents asked "do I read README or CONTEXT?"). Reintroducing CONTEXT.md at subfolder level — even with the intent of "this is agent context" — recreates that friction. README.md is also more portable: GitHub renders it, humans expect it.

The narrow legitimate use of CONTEXT.md is agent-only working folders (e.g., `docs/_private/`, `scripts/` working notes) where a parent file (AGENTS.md, README.md) routes to it explicitly.

**Fix:** Rename CONTEXT.md → README.md and conform to A14's content rules. If the folder is genuinely agent-only AND a parent file routes to it, CONTEXT.md is acceptable.

---

## A16. Pattern/architecture content in AGENTS.md

**Signature:** AGENTS.md contains H2 sections like "Supabase Patterns", "API Route Patterns", "React Patterns", "TypeScript Rules" — pattern reference material that runs 30+ lines and describes HOW to write code in that area, not WHAT rules to follow.

**Why it matters:** AGENTS.md is hot-loaded every session. Pattern reference is warm content — the agent opens it when writing in that area, not before. Stuffing patterns into AGENTS.md inflates the always-loaded token cost without proportional value. It's also a maintenance hazard: pattern docs drift fast and benefit from sitting next to the code they describe.

The hot/warm test (Rule 4 in `doc-architecture.md`): does the agent need this in context to AVOID a mistake (hot), or will they QUERY it when relevant (warm)? Pattern reference is the canonical "warm" content.

**Fix:** Extract each pattern section to `docs/dev/<topic>.md`. Add a 1-line entry to AGENTS.md §3 Routing-by-task ("Supabase work → docs/dev/supabase-patterns.md").

**Real example:** `overdrive-lab/AGENTS.md` (2026-05-16) had 67 lines of Supabase Patterns + 101 lines of API Route Patterns + 54 lines of React Patterns inside AGENTS.md, contributing to a 552-line file (2.75x cap).

---

## A17. DOCS-INDEX.md has a Quick Start / task-routing table

**Signature:** `docs/DOCS-INDEX.md` opens with a "Quick Start" or "If you need X, read Y" table that routes by task type to specific files.

**Why it matters:** Task-driven routing is owned by AGENTS.md §3 (Routing-by-task). DOCS-INDEX.md answers "what files exist here?" — a location-driven question. Mixing the two means agents have two routing tables to maintain, and they drift independently.

This is the v3.2 analog of A10 (REFERENCES duplicating DOCS-INDEX). v3.2 closes the symmetrical hole: DOCS-INDEX must not duplicate AGENTS.md routing either.

**Fix:** Delete the Quick Start / routing table from DOCS-INDEX.md. Keep only file inventory (tree + per-subfolder tables of files with purposes). AGENTS.md §3 is the only task→file router.

**Real example:** `smg-ep/docs/DOCS-INDEX.md` lines 10-22 contained a "Quick Start" table that duplicated AGENTS.md §3 routing rows.

---

## A18. Project-level ROUTER.md split when thresholds not met

**Signature:** A project (not the workspace root) contains a `ROUTER.md` separate from `AGENTS.md`, when the project's routing-by-task content is small enough to fit comfortably in AGENTS.md §3.

**Why it matters:** Splitting routing into a separate file re-introduces the v1.x "look up which routing file to consult" intermediate hop that v3.0 specifically eliminated. The split costs more in load decisions than it saves in AGENTS.md line count.

Rule 5 thresholds for a justified split (ALL must be true):
- AGENTS.md exceeds the 200-line cap AND warm extraction is exhausted, AND
- Routing-by-task table exceeds ~25 rows, AND
- Project has ≥3 distinct task families with their own routing tables

Workspace root (`+vantage-point/`-style) is not automatically exempt — the same thresholds apply, scale just makes them easier to meet. See A19 for the more general "two hot files" anti-pattern that covers workspace-level cases.

**Fix:** Fold ROUTER.md back into AGENTS.md §3. See A19 for the general principle and the worked example.

---

## A19. Two hot files hold overlapping content at the same level

**Signature:** Two files at the same level (e.g., both at the project root, or both at the workspace root) hold overlapping hot content. The canonical example: `AGENTS.md` has a routing-by-task table AND a separate `ROUTER.md` has its own routing table. Variant: `AGENTS.md` has a Gotchas section AND a separate `gotchas.md` exists.

**Why it matters:** The foundational principle of v3.x is "ONE hot file." Hot content (rules, routing, gotchas — anything the agent needs in head at session start) belongs in AGENTS.md alone. Splitting hot content across two files:

- Forces the agent to load BOTH files at boot, increasing cold-start cost
- Forces a "which file owns this?" routing decision before the agent can act
- Creates a drift hazard — the same fact in two places diverges over time
- Re-introduces the v1.x multi-hop pattern (AGENTS → second hot file → warm target) that v3.0 specifically eliminated in favor of direct paths from AGENTS §3

A19 differs from A18 by being more general: A18 names ROUTER.md specifically and applies to project-level splits; A19 covers any two hot files at any level — including workspace-level cases like `+vantage-point/AGENTS.md` "Common Tasks Cheat Sheet" + `+vantage-point/ROUTER.md` Department Routing, or future variants like a separate `gotchas.md` or `rules.md`.

**Fix:** Identify which file is canonical (default: AGENTS.md). Collapse the overlapping hot content into it. Dedupe any rows that appear in both. Delete the non-canonical file entirely (no stubs — they add a hop without value). Update all live inbound pointers to point at AGENTS.md sections. Leave historical/snapshot files untouched.

**Worked example:** See `reference/doc-architecture.md` appendix "Worked example — collapsing two hot files (A19)" for the `+vantage-point/` ROUTER.md → AGENTS.md collapse executed 2026-05-16. Before: 2 hot files (92 + 76 lines), 3-hop boot pattern. After: 1 hot file (~165 lines), 2-hop boot pattern.

**Rule 5 check before invoking A19:** If the combined content would push AGENTS.md over the 200-line cap AND warm-content extraction is already exhausted AND the routing table itself is ≥25 rows AND there are ≥3 distinct task families, a split MAY be justified. In that narrow case A19 does not apply. For all other cases, collapse.

---

## A20. Inherits-from wording drift (v3.4)

**Signature:** Project AGENTS.md has a `## Inherits from` section, but the section body understates what's at the workspace AGENTS destination — e.g., a narrow 3-item list like "Hard Rules + naming + routing" instead of the canonical bulleted summary (Routing-by-task, Hard Rules baseline, Skills + Operating Agreements + Infrastructure maps, Connection-first rule, File naming conventions).

**Why it matters:** Agents that read a project's "Inherits from" pointer use the wording at the project level to decide whether to bother opening the workspace AGENTS.md. If the wording sounds narrow ("just rules and naming"), the agent may skip the re-read and miss the workspace's actual scope — routing tables, skills registry, credential-injection patterns, connection-first rule. The practical failure mode: an agent reads project AGENTS, doesn't re-read workspace AGENTS, then violates a workspace Hard Rule (e.g., raw `git push` falling through to keychain instead of using Infisical injection per Rule #12). This is the gap that motivated the v3.3.2 canonical template patch; v3.4 adds the audit-time enforcement so the wording can't silently drift back.

**Canonical incident:** The v3.3.2 patch (2026-05-16) strengthened the agents-template.md "Inherits from" section after recognizing that the prior 3-item phrasing didn't surface the workspace's actual scope. Existing v3.x-migrated projects (smg-ep, split-sheet, etc.) kept the prior wording until manually updated — no audit-time check existed to catch the drift. A20 is that check.

**Fix:** Replace the project's `## Inherits from` section body with the canonical wording from `reference/templates/agents-template.md` "Inherits from" block. Substitute the `{{LAYER_0_HOME}}` parameter with the project's actual Layer 0 path (e.g., `+vantage-point/` in a vantage-point monorepo). Preserve the conflict-resolution sentence ("Project-specific rules above OVERRIDE inherited defaults...").

**Detection severity (per `drift-checks.md`):**
- **Soft warning** (Mode 4 reports, doesn't block): bullet count is 3 OR keyword coverage is 4.
- **Hard fix** (Mode 4 proposes the rewrite as a structural finding): bullet count < 3 OR keyword coverage < 4. Hard fix is consistent with how A3 and A11 are scored (both also Inherits-from-related).

**Note on A11 vs A20:** A11 fires when "Inherits from" is MISSING and Hard Rules are pasted instead. A20 fires when "Inherits from" is PRESENT but its content is too narrow. The two are complementary — projects can drift from "no inheritance pointer at all" → "pointer with narrow wording" → "pointer with canonical wording" as they mature.

---

## A21. ROADMAP/status claims marked DONE without code verification

**Signature:** ROADMAP.md, SESSION-HANDOFF.md, or a plan doc marks an item "DONE", "CODE COMPLETE", or "shipped" without a Source citation into the code path that actually implements it.

**Why it matters:** A decision settled in a doc (a default, a sequencing plan, a copy/brand requirement) can silently never get wired into code, or only cover the happy path. Nothing catches the gap until a user hits it.

**Canonical incident:** overdrive-lab — a 3-parallel-agent plan-vs-reality audit found: (1) generated images defaulted to 1:1 despite the crop-arch doc setting master=4:5, because DB `platform_configs` gen-size keys were all NULL and a CODE fallback silently governed; (2) ROADMAP marked onboarding voice-research "CODE COMPLETE" but collect→analyze were decoupled fire-and-forget with no sequencing — analyze could run on an empty corpus and fail silently behind a swallowed `.catch` — corrected to "wired but fragile"; (3) Spark page placeholders/tips still hardcoded Jay-J/Shifted Music copy despite the roadmap saying brand-aware. Jay-J: "how did this get missed? then I wonder what else was missed... it's annoying."

**Fix:** Before trusting a DONE/CODE COMPLETE marker, open the code path it claims to cover and confirm it runs on the documented default/sequencing, not just the happy path. Add `done`, `complete`, `shipped`, `wired` to `components/verify.md`'s "Verify When" trigger words. Periodically run a plan-vs-reality audit (grep ROADMAP for DONE/COMPLETE markers, spot-check each against live code) instead of trusting the marker.

---

## Severity + scoring

When Audit scores doc health (100-point system per the rubric in `reference/audit-rubric.md`), each anti-pattern affects different dimensions:

| Anti-pattern | Primary impact |
|---|---|
| A1 (duplicate rules) | Drift / Routing accuracy |
| A2 (architecture in SESSION-HANDOFF) | Concision / Scope discipline |
| A3 (CONTEXT.md still present) | Migration debt |
| A4 (SESSION-HANDOFF > 200 lines) | Concision / Token cost |
| A5 (stale tech-stack) | Currency / Anti-fabrication |
| A6 (pointer-only file) | AI-actionability / Specificity |
| A7 (detail in top-level) | Concision / Scope discipline |
| A8 (hypothesis as fact) | Anti-fabrication / Trustworthiness |
| A9 (parallel priority lists) | Drift / Single-owner discipline |
| A10 (REFERENCES duplicates DOCS-INDEX) | Routing / Findability |
| A11 (Layer 0 duplicated) | Maintenance / Drift |
| A12 (Asks without routing tags) | Handoff hygiene / Idempotency |
| A13 (handoff non-idempotent) | Automation readiness |
| A14 (subfolder README lists files) | Single-owner discipline / Token cost |
| A15 (CONTEXT.md in human folder) | Routing axis / Portability |
| A16 (patterns in AGENTS.md) | Concision / Hot-vs-warm discipline |
| A17 (DOCS-INDEX has routing table) | Single-owner discipline / Drift |
| A18 (premature ROUTER.md split) | Routing accuracy / Load-decision overhead |
| A19 (two hot files overlap) | Drift / Hot-load discipline / Multi-hop boot |
| A20 (Inherits-from wording drift) | Maintenance / Drift / Cross-tool durability |
| A21 (DONE without code verification) | Anti-fabrication / Trustworthiness |
| MG1 (active → _private) | Security / Findability |
| MG2 (ROADMAP overflow) | Concision / History preservation |
| MG3 (broken links) | Findability |
| MG4 (split authority) | Drift / Routing accuracy |

---

## How Init pre-empts these

When generating the initial 5-file v3.0 doc set, Anchor Point AVOIDS creating these patterns from day one:

- **A1, A11:** Hard Rules baseline lives at <your Layer 0 home> (e.g., `+vantage-point/AGENTS.md` in a vantage-point monorepo); per-project AGENTS.md uses `## Inherits from` pointer
- **A2, A4:** SESSION-HANDOFF.md skeleton is bounded (TL;DR + Last shipped + Next + Blockers + Debugging Playbook only)
- **A3:** v3.0 never creates CONTEXT.md; "Current Phase" lives inside AGENTS.md
- **A5:** Tech-stack table only includes versions confirmed by reading manifest files; Source column cited
- **A6:** Each root file has summary content, not just pointers
- **A7:** ROADMAP items are concise summaries; detail routes to `docs/release/` or `docs/dev/`
- **A8:** Generated content cites sources for any cause-effect claim
- **A9:** Only ROADMAP.md owns priority lists; SESSION-HANDOFF.md "Next session" points to ROADMAP top 3
- **A10:** REFERENCES.md is topic-driven; DOCS-INDEX.md is location-driven; never overlap
- **A12:** SESSION-HANDOFF template includes the `[DEST.md: notes]` pre-route convention by default
- **A13:** Mode 3 + Mode 6 outputs are idempotent by design (sort, integer scores, frontmatter-only timestamps)
- **MG1:** Init never writes links from root docs into `docs/_private/`
- **MG2:** Init creates `docs/decisions/` and `docs/roadmap-history/` as part of the standard 9-folder bootstrap
- **MG3:** Init validates relative links from generated content
- **MG4:** Init creates AGENTS.md as canonical and CLAUDE.md as 1-line stub (when needed for tool compatibility)
- **A14:** Init writes subfolder READMEs with the three-section shape (Purpose / What goes here / Out of scope) only — no file tables
- **A15:** Init never writes CONTEXT.md in human-visible folders; reserved name for agent-only working folders
- **A16:** Init's AGENTS.md template has no pattern-reference sections; pattern docs are seeded as `docs/dev/` files when relevant
- **A17:** Init's DOCS-INDEX.md template has no Quick Start / routing table; routing lives only in AGENTS.md §3
- **A18:** Init never creates a project-level ROUTER.md
- **A19:** Init creates exactly ONE hot file per project (AGENTS.md). No parallel hot files for routing, rules, gotchas, or any other hot content. Audit Mode 4 scans for any file at the project or workspace root holding hot content that overlaps with AGENTS.md, and proposes collapse per the worked example in `reference/doc-architecture.md`.
- **A20:** Init writes the `## Inherits from` section using the canonical template wording (5-point summary covering routing, Hard Rules, skills + operating agreements + infrastructure, connection-first, file naming). Audit Mode 4 detects drift via bullet count + keyword coverage thresholds per `reference/drift-checks.md` and proposes a rewrite to the canonical wording when the section understates the workspace AGENTS scope.
- **A21:** ROADMAP/SESSION-HANDOFF templates require a Source citation before a DONE/CODE COMPLETE marker; Init's generated docs never assert completion without a code-path citation.
