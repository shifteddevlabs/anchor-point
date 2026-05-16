# {{PROJECT_NAME}} Agent Instructions

{{PROJECT_TAGLINE}}

## Identity

{{PROJECT_DESCRIPTION_ONE_PARAGRAPH}}

## Current Phase

**What we're building:** {{CURRENT_PHASE_GOAL}}

**Success criteria:**
- {{SUCCESS_CRITERION_1}}
- {{SUCCESS_CRITERION_2}}
- {{SUCCESS_CRITERION_3}}

**Out of bounds (don't touch this phase):**
- {{OUT_OF_BOUNDS_1}}
- {{OUT_OF_BOUNDS_2}}

## Routing-by-task

| If you need to... | Open |
|---|---|
| Understand current state + last session | `STATUS.md` |
| See what's next | `ROADMAP.md` |
| Find a verified config / SOT | `docs/reference/<topic>.md` (see LOOKUP.md SOT registry for index) |
| Follow a process runbook | `docs/playbooks/<topic>-playbook.md` (see LOOKUP.md Playbooks index) |
| Look up an API constraint | `LOOKUP.md` API Constraints section |
| Read decision rationale | `docs/decisions/<date>-<topic>.md` |
| Check release notes | `docs/release/v<X.Y>-notes.md` |
| Check completed work history | `docs/status-history/<date>-session-<NN>.md` |
| Find cross-project rules | `{{LAYER_0_HOME}}/AGENTS.md` (replace with your Layer 0 path; e.g., `+vantage-point/AGENTS.md`) |

## Where new files go

| Type | Destination | Naming |
|---|---|---|
| Architecture / schema / API design | `docs/dev/` | `lowercase-kebab.md` |
| Verified-state config (IDs, env, version pins) | `docs/reference/` | `<topic>.md` |
| Process runbook ("when X, do Y") | `docs/playbooks/` | `<topic>-playbook.md` |
| Code review / security audit | `docs/reviews/` | `YYYY-MM-DD-<scope>.md` |
| Release notes / migration | `docs/release/` | `v<X.Y>-notes.md` |
| Research / exploration | `docs/research/` | `<topic>.md` |
| Decision rationale | `docs/decisions/` | `YYYY-MM-DD-<topic>.md` |
| Persistent rule for this project | `AGENTS.md` "Project Rules" section | use **Burned:** [incident] format |
| Sensitive content (gitignored) | `docs/_private/` | per project convention |
| Retired content | `docs/_archive/<YYYY-MM-DD-batch>/` | (dated batch folder) |

## Project Rules

{{PROJECT_RULES_OR_EMPTY}}

Use **Burned:** [incident] format for incident-driven rules. Example:

> **Burned:** [2026-04-12 Meta OAuth outage] Never assume Meta access tokens refresh silently. Always confirm via `vercel env pull` before deploying after a Meta config change.

## Tech Stack

| Component | Version | Source |
|---|---|---|
{{TECH_STACK_ROWS}}

The Source column cites the file the version came from (e.g., `package.json`, `Cargo.toml`). Single-owner: this table is the canonical version registry. Other docs should point here, not duplicate.

## Gotchas

{{GOTCHAS_OR_EMPTY}}

Sentence-form discipline:
- **Imperative** ("do X" / "don't Y") = rule (this section or "Project Rules")
- **Declarative** ("X is Y") = fact (Tech Stack or `docs/reference/`)
- **Conditional** ("if X breaks, do Y") = recipe (STATUS.md Debugging Playbook)

## Inherits from

This project inherits cross-project conventions from `{{LAYER_0_HOME}}` (replace with your monorepo's Layer 0 path; for example, `+vantage-point/AGENTS.md`), including:
- Layer 0 Hard Rules baseline (security, push, root-cause, verify-before-claim, etc.)
- File naming conventions (see `{{LAYER_0_HOME}}/docs/conventions/file-naming-and-frontmatter.md`)
- Routing conventions (see `{{LAYER_0_HOME}}/ROUTER.md`)

Project-specific rules above OVERRIDE inherited defaults when they conflict. Surface conflicts per `{{LAYER_0_HOME}}` Hard Rule #8.
