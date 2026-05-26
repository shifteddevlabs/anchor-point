# Session Handoff — {{PROJECT_NAME}}

> **Updated:** {{YYYY-MM-DD}}

## TL;DR

{{TWO_LINE_PROJECT_SNAPSHOT}}

## Last session shipped

{{COMPUTED_FROM_GIT_LOG_OR_USER_PROVIDED}}

- {{COMMIT_OR_CHANGE_1}}
- {{COMMIT_OR_CHANGE_2}}

## Next session

Pointer to `ROADMAP.md` top 3, plus 1-2 specific pickup notes:

- {{SPECIFIC_NEXT_STEP_1}} — file path, function name, exact change
- {{SPECIFIC_NEXT_STEP_2}}

## Active blockers

(or: "None")

- {{BLOCKER_1}} — owner, status, ETA

## Debugging Playbook

Conditional recipes that survived prior sessions. **Conditional form** ("if X breaks, do Y") only — declarative facts go to `docs/reference/`, imperative rules go to `AGENTS.md`.

(or: "None yet")

- **If {{SYMPTOM}}:** {{ACTION_RECIPE}}
- **If Meta OAuth fails after deploy:** run `vercel env pull` first; check `META_APP_SECRET` is not empty.

## Asks (pre-routed)

Open questions that need a decision. Each Ask carries a routing tag in brackets so doc-handoff knows where the answer goes. Untagged Asks are an anti-pattern (A12).

(or: "None")

- `[AGENTS.md: Project Rules]` Should we add a "never log raw HealthKit data" rule? (came up Session 14)
- `[docs/reference/api-keys.md]` Confirm Stripe webhook secret rotation date.
- `[ROADMAP.md: vNext]` Decide whether to support Apple Sign-In before v2.0.

---

*SESSION-HANDOFF.md is HOT — loaded every session. Keep it ≤ 150 lines. When it exceeds 200, rotate the oldest 50% to `docs/handoff-history/<YYYY-MM-DD>-NN.md` BEFORE the next write. doc-handoff (Mode 3) does this automatically.*
