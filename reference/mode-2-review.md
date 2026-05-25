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
2. Read `CONTEXT.md`, `STATUS.md`, `ROADMAP.md`, `LOOKUP.md`, and `README.md` when present.
3. Skim `docs/DOCS-INDEX.md` if present.
4. Run a cheap drift check.
5. Use `components/snapshot.md` to summarize current state, active work, blockers, and suggested next actions.
6. Use `components/verify.md` only for claims that affect the next action.

## Rules

- Read-only.
- Do not write files.
- Do not perform structural cleanup.
- If drift exists, recommend Update or Audit.

## Component Chain

`snapshot → verify only for high-impact claims`
