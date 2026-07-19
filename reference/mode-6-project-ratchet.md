---
name: anchor-point-mode-6-project-ratchet
type: reference
parent-skill: anchor-point
mode: 6
last_reviewed: 2026-07-19
---

# Mode 6, Project Ratchet

Loaded by the router at [SKILL.md](../SKILL.md). Mode 6 of 7. Related modes: [Mode 4, Audit](mode-4-audit.md) (baseline source), [Mode 7, Doc Workflow Ratchet](mode-7-doc-workflow-ratchet.md) (consumes Mode 6 evidence).

Use when the user wants iterative cleanup of **one project's docs** with scoring, usually on a duplicate before touching the real project. Targets project docs against the existing spec; does not modify the spec itself (that is Mode 7).

Read `reference/doc-ratchet.md` before running this mode. Load `reference/audit-rubric.md` for the baseline score (Hard Gates + 100-point categories) and `reference/output-shapes.md` for the Project Ratchet Output shape.

## Workflow

1. Use `components/security-preflight.md` to define excluded paths.
2. Duplicate only docs and needed manifests into a safe `test-runs/` fixture location outside the source project.
3. Verify the duplicate contains no protected paths before reading or scoring copied files.
4. If a protected path was copied, delete the fixture, rebuild it, and record the setup failure as a harness learning.
5. Record whether the duplicate is full-project or scoped-docs-only.
6. Run Audit on the duplicate to establish a baseline.
7. If the duplicate is scoped, validate Markdown links with fallback to the source project for intentionally omitted non-protected paths.
8. Apply one cleanup strategy to the duplicate.
9. Re-run Audit and compare scores.
10. Keep the attempt only if score improves and context preservation passes.
11. Repeat until the target score, max attempts, or stop condition.
12. Score the reusable workflow with `components/workflow-score.md` when the run exposes a structural miss.
13. Write a delta report to your shared `docs/test-runs/` location.
14. Ask before applying any changes to the real project and include the real-project approval checklist.

## Mode 6 read-budget (bounded; enforced)

To meet rubric v2.0 D2 (Mode 6 budget 15,000 tokens) and HG9 (no run exceeds 2x budget), the baseline audit MUST read no more than this file set, in priority order. Stop reading additional files once the budget is consumed.

| Priority | Files to read | Why |
|---|---|---|
| 1 (always) | The 5 root .md files at most (README, AGENTS, SESSION-HANDOFF or legacy STATUS, ROADMAP, REFERENCES or legacy LOOKUP, and CLAUDE if present) | These hold the v4 canonical content; every drift check applies to them |
| 2 (always) | CONTEXT.md if present | Legacy v1.x signal; triggers A3 |
| 3 (on demand) | `docs/DOCS-INDEX.md` if present | Required for A10 / index drift check |
| 4 (on demand) | `docs/playbooks/*.md` heads (first 30 lines each) | Required for A7 file-scope; full read only if a head suggests detail |
| 5 (on demand) | `docs/reference/*.md` heads (first 30 lines each) | Required for A8 file-scope |
| 6 (on demand) | `docs/dev/*.md` heads (first 30 lines each) | Required for A7 file-scope |
| 7 (never in baseline) | `_archive/` and `_private/` content | HG1, never read these in any mode |

Mode 6 baseline does NOT read source-code directories (`src/`, `app/`, `web/`, etc.), `sequences/`, `node_modules/`, build output, or any content folder. If the project's Mode 4 hard gate "ROADMAP > 300 lines" applies, the skill confirms via `wc -l` only, not by reading the full ROADMAP.

For projects with > 30 .md files in scope: read root + DOCS-INDEX (always); sample up to 10 docs from playbooks/reference/dev (by recency × size heuristic); stop. Note in the audit Summary that sampling was used.

## Mode 6 timing target (bounded; enforced)

To meet rubric v2.0 D6 (Mode 6 cold-start budget 30 min) and WG4 (no run exceeds 2x budget = 60 min), the baseline audit MUST complete within 10 min of wall-clock time on typical-sized projects (≤ 30 .md in scope) and 20 min on large projects (sampling discipline applies). Time decomposition target:

| Phase | Budget | Why |
|---|---|---|
| Skill load (read SKILL.md, components, drift table, routing table) | 30 sec | One-time per session |
| Security preflight (verify no protected paths) | 30 sec | Fast directory scan |
| Inventory + sample selection | 1 min | File listing + heuristic |
| Read priority-1 + priority-2 files (root + CONTEXT) | 1 min | 5-7 files, ~10K tokens total |
| Read priority-3 + sampled priority-4/5/6 files | 2 min | DOCS-INDEX + 10 sampled docs |
| Apply 16 Drift Check Detection criteria | 2 min | Mostly grep/structural, deterministic |
| Compute Health Score (Mode 4 rubric) | 30 sec | Formula, deterministic |
| Write Audit Output (per schema) | 2 min | ~2K-token markdown |
| **Total** | **~9.5 min** | Under 10-min typical target; under 30-min Mode 6 hard budget |

If any phase exceeds 1.5x its target, the skill SHOULD note the slowdown in the Risk Notes section so the cause (e.g., unusual project size, content density) is attributable.

## Rules

- Duplicate first.
- One cleanup strategy per attempt.
- Never optimize score by deleting useful context.
- Never apply ratchet changes to a real project without approval.
- Do not count a scoped-fixture missing source directory as a broken doc link if the same non-protected target exists in the source project.
- Always count links into protected/private paths as failures for current navigation, even if the target exists.
- Treat product content, email sequences, blog posts, and embedded project content as separate inventory categories, not docs clutter.
- When a file moves into history, repair its relative links or explicitly mark it as an unlinked snapshot.
- **Output must be deterministic** for identical fixture inputs. Sort the Findings list by anti-pattern code first (A1, A2, ..., A13, then MG1, MG2, ...), then alphabetically by file path within each code. Sort Proposed Changes by target folder path (lexicographic). Use ISO-8601 timestamps only in the report's frontmatter `run-date` field, never inside finding bodies or score tables. Score values are integers in `[0, 100]`; never use floating point in dimension scores. Two runs on identical fixture inputs MUST produce byte-equivalent output modulo frontmatter timestamps and run UUIDs. (Rubric v2.0 D4 + HG3.)
- **Routing decisions follow `reference/drift-checks.md`, not per-finding judgment.** For each Finding, the Proposed Change is fully determined by its code via the detection table. No ad-hoc routing; if a code is not in the table, that is a skill-extension request (file an issue), not a judgment call.
- **Self-verify against `reference/drift-checks.md` "Routing accuracy test cases" (RT-01 through RT-13).** Any drift from those expected outputs is a routing-accuracy defect.

## Component Chain

`security-preflight → snapshot → doc-audit baseline → duplicate cleanup attempts → verify → promote-memory if reusable learning appeared`
