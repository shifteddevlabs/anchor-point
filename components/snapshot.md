# Component: Snapshot

## Purpose

Capture current state and recent changes so the next agent can resume without rereading the whole project.

## Used By

- `doc-review`
- `doc-update`
- `doc-audit`
- `inventory and extract`

## Inputs

- Current project path
- Root docs when present
- Git status/log when the project is a repo
- Recent file changes

## Workflow

1. Identify the project and active phase.
2. Read `AGENTS.md` or `CLAUDE.md`, then `STATUS.md` (or legacy `SESSION-HANDOFF.md`), `ROADMAP.md`, and `LOOKUP.md` (or legacy `REFERENCES.md`) when present.
3. Check recent git activity if available.
4. Capture:
   - current state
   - last completed work
   - active blockers
   - next likely actions
   - files or docs that changed
5. Keep the snapshot concise enough to fit in `STATUS.md`.

## Output

```text
Current state:
Last changed:
Active blockers:
Next action:
Docs that may need sync:
```

## Rules

- Do not infer system state from memory.
- Do not scan the entire project unless the active mode requires it.
- Do not include secrets.
