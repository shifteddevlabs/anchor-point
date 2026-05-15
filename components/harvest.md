# Component: Harvest

## Purpose

Extract reusable process, patterns, prompts, checklists, and workflows from project-local work into `+vantage-point/`.

## Used By

- `doc-update` when reusable learning appeared during a session
- `doc-audit` when structural review surfaces reusable material
- `inventory and extract`

## Workflow

1. Identify candidate reusable material.
2. Separate reusable process from project-specific facts.
3. Classify the candidate:
   - department
   - skill
   - routine
   - adapter
   - project overlay
   - source example
4. Update `+vantage-point/docs/inventories/scan-register.md`.
5. Draft the extraction in the appropriate place.
6. Update `ROUTER.md` only after the extracted asset exists.

## Output

```text
Reusable candidates:
Project facts to leave local:
Sensitive items:
Proposed extraction:
Router update needed:
```

## Rules

- Rewrite for reuse instead of copying project docs verbatim.
- Keep examples linked to the source project unless they are generic.
- Do not move source files during harvest unless a separate migration plan is approved.

