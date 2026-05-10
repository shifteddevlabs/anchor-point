# Current Phase

## What we're building

[One paragraph: the current phase's goal. Example: "v0.2 — wire up authentication and the task CRUD flow. The goal is a working MVP that a single user can log in and manage tasks. No collaboration features in this phase."]

Concrete signals of where we are:
- [Signal 1, e.g., "Auth flow complete; tasks read + create working"]
- [Signal 2, e.g., "20 unit tests passing; no E2E yet"]
- [Signal 3, e.g., "Deployed to staging; not in prod yet"]

## What good looks like

When this phase is done:
- [Success criterion 1, e.g., "User can sign up, log in, create/edit/delete tasks"]
- [Success criterion 2, e.g., "All CRUD operations have unit tests with >80% coverage"]
- [Success criterion 3, e.g., "Deployment pipeline pushes to staging on merge to main"]

## What's out of bounds

Things explicitly NOT in scope this phase. If a request touches these, push back or write a ROADMAP entry instead of doing the work.
- [Out-of-bounds 1, e.g., "Multi-user / collaboration features"]
- [Out-of-bounds 2, e.g., "Mobile app (web only this phase)"]

## What I will not assume this phase

State / capabilities / settings that should be verified (read code, run CLI, check dashboard) rather than assumed from memory or summaries. Each entry has a verification path.

| Topic | What NOT to assume | How to verify |
|---|---|---|
| [e.g., Database schema] | [Don't assume any specific column exists] | [Read the latest migration file in supabase/migrations/] |
| [e.g., Deployment env] | [Don't assume which env vars are set] | [Run `vercel env ls` or check the dashboard] |

(OMIT this table entirely if there are no specific verification paths yet — add as the project matures.)

## What to avoid

Patterns we've seen go wrong on this project (phase-specific; persistent rules go in AGENTS.md):
- [Anti-pattern 1, e.g., "Don't add new dependencies without checking bundle size impact"]
- [Anti-pattern 2, e.g., "Don't bypass auth middleware in dev — keep behavior identical to prod"]

## Open questions

[List any decisions that need to be made this phase, or "None" if none.]

---

*This file describes the **active phase** of work. Test for any line: "Will this still be true 3 sessions from now?" — yes → here. No → SESSION-HANDOFF.md. Update when the phase boundary changes (typically monthly), not at every commit.*
