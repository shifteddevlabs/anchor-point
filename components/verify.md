# Component: Verify

## Purpose

Prevent stale or guessed facts from becoming project memory.

## Used By

- `doc-review`
- `doc-update`
- `doc-audit`
- `inventory and extract`

## Verify When

- A doc mentions current versions, API behavior, env vars, dashboards, prices, schedules, or external service state.
- A claim says X caused Y.
- A doc uses words like "currently", "live", "latest", "active", "approved", "blocked", "fixed", "done", "complete", "shipped", or "wired".
- A claim would change what an agent does next.

## Workflow

1. Identify claims that are volatile or high impact.
2. Find the verification path:
   - source file
   - manifest
   - CLI
   - dashboard
   - official docs
   - project source-of-truth doc
3. Verify only what affects the current workflow.
4. Mark uncertain claims as hypothesis.
5. Add `Last verified: YYYY-MM-DD` when writing durable verified facts.

## Output

```text
Verified:
- [claim] via [source]

Unverified:
- [claim] needs [verification path]

Hypotheses:
- [claim] labeled as hypothesis until verified
```

## Rules

- Do not browse or call external services unless current information is required.
- Do not ask the user to re-confirm facts already documented in a verified source.
- Do not turn correlation into causation.

