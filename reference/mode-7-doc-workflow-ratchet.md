---
name: anchor-point-mode-7-doc-workflow-ratchet
type: reference
parent-skill: anchor-point
mode: 7
last_reviewed: 2026-05-24
---

# Mode 7, Doc Workflow Ratchet

Loaded by the router at [SKILL.md](../SKILL.md). Mode 7 of 7. Related modes: [Mode 6, Project Ratchet](mode-6-project-ratchet.md) (produces the evidence Mode 7 consumes), [Mode 4, Audit](mode-4-audit.md) (provides the rubric used for project doc scoring).

Use when the user wants to evaluate the rules and workflows themselves, usually after two or three Project Ratchet runs. Targets **the doc workflow** (SKILL, rubric, components, reference files); uses evidence from Mode 6 runs as input.

Read `reference/workflow-autoresearch.md` and `components/workflow-score.md`.

## Workflow

1. Read the evidence run reports.
2. Score the project docs using the audit rubric.
3. Score the reusable workflow using `components/workflow-score.md`.
4. Identify structural misses that the workflow should have caught earlier.
5. Change one workflow rule, rubric, component, or routine at a time.
6. Run the workflow-score script (`scripts/score-doc-workflow.js` in your shared knowledge-ops location).
7. Keep the change if workflow readiness improves or closes a hard gate without adding noise.
8. Defer changes that are project-specific or belong in an overlay.
9. Write a workflow-autoresearch report in your shared `docs/test-runs/` location.

## Rules

- Do not modify real project folders in Doc Workflow Ratchet mode.
- Do not promote a one-project fact into a global rule.
- Optimize for safer future runs, not just a higher score.
- A workflow score of 95+ is required before scheduling.
