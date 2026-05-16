---
type: reference
status: active
owner: knowledge-ops
last_reviewed: 2026-05-15
applies_to: anchor-point Audit + Review modes
description: Rationale catalog for the doc-architecture drift codes (A1-A13, MG1-MG4). For deterministic detection grep patterns, see drift-checks.md. This file explains WHY each code matters and what real-world incidents drove it into the catalog.
---

# Anti-Patterns — Doc Architecture Drift Catalog

Drift signatures that indicate the doc architecture has slipped. Each entry has a rationale and a fix. The deterministic detection grep/scan patterns live in `drift-checks.md`. Anchor Point uses both in Review (cheap detection) and Audit (deep scoring + fixing).

Codes prefixed `A` are content/structure anti-patterns. Codes prefixed `MG` are migration-grade issues that typically require structural moves.

---

## A1. Duplicate rules in multiple root docs

**Signature:** A "Hard rules" / "Important Notes" / "Behavioral reminders" section appears verbatim or nearly-verbatim in two root files (commonly AGENTS.md and LOOKUP.md/REFERENCES.md).

**Why it matters:** Two homes for rules means agents may follow different versions; updates drift.

**Fix:** Keep in AGENTS.md only. Replace duplicates with `## See: AGENTS.md "Project Rules"` pointer or delete entirely. The pointer form is acceptable; verbatim duplication is not.

---

## A2. Architecture hiding in STATUS.md / SESSION-HANDOFF.md

**Signature:** STATUS.md (or legacy SESSION-HANDOFF.md) contains H2/H3 sections like "X Feature Architecture", "Y Routing Flow", "Z Database Schema" — content that is stable, not session-volatile.

**Why it happens:** Engineer wrote architecture during a session, dropped it in the handoff doc, never moved it.

**Fix:** Move to `docs/dev/<feature>.md`. Replace STATUS section with one-line pointer.

---

## A3. Session-volatile content in CONTEXT.md (legacy v1.x signal)

**Signature:** CONTEXT.md exists at project root with phase-level content.

**Why it matters:** v3.0 absorbed CONTEXT.md into AGENTS.md "Current Phase" section. A standalone CONTEXT.md indicates the project is on the v1.x shape and has not migrated.

**Fix:** Migrate CONTEXT.md content to AGENTS.md "Current Phase" section. Delete CONTEXT.md.

---

## A4. STATUS.md over 200 lines

**Signature:** Live handoff has accumulated multiple session-log entries beyond the latest delta.

**Why it matters:** STATUS.md is a HOT file loaded every session. Length inflation costs tokens on every cold start.

**Fix:** Rotate sessions older than the latest delta to `docs/status-history/<YYYY-MM-DD>-NN.md`. Update the history README.

**Soft warning:** at >150 lines, suggest rotation. **Hard fix:** at >200 lines, rotate.

---

## A5. Stale tech-stack claims

**Signature:** AGENTS.md Tech Stack table lists a version number that doesn't match the manifest (`package.json`, `Cargo.toml`, `pyproject.toml`, etc.).

**Why it matters:** Docs are downstream of code. A drifted version claim corrupts every downstream decision.

**Fix:** Update AGENTS.md Tech Stack from manifest grep. Add a Source column citing the manifest path so future audits can re-verify.

---

## A6. Pointer-only root docs

**Signature:** A root file is 100% pointers to other docs with zero original content of its own.

**Likely culprits:** AGENTS.md missing rules, LOOKUP.md missing tables, README.md skipping the summary.

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

**Signature:** Two or more root docs (STATUS, AGENTS, README, LOOKUP) have a section with a priority-like header (`Priorities`, `Top N`, `Next Steps`, `Active Work`, `In Progress`, `Upcoming`, `Backlog`, `Next Session`) AND those sections enumerate forward-looking items rather than just pointing to ROADMAP.

**Why it matters:** ROADMAP is the single owner of priorities. Parallel lists drift; agents read one and act on stale context.

**Fix:** Keep one section pointing to ROADMAP top 3. Reduce other instances to a 1-line pointer.

---

## A10. LOOKUP.md duplicating DOCS-INDEX inventory

**Signature:** LOOKUP.md (or REFERENCES.md) has a section titled "Inventory", "File Map", "Documentation Index", "docs/ tree", or contains a table with rows that are mostly file paths under `docs/`.

**Why it matters:** LOOKUP.md owns TOPIC-driven lookup (SOT registry by topic, playbook index by topic). DOCS-INDEX.md owns LOCATION-driven inventory. Mixing them creates two homes for the file map.

**Fix:** LOOKUP owns topic lookup; DOCS-INDEX owns file inventory. Strip file-inventory rows from LOOKUP, leave only topic SOT and playbook tables.

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

**Signature:** STATUS.md or SESSION-HANDOFF.md (or ROADMAP.md "Open Questions" section) contains an "Asks" / "Open Questions" / "Decisions Needed" / "Questions for User" / "Pending Decisions" / "Need Input" section with entries that lack routing tags like `[AGENTS.md: notes]`, `[STATUS.md: notes]`, etc.

**Why it matters:** Untagged Asks defer routing decisions to handoff time. doc-handoff (Mode 3) becomes a judgment-heavy procedure instead of an idempotent pure function.

**Fix:** Add `[DEST.md: notes]` pre-route convention to your STATUS template. Migrate existing Asks to tagged form before next handoff. The agent that writes the Ask owns the routing decision; doc-handoff just executes the move.

---

## A13. doc-handoff output drifts when re-run on same inputs (NEW in v3.0)

**Signature:** Running Mode 3 (Update) or Mode 6 (Audit) twice on identical inputs produces different output (excluding frontmatter timestamps and run UUIDs).

**Why it matters:** Idempotency is the contract that makes the skill safe to automate. Drift means hidden judgment calls or non-deterministic ordering.

**Fix:** Apply the deterministic-output rule (sort findings by code then by file path; integer-only scores; ISO-8601 timestamps only in frontmatter `run-date`). Two runs on identical fixture inputs MUST produce byte-equivalent output modulo frontmatter timestamps and run UUIDs.

---

## MG1. Active doc routes through `_private/` (migration-grade)

**Signature:** AGENTS.md, LOOKUP.md, STATUS.md, or README.md contains a markdown link to a path under `docs/_private/`.

**Why it matters:** Public/current docs become impossible to use in a repoed or shared context. Agents may try to read sensitive material unnecessarily. The private content may also be excluded from cloning, causing broken navigation.

**Fix:** Remove the pointer from active routing. If the target is needed for cross-reference, move the pointer to a separate "Sensitive SOT register" table in LOOKUP.md flagged "private; do not route as primary navigation".

---

## MG2. ROADMAP > 300 lines without extracted decisions or rotated history

**Signature:** ROADMAP.md exceeds 300 lines AND either `docs/decisions/` is empty/absent OR `docs/roadmap-history/` is empty/absent.

**Why it matters:** ROADMAP should help pick the next priority. When completed work AND decision rationale both pile up inline, the file becomes a completed-work archive instead of a forward-looking priority doc.

**Fix:** Two-part:
- (a) Extract Decision Log entries to `docs/decisions/YYYY-MM-DD-<topic>.md` (one file per decision). Replace inline Decision Log with a 1-line pointer.
- (b) Rotate completed-priority sections older than the visible window to `docs/roadmap-history/YYYY-MM-DD-NN.md`. Keep the live ROADMAP focused on red/yellow/green for the current and next versions.

`docs/roadmap-history/` is the parallel of `docs/status-history/` for ROADMAP rotation: same dated-file convention, same README documenting the rotation policy.

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

## Severity + scoring

When Audit scores doc health (100-point system per the rubric in `SKILL.md`), each anti-pattern affects different dimensions:

| Anti-pattern | Primary impact |
|---|---|
| A1 (duplicate rules) | Drift / Routing accuracy |
| A2 (architecture in STATUS) | Concision / Scope discipline |
| A3 (CONTEXT.md still present) | Migration debt |
| A4 (STATUS > 200 lines) | Concision / Token cost |
| A5 (stale tech-stack) | Currency / Anti-fabrication |
| A6 (pointer-only file) | AI-actionability / Specificity |
| A7 (detail in top-level) | Concision / Scope discipline |
| A8 (hypothesis as fact) | Anti-fabrication / Trustworthiness |
| A9 (parallel priority lists) | Drift / Single-owner discipline |
| A10 (LOOKUP duplicates DOCS-INDEX) | Routing / Findability |
| A11 (Layer 0 duplicated) | Maintenance / Drift |
| A12 (Asks without routing tags) | Handoff hygiene / Idempotency |
| A13 (handoff non-idempotent) | Automation readiness |
| MG1 (active → _private) | Security / Findability |
| MG2 (ROADMAP overflow) | Concision / History preservation |
| MG3 (broken links) | Findability |
| MG4 (split authority) | Drift / Routing accuracy |

---

## How Init pre-empts these

When generating the initial 5-file v3.0 doc set, Anchor Point AVOIDS creating these patterns from day one:

- **A1, A11:** Hard Rules baseline lives at <your Layer 0 home> (e.g., `+vantage-point/AGENTS.md` in a vantage-point monorepo); per-project AGENTS.md uses `## Inherits from` pointer
- **A2, A4:** STATUS.md skeleton is bounded (TL;DR + Last shipped + Next + Blockers + Debugging Playbook only)
- **A3:** v3.0 never creates CONTEXT.md; "Current Phase" lives inside AGENTS.md
- **A5:** Tech-stack table only includes versions confirmed by reading manifest files; Source column cited
- **A6:** Each root file has summary content, not just pointers
- **A7:** ROADMAP items are concise summaries; detail routes to `docs/release/` or `docs/dev/`
- **A8:** Generated content cites sources for any cause-effect claim
- **A9:** Only ROADMAP.md owns priority lists; STATUS.md "Next session" points to ROADMAP top 3
- **A10:** LOOKUP.md is topic-driven; DOCS-INDEX.md is location-driven; never overlap
- **A12:** STATUS template includes the `[DEST.md: notes]` pre-route convention by default
- **A13:** Mode 3 + Mode 6 outputs are idempotent by design (sort, integer scores, frontmatter-only timestamps)
- **MG1:** Init never writes links from root docs into `docs/_private/`
- **MG2:** Init creates `docs/decisions/` and `docs/roadmap-history/` as part of the standard 9-folder bootstrap
- **MG3:** Init validates relative links from generated content
- **MG4:** Init creates AGENTS.md as canonical and CLAUDE.md as 1-line stub (when needed for tool compatibility)
