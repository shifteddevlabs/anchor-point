# References

Lookup index. AI tools should consult these for context but should not act on them directly. For project rules, see `AGENTS.md`. For current phase, see `CONTEXT.md`.

## Source-of-truth files (SOT)

When generating content / SQL / code about these topics, read the SOT file, not summaries or memory. Each row exists because a summary went stale and caused an incident.

(OMIT this section entirely on greenfield — add rows as SOT files in `docs/reference/` accumulate.)

| Topic | SOT file | Why it's SOT |
|---|---|---|
| [e.g., Auth flow capabilities] | docs/reference/auth-capabilities.md | [e.g., dashboard layout changed; agents kept giving stale nav steps] |

## Playbooks (do-this-when-X runbooks)

Long-form process runbooks live under `docs/playbooks/`. This table is the index. Read a playbook before doing the kind of work it covers, not after.

(OMIT this section entirely if no playbooks exist yet.)

| Playbook | When to read | File |
|---|---|---|
| [e.g., Database migration playbook] | [Before any schema migration] | docs/playbooks/migration-playbook.md |
| [e.g., Deployment runbook] | [Before pushing to prod] | docs/playbooks/deployment-runbook.md |

## Gotchas & API immutables

Per-integration constraints that bite if forgotten: fields that can't be PATCHed (must rebuild), actions that don't cascade, env vars that need defensive handling, refresh-token rotation rules.

(OMIT this section entirely on greenfield — add rows as integrations surface their constraints.)

| Integration | Constraint | Mitigation |
|---|---|---|
| [e.g., Some Ads API] | [e.g., Attribution windows immutable post-creation] | [Set at ad-set creation; rebuild ad set if change needed] |
| [e.g., Some env var] | [e.g., Trailing whitespace crashes widget] | [.trim() on every read] |

## Project documentation

Map of every doc in this project's `docs/` tree. (Auto-maintained by the audit/update workflow if you have one — otherwise update manually.)

| File | Purpose |
|---|---|
| docs/DOCS-INDEX.md | Location-driven navigation aid for the docs/ folder |
| [docs/dev/auth-flow.md] | [Architecture for the auth subsystem] |
| [docs/playbooks/migration-playbook.md] | [Step-by-step for schema migrations] |

## External — official docs to consult

| Topic | URL |
|---|---|
| [e.g., Next.js] | https://nextjs.org/docs |
| [e.g., Supabase Auth] | https://supabase.com/docs/guides/auth |

## External infrastructure

IDs and account numbers only. Never secrets.

| Resource | ID / Account | Where keys live |
|---|---|---|
| [e.g., Vercel project] | [project-id] | [.env.local + Vercel dashboard] |
| [e.g., Supabase project] | [project-ref] | [.env.local] |

## Naming conventions

(Project-specific naming conventions for IDs, tag prefixes, signals, etc. OMIT entirely if your project follows the universal conventions and has no project-specific additions.)

[List any project-specific conventions]

---

*For rules, see `AGENTS.md`. For current phase, see `CONTEXT.md`. For latest deltas, see `SESSION-HANDOFF.md`.*
