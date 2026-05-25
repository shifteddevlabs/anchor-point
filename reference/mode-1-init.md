---
name: anchor-point-mode-1-init
type: reference
parent-skill: anchor-point
mode: 1
last_reviewed: 2026-05-24
---

# Mode 1, Init

Loaded by the router at [SKILL.md](../SKILL.md). Mode 1 of 7. Related modes: [Mode 2, Review](mode-2-review.md), [Mode 3, Update](mode-3-update.md), [Mode 4, Audit](mode-4-audit.md).

Use when starting docs for a new project or bringing a thin project up to baseline.

## Workflow

1. Read existing `README.md`, `AGENTS.md`, `CLAUDE.md`, package manifests, and visible folder structure.
2. Identify whether the project is new, legacy, or already has a docs system.
3. Create or propose the canonical surfaces.
4. For new model-agnostic infrastructure, create `AGENTS.md`.
5. For existing Claude-shaped projects, preserve `CLAUDE.md` and only add `AGENTS.md` if the user wants cross-model routing.
6. Do not invent stack, commands, rules, or credentials.
7. Use source citations in generated tech-stack or command sections when possible.
8. Use `components/verify.md` for stack, command, or service claims.
9. Use `components/snapshot.md` to seed initial handoff state.

## Outputs

- created or proposed root docs
- initial docs folder map
- first `STATUS.md`
- initial `ROADMAP.md`

## Component Chain

`verify → snapshot`
