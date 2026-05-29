---
name: anchor-point-mode-2-review
type: reference
parent-skill: anchor-point
mode: 2
last_reviewed: 2026-05-24
---

# Mode 2, Review

Loaded by the router at [SKILL.md](../SKILL.md). Mode 2 of 7. Related modes: [Mode 1, Init](mode-1-init.md), [Mode 3, Update](mode-3-update.md), [Mode 4, Audit](mode-4-audit.md).

Use at session start or when the user asks for status.

## Workflow

1. Read `AGENTS.md` or `CLAUDE.md`, whichever is present and authoritative.
2. Read `SESSION-HANDOFF.md` (or `STATUS.md` if v3), `ROADMAP.md`, `REFERENCES.md` (or `LOOKUP.md` if v3), and `README.md` when present. Also `CONTEXT.md` if it exists as a v1.x holdover.
3. Skim `docs/DOCS-INDEX.md` if present.
4. Run a cheap drift check.
5. Use `components/snapshot.md` to summarize current state, active work, blockers, and suggested next actions.
6. Use `components/verify.md` only for claims that affect the next action.

## Rules

- Read-only. Do not write files.
- Do not perform structural cleanup.
- If drift exists, recommend Update or Audit; do not auto-fix.
- **Be FAST** — under 60 seconds end-to-end.
- **Be CONCISE** — output should be scannable in 10 seconds.
- **Be HONEST** — if you can't determine something, say so. Don't guess.

## Component Chain

`snapshot → verify only for high-impact claims`
