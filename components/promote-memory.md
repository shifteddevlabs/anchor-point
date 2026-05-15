# Component: Promote Memory

## Purpose

Move durable learnings to the right memory layer so future agents can find them without rereading old chats.

## Used By

- `doc-update`
- `doc-audit`
- `inventory and extract`

## Memory Ladder

```text
Chat/session insight
→ SESSION-HANDOFF.md
→ project AGENTS.md / CLAUDE.md / REFERENCES.md / docs/playbooks
→ +vantage-point scan register
→ department / skill / routine
→ ROUTER.md
```

## Workflow

1. Identify learnings from the current session.
2. Classify each learning:
   - session-only
   - project rule
   - project source-of-truth fact
   - project runbook
   - reusable cross-project process
   - security warning
3. Place it at the lowest durable layer that can answer future questions.
4. Promote only stable, reusable lessons upward.
5. Update the router only after a reusable department, skill, or routine exists.

## Placement Rules

| Learning Type | Home |
|---|---|
| What happened this session | `SESSION-HANDOFF.md` |
| Durable project rule | `AGENTS.md` or existing project bootstrap doc |
| Verified project fact | `REFERENCES.md` or `docs/reference/` |
| Repeatable project process | `docs/playbooks/` |
| Cross-project reusable process | `+vantage-point/skills/`, `departments/`, or `routines/` |
| Sensitive warning | `+vantage-point/docs/security/` plus local private handling |

## Rules

- Do not promote project facts into global doctrine.
- Do not duplicate the same rule in multiple places.
- Prefer pointers over copying large docs.

