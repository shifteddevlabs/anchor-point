---
name: anchor-point-mode-4-audit
type: reference
parent-skill: anchor-point
mode: 4
last_reviewed: 2026-06-26
---

# Mode 4, Audit

Loaded by the router at [SKILL.md](../SKILL.md). Mode 4 of 7. Related modes: [Mode 2, Review](mode-2-review.md), [Mode 3, Update](mode-3-update.md), [Mode 6, Project Ratchet](mode-6-project-ratchet.md).

Use when the user asks to clean up, reorganize, or check doc health.

The Audit Scoring Rubric (Hard Gates + 100-point categories + health bands) lives in the router at [SKILL.md](../SKILL.md). Drift detection table lives in [drift-checks.md](drift-checks.md). Anti-pattern rationale lives in [anti-patterns.md](anti-patterns.md).

## Workflow

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

## Rules

- **Always present plan before executing. No exceptions.**
- **Atomic per-finding approval.** Never bulk-rewrite multiple files in one operation. Every move/rename/merge surfaces as its own finding awaiting user OK.
- Never delete, archive instead.
- Do not move secret-bearing files without a private migration plan.
- **Never modify history files post-write** except for link repair or sensitive-data redaction.
- Execute changes atomically per finding.
- **Always update DOCS-INDEX.md and AGENTS.md folder map** after structural changes.
- **Always update cross-references to prevent link rot.**

## Auto-invocation safety

Mode 4 is structural (rename, relocate, merge, archive) and may take 30-60 minutes. Auto-invocation is acceptable because the per-finding approval gate (Rules above) prevents accidental writes — every move/rename/merge surfaces in a plan and waits for explicit user OK before executing.

Host adapters MAY add `disable-model-invocation: true` in their frontmatter if they want manual-only invocation in a specific environment. Adapters that auto-invoke must clearly document that the approval gate is the safety mechanism.

## Component Chain

`security-preflight → snapshot → verify → promote-memory → harvest if reusable material is found`

## Field-proven execution notes (AI Health Export, Session 35)

> Promoted 2026-06-26 from the transcript-vault learnings lake (prop_e74bb33729e9be2d).

- **Re-verify the plan against the live filesystem before executing.** An audit plan drafted from a stale snapshot routinely misreports file counts, staleness, subfolder structure, and noise (stray `.DS_Store`, etc.). Five minutes re-checking the plan's own claims against ground truth caught 4 inaccuracies before any file moved. This is distinct from the step-1 security-preflight (a credential scan) and the step-4 content verify (doc claims) — it is a ground-truth check on the *plan's own* accuracy.
- **MOVES before RENAMES when decomposing structural changes into commits/PRs.** Git rename-detection only tracks a file already at its final path before the name changes; interleaving a move and a rename in one commit defeats detection and loses history. Order the work: relocate first (own commit), rename second. The same applies when a child rename depends on a parent-folder move — land the parent move first.
