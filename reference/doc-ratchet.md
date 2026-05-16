# Doc Ratchet Workflow

Use this when a project has doc drift, oversized handoffs, stale current-state files, unclear routing, or a low audit score, and the user wants an iterative cleanup instead of one large restructure.

## Core Idea

Duplicate first. Score first. Change one strategy at a time. Keep only improvements that raise the score without losing useful context.

## Inputs

| Input | Default |
|---|---|
| Source project path | Required |
| Target score | 85 |
| Maximum attempts | 5 |
| Cleanup scope | Docs only |
| Protected paths | Always excluded before reading |

## Protected Paths

Never copy, read broadly, or mutate these during ratchet setup:

- `.git/`
- `.next/`
- `.vercel/`
- `.pnpm-store/`
- `node_modules/`
- `coverage/`
- `dist/`
- `build/`
- `.venv/`
- `venv/`
- `.claude/projects/`
- `zzz_private/`
- `zzz-private/`
- `_private/`
- `docs/_private/`

## Workflow

1. Create a duplicate fixture outside the source project.
2. Exclude protected paths before including Markdown or manifests.
3. Verify the duplicate contains no protected paths before reading or scoring copied files.
4. If a protected path was copied, delete the fixture, rebuild it, and record the setup failure as a harness learning.
5. Record whether the duplicate is full-project or scoped-docs-only.
6. Run Audit baseline scoring on the duplicate.
7. When the duplicate is scoped, run link checks with source-project fallback for intentionally omitted non-protected paths.
8. Report duplicate-only fixture gaps separately from true broken links.
9. Pick one cleanup strategy.
10. Apply it to the duplicate only.
11. Re-run Audit and compare against the previous best.
12. Keep the attempt if it improves score and preserves context.
13. Discard or ignore the attempt if score drops, links break, or useful context disappears.
14. Repeat until target score, max attempts, or diminishing returns.
15. Write a final delta report.
16. Ask before applying any selected cleanup plan to the real project.

## Cleanup Strategies

Try one strategy per attempt:

- Compress `STATUS.md` and move rotated context to `docs/status-history/`.
- Split implementation detail out of `ROADMAP.md`.
- De-duplicate active/archive file pairs.
- Move root docs index to `docs/DOCS-INDEX.md` or document the legacy exception.
- Clarify AGENTS.md and tool-specific adapter authority.
- Update stale current-state claims after verification.
- Repair links after moves.
- Repair links inside archived snapshots after moving them.
- Move historical docs to `_archive/`.
- Separate product/content Markdown from operating docs before scoring.

## Keep Criteria

Keep an attempt only when:

- Audit score improves.
- Hard gates clear or move to a less severe cap.
- Context preservation passes.
- Sensitive docs remain excluded or private.
- Important docs remain discoverable through an index, router, or reference doc.
- Product facts stay local and reusable process gets harvested intentionally.

## Output

```text
Summary
Baseline Score
Best Score
Score Delta
Attempts
Kept Strategies
Discarded Strategies
Context Preservation Notes
Recommended Real-Project Changes
Approval Items
```

## Stop Conditions

Stop when one of these is true:

- Target score reached.
- Max attempts reached.
- Current best is blocked by a hard gate that needs human decision or live verification.
- Two consecutive attempts fail to improve.
- Cleanup requires project decisions the agent should not make.
- Security preflight finds sensitive material that needs manual review.

## Real-Project Approval Checklist

Before touching a real project, confirm:

- Source project path.
- Duplicate best attempt path.
- Baseline score and best duplicate score.
- Exact files proposed for real-project edits.
- Files that will be moved, archived, or created.
- Private/protected paths that remain excluded.
- Live-verification items that will not be rewritten as facts.
- Rollback plan, usually a branch, commit boundary, or copied backup.
- Post-application checks: protected/private path check, strict secret-pattern check, Markdown link check, duplicate-doc check, line counts for `STATUS.md` (or legacy `SESSION-HANDOFF.md`) and `ROADMAP.md`, and project doc score.

If any checklist item is unclear, stop and ask.
