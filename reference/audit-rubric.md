---
name: anchor-point-audit-rubric
type: reference
parent-skill: anchor-point
last_reviewed: 2026-07-19
---

# Audit Scoring Rubric + Doc Placement

Loaded by the router at [SKILL.md](../SKILL.md). Load for Audit (Mode 4); Mode 6 reuses this rubric for baselines and Mode 7 scores the workflow alongside it. Not needed on the Review (Mode 2) or Update (Mode 3) paths.

## Audit Scoring Rubric

Use this 100-point rubric for doc-audit and test runs. Apply hard gates first, then score categories. Mode 4 owns this rubric; Mode 6 reuses it for baselines; Mode 7 scores the workflow alongside it.

### Hard Gates

These caps make the score harder to game:

| Condition | Score Cap |
|---|---:|
| Strict secret-pattern hit in active docs | 69 |
| Sensitive value appears in historical docs without redaction plan | 79 |
| `SESSION-HANDOFF.md` over 200 lines | 84 |
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
| Rotated SESSION-HANDOFF.md content | `docs/handoff-history/` | `YYYY-MM-DD-NN.md` |
| Rotated ROADMAP.md content | `docs/roadmap-history/` | `YYYY-MM-DD-NN.md` |
| Doc-system test reports | Shared `docs/test-runs/` location | `YYYY-MM-DD-<run>.md` |
| Sensitive docs | `docs/_private/` | `lowercase-kebab.md` |
| Historical/archive-only docs | `docs/_archive/` | `YYYY-MM-DD-batch/` |
