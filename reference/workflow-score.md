# Workflow Score Rubric

Score the reusable documentation workflow itself, separately from the project docs it improves.

Use this after every doc-ratchet or multi-project hardening run.

## Scorecard

Total: 100 points.

| Category | Points | Checks |
|---|---:|---|
| Fixture safety | 15 | Protected/private paths are excluded before reading; setup failures are deleted, rebuilt, and recorded. |
| Scoring integrity | 15 | Baseline, attempts, kept/discarded decisions, and hard gates are explicit and reproducible. |
| Context preservation | 15 | Useful history, decisions, references, and project facts remain discoverable after cleanup. |
| Generalization | 15 | Rules are tested across at least three different project shapes before being considered stable. |
| Rule promotion quality | 15 | Local facts stay local; reusable misses become explicit global rules only when justified. |
| Real-project readiness | 15 | Duplicate wins become an approval checklist before touching the real project. |
| Automation readiness | 10 | The workflow is clear enough to schedule, with stop conditions and failure handling. |

## Hard Gates

| Condition | Score cap |
|---|---:|
| Protected/private path copied and read before verification | 59 |
| Real project modified without explicit approval | 49 |
| Workflow improves score by deleting context instead of preserving or relocating it | 69 |
| A one-project pattern is promoted globally without cross-project validation | 79 |
| No workflow score reported after a multi-project hardening run | 84 |
| No approval checklist for real-project application | 89 |

## Health Bands

| Score | Meaning |
|---:|---|
| 95-100 | Schedule-ready candidate |
| 90-94 | Strong, but needs one more hardening or approval-gate check |
| 80-89 | Useful, not ready for automation |
| 65-79 | Manual-only workflow, structural misses remain |
| Below 65 | Rebuild the workflow before applying to real projects |

## Required Output

Every workflow-autoresearch report must include:

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
