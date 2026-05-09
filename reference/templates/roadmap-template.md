# Roadmap

> **Last Updated:** [DATE]
> **Current version:** [vX.Y]

---

## Current Version — [vX.Y]

### Red — blockers (must ship before release)

(or: "None — all blockers cleared")

- [ ] [Blocker 1, e.g., "Auth flow complete with tests"]
- [ ] [Blocker 2, e.g., "Production deployment pipeline working"]

### Yellow — strongly recommended

(or: "None")

- [ ] [Yellow item 1, e.g., "Pagination on task list"]

### Green — nice to have

(or: "None")

- [ ] [Green item 1, e.g., "Dark mode toggle"]

---

## vNext Backlog ([vX.Y+1])

Polish items + small features queued for the next release after current.

(or: "None yet")

- [Item 1]
- [Item 2]

---

## Future / Later Versions

Larger features scoped to later versions.

(or: "None yet")

- **[v1.0]:** [Major theme, e.g., "Multi-user collaboration"]
- **[v1.5]:** [Major theme, e.g., "Mobile app"]

---

## Completed

Dated history of shipped items. Newest first.

- **2026-MM-DD:** [Item shipped, brief description]
- **2026-MM-DD:** [Item shipped]

---

## Decision Log

**Why** we chose what we chose. Prevents re-litigating settled decisions. Each entry: date, decision, alternatives considered, reason picked.

| Date | Decision | Alternatives | Why we picked it |
|---|---|---|---|
| YYYY-MM-DD | [Initial doc bootstrap] | (none — initial decision) | Project initialization |
| YYYY-MM-DD | [e.g., Picked Next.js 15 + Supabase] | [e.g., Remix + Postgres, T3 stack] | [e.g., Familiar stack, fast iteration, free Supabase tier covers needs] |

---

## Backlog / Ideas

Unscoped future ideas that haven't been triaged into a version yet.

(or: "None yet")

- [Idea 1, e.g., "AI-powered task prioritization"]

---

## Iceboxed

Tracked but explicitly NOT active. If a request touches one of these, surface the icebox reason before doing the work.

(or: "None")

- [Iceboxed item 1, e.g., "Slack integration — iceboxed 2026-04-12; not enough user demand"]

---

## Pre-flight checklist pattern (use for destructive / irreversible items)

Any roadmap item that involves a destructive or irreversible operation (bulk send, schema migration, paid campaign launch, prod data delete) should follow this format. The checklist runs **before** the action; the verification runs **after**.

```
### [Task name]
**Pre-flight checklist** (verify all before action):
- [ ] [check 1, e.g., "API credit balance > N"]
- [ ] [check 2, e.g., "no prior successful send for this batch"]
- [ ] [check 3, e.g., "dual-filter verification passed"]

**Action:** [the thing]

**Verification after:** [how to confirm it worked, e.g., "query logs for unique recipient count, expect ~N"]
```

This pattern is incident-driven: most "irreversible mistake" stories trace back to skipping a pre-flight check that would have caught the issue cheaply. Pre-flighting is cheap; cleanup is not.

---

*Each item has: priority emoji (red/yellow/green), task name, status, notes pointer (to a detail doc if applicable). Item bodies stay short — implementation details go to `docs/release/<item>.md` or `docs/dev/<item>.md`.*
