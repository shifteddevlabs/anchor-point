---
name: anchor-point-mode-3-update
type: reference
parent-skill: anchor-point
mode: 3
last_reviewed: 2026-05-24
---

# Mode 3, Update

Loaded by the router at [SKILL.md](../SKILL.md). Mode 3 of 7. Related modes: [Mode 2, Review](mode-2-review.md), [Mode 4, Audit](mode-4-audit.md), [Mode 5, Inventory And Extract](mode-5-inventory-extract.md).

Use at session end or after meaningful work.

## Workflow

1. Identify what changed this session.
2. Use `components/snapshot.md` to capture the session delta.
3. Use `components/security-preflight.md` if docs were copied, moved, generated, or newly scanned.
4. Use `components/verify.md` for volatile or high-impact claims.
5. Use `components/promote-memory.md` to place durable learnings.
6. Use `components/harvest.md` only if reusable cross-project process appeared.
7. Update `SESSION-HANDOFF.md`.
8. Sync `ROADMAP.md` for completed and newly surfaced work.
9. Add decision rows when decisions were made.
10. Update `REFERENCES.md` when new source-of-truth or playbook docs appear.
11. Add persistent rules to `AGENTS.md` or project-local bootstrap docs only when a durable lesson emerged.
12. Rotate when needed: SESSION-HANDOFF.md content older than the latest delta → `docs/handoff-history/`; ROADMAP.md completed-priority entries beyond the visible window → `docs/roadmap-history/`. Decision Log rows → `docs/decisions/` (one file per decision).

## Rules

- Content-only.
- Do not move or rename files (that's Mode 4's job).
- Do not rewrite project history.
- Keep handoff concise and specific.
- **Idempotent** — same inputs (git log, file diffs, pre-routed Asks) MUST produce a byte-identical `SESSION-HANDOFF.md`. Re-running Mode 3 on the same state should be a no-op. Anti-pattern A13 prevents drift here.

## Component Chain

`snapshot → security-preflight if docs changed → verify → promote-memory → harvest if reusable learning appeared`
