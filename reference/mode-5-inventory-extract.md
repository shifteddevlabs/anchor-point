---
name: anchor-point-mode-5-inventory-extract
type: reference
parent-skill: anchor-point
mode: 5
last_reviewed: 2026-05-24
---

# Mode 5, Inventory And Extract

Loaded by the router at [SKILL.md](../SKILL.md). Mode 5 of 7. Related modes: [Mode 4, Audit](mode-4-audit.md), [Mode 6, Project Ratchet](mode-6-project-ratchet.md), [Mode 7, Doc Workflow Ratchet](mode-7-doc-workflow-ratchet.md).

Use when scanning a project for reusable learning.

This mode owns the old `project-inventory` workflow. Treat `../project-inventory/` as a trigger adapter, not a separate source of truth.

## Workflow

1. Use `components/security-preflight.md`.
2. Read project bootstrap docs and docs index.
3. Use `components/snapshot.md` for current project context.
4. Inventory docs, playbooks, scripts, skills, templates, routines, and notable process notes.
5. Exclude archives, dependency folders, build output, and generated draft folders unless the user asks.
6. Classify each item:
   - reusable
   - project-specific
   - sensitive
   - stale/archive
   - unclear
7. Use `components/harvest.md` to separate reusable process from project facts.
8. Write or update an inventory in your shared knowledge-ops `inventories/` location.
9. Update the inventories scan-register.
10. Use `components/promote-memory.md` to place durable learnings.
11. Propose extraction targets.
12. Update the Layer 0 home's `AGENTS.md` "Routing-by-task" section (or, in the rare case a workspace ROUTER.md is justified per Rule 5, that file) only after the reusable target exists.

## Rules

- Do not move files during inventory.
- Do not promote local facts into global rules.
- Do not quote secrets.
- Prefer rewriting reusable process over copying project-local docs verbatim.

## Component Chain

`security-preflight → snapshot → harvest → promote-memory`
