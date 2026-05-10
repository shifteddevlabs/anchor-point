# Workflow Autoresearch

Use this to harden the documentation workflow itself. It is for structural misses in Init, Review, Update, Audit, Ratchet, and their internal components.

## Inputs

- At least two prior project ratchet reports.
- Preferred evidence set: three materially different projects.
- Current `rules.md`.
- Current `reference/doc-ratchet.md`.
- Current `reference/workflow-score.md`.
- Current templates and anti-pattern catalog.

## Loop

1. Read the project ratchet reports.
2. Score each project result with the project doc rubric.
3. Score the reusable workflow with `reference/workflow-score.md`.
4. Identify structural misses:
   - things the workflow failed to check early enough
   - things it checked manually but did not encode as a rule
   - places where cleanup could damage project context
   - places where one model or platform would miss routing
5. Pick one workflow change.
6. Edit the smallest relevant workflow doc, component, or template.
7. Re-score the workflow.
8. Keep the change if the workflow score improves or closes a hard gate without adding noise.
9. Defer the change if it is project-specific, too speculative, or better handled by a local overlay.
10. Repeat for the configured experiment count.
11. Write a report.

## Keep Criteria

Keep a workflow rule when:

- It prevents a real miss found in at least one run and is plausible across other projects.
- It improves safety, context preservation, routing, or repeatability.
- It is specific enough to execute.
- It does not force all projects into one layout when a scoped exception is safer.

## Reject or Defer Criteria

Reject or defer a workflow rule when:

- It only fits one project and belongs in that project's overlay.
- It encourages deleting useful context.
- It adds a command or document name without a real decision point.
- It makes scheduled automation more dangerous.
- It duplicates an existing rule without clarifying execution.

## Output

```text
Summary
Evidence Runs
Baseline Workflow Score
Final Workflow Score
Structural Misses Found
Experiments
Kept Rules
Rejected or Deferred Rules
Project Doc Score Impact
Workflow Score Impact
Real-Project Approval Checklist
Next Hardening Target
```

## Schedule Readiness

Do not schedule a workflow until:

- Three different project shapes have passed duplicate-only ratchets.
- The workflow score is at least 95.
- The real-project approval checklist exists.
- Protected/private path checks are automated or explicitly scripted.
- The workflow has clear stop conditions for private data, live verification, and unclear ownership.
