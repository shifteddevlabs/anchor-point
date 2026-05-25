---
name: anchor-point-mode-4-audit
type: reference
parent-skill: anchor-point
mode: 4
last_reviewed: 2026-05-24
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

- Approval required before structure changes.
- Never delete, archive instead.
- Do not move secret-bearing files without a private migration plan.
- Execute changes atomically.

## Component Chain

`security-preflight → snapshot → verify → promote-memory → harvest if reusable material is found`
