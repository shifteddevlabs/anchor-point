# Session Handoff

> **Last Updated:** [DATE]
> **Version:** v1.0
> **Last Session:** [Brief one-line summary of the session being handed off]

---

## Current State (TL;DR)

[2-4 sentences describing what works, what's in progress, what's blocked. Example: "v0.2 in progress. Auth flow complete, task CRUD is 60% done (read + create work, edit + delete pending). Last session shipped server actions for task creation. Test coverage at 78%."]

---

## Last Session — What Shipped

[5-15 bullet points of what was completed this session. Be specific.]

- [Implemented X in file Y]
- [Fixed bug Z by changing approach to W]
- [Added tests for the auth flow — coverage now 78%]

---

## Next Session — Execution Plan

[Pick this up cold. Each item should be actionable without reading other files. Include specific file paths, function names, and exact next steps.]

1. **First priority** — [What to tackle first and WHY]
   File: `path/to/file.ts` — function `editTask`. Mirror the `createTask` pattern at line 47.
2. **Second priority** — [Next most important]
   File: `path/to/other.ts` — implement the optimistic UI pattern from `docs/dev/optimistic-ui.md`.
3. **Third priority** — [If time permits]

---

## Active Blockers

Waiting on external action (user, third-party, async process). Each blocker names the unblock condition.

(or: "None" if none)

- [Blocker 1, e.g., "Waiting on design review for the new dashboard — unblocks once Figma link arrives"]

---

## Active Gotchas

Session-specific hazards. Persistent rules go in CLAUDE.md, not here.

(or: "None yet" if none)

- [Active gotcha, e.g., "The local Supabase migration is one ahead of staging — don't run migrations on staging until we sync"]

---

## Debugging Playbook (if X breaks again)

Proven recipes for failures we've already debugged once. Each entry is a short repro + fix path.

(OMIT this entire section on day 1 — populate as incidents accumulate.)

| If you see... | Run / check first | Why |
|---|---|---|
| [e.g., "Auth redirect loop on login"] | [Check NEXT_PUBLIC_AUTH_REDIRECT_URL matches dashboard config exactly] | [Trailing slash mismatch is the recurring cause] |
| [e.g., "Tests pass locally but fail in CI"] | [Verify test database is reset before CI runs] | [State leak from prior test runs] |

---

## Asks the docs could have answered

Questions the agent had to ask the user this session that *should have been* in CLAUDE.md / CONTEXT.md / REFERENCES.md / a SOT file. This list drives doc improvement, NOT user-blame.

When the audit/update workflow runs at session end, it migrates each entry into the correct file and clears it from this list.

(OMIT this section entirely if it's empty.)

- [e.g., "Asked: 'where do I find the Supabase project ref?' — should have been in REFERENCES.md infrastructure section. Add row."]

---

**Rotation rule:** when the next session starts and the audit/update workflow runs, the prior "Last Session" + completed "Next Session" content rotates to `docs/handoff-history/<YYYY-MM-DD>-session-<NN>.md`. This file stays under 200 lines.
