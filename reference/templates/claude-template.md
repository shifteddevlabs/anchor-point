# [PROJECT NAME]

## Identity

[1-2 sentences: who you are, what this project is, who you're working with]

| | |
|---|---|
| **Type** | [web app / iOS app / CLI / library / etc.] |
| **Tech stack** | [one-line summary, e.g. "Next.js + Supabase + Vercel"] |
| **Status** | [Active / Beta / Maintenance] |
| **Live URL** | [https://... or N/A] |

## Hard Rules — Inherited Baseline (do not remove)

These 7 rules are inherited from the global doc spec. They are derived from cross-project incidents that have repeated 3+ times. Do not edit or delete; add project-specific rules in the section below.

1. **Verify before claiming.** Read the code, run the CLI, or open the dashboard. Never answer state-of-the-system questions from memory.
2. **Root-cause, not symptoms.** When something that worked is broken, identify what changed FIRST. Don't layer patches over a broken state.
3. **Quality gate every commit.** Whatever the project's commit/push process is, follow it. Never skip review steps to save time.
4. **Delegate to focused investigation** when a task needs to read >3 unrelated files. Protects context.
5. **Trust documented state.** If a verified-state doc says X, use X. If state has drifted, UPDATE the doc — don't ask the user to re-verify what they already documented.
6. **Hypothesis vs Fact.** Label confidence. Correlation is not causation. Don't pattern-match training data into confident claims without verification.
7. **No session editorializing.** Report what changed and what's next. No "ready to ship," no commentary on session length, effort, or pacing.

## Project-specific Rules

Use **Burned:** [incident] format for incident-driven rules so future agents can judge edge cases.

[Examples:]
- **Never call the production API in dev environment.** **Burned:** sandbox / live env var swap caused a real customer charge in 2026-01.
- **Always run the test suite before committing.** **Burned:** silent regression shipped to prod when tests were skipped 2026-02.

## Tool-usage rules

- **Library docs** before guessing API/SDK usage. Even for well-known frameworks. Training data goes stale.
- **Verify env vars first** when integrations break. The 30-second check often saves 2 hours of dashboard hunting.
- **Never assume SaaS dashboard layouts.** Verify via official docs or ask user what tabs they see. UIs change between versions.
- **Source-of-truth files** (registered in REFERENCES.md) are read instead of summaries when generating content/SQL/code.

## Folder Structure

```
[project-name]/
├── README.md
├── CLAUDE.md                       (this file)
├── CONTEXT.md
├── SESSION-HANDOFF.md
├── ROADMAP.md
├── REFERENCES.md
├── [src/ or app/ or lib/]          (main code)
├── docs/
│   ├── DOCS-INDEX.md               (navigation aid; required >10 docs)
│   ├── handoff-history/            (rotated session backups)
│   ├── reference/                  (verified-state SOT files)
│   ├── playbooks/                  (process runbooks)
│   ├── dev/                        (architecture / feature design)
│   ├── release/                    (release notes)
│   └── _archive/                   (historical material)
└── [other project folders]
```

## Where new files go (creation cheat sheet)

When creating any new doc during a session, follow this cheat sheet. For full detail, see the universal architecture spec.

| If the file is... | Goes to | Naming |
|---|---|---|
| One of the 6 canonical root docs | Project root | ALL-CAPS-KEBAB.md |
| Architecture / API design / schema doc | `docs/dev/` (or `docs/dev/<tech>/` if 3+ exist) | lowercase-kebab.md |
| Release notes / changelog | `docs/release/` | `vX.Y-release-notes.md` |
| Migration plan / decisions | `docs/dev/migrations/` (group when 3+) | `YYYY-MM-DD-<migration-name>.md` |
| Migration runbook / rollback | `docs/playbooks/` | `migration-playbook.md` |
| Code review / security audit | `docs/reviews/` | lowercase-kebab.md |
| Verified-state config | `docs/reference/` | lowercase-kebab.md → also add SOT row in REFERENCES.md |
| Process runbook | `docs/playbooks/` | lowercase-kebab.md → also add Playbooks row in REFERENCES.md |
| Research / experiment | `docs/research/` | lowercase-kebab.md |
| Sensitive (secrets, internal numbers) | `docs/_private/` | lowercase-kebab.md |
| Don't know | Drop in `docs/research/` and let `/doc-audit` relocate later, or ask before creating |

**Rules at the moment of creation:**
- NEVER create a `.md` file at project root unless it's one of the 6 canonical
- NEVER use spaces, underscores (outside `_archive`/`_private` prefix), or camelCase in filenames
- ALWAYS use `lowercase-kebab.md` inside `docs/`
- ALWAYS use `YYYY-MM-DD` for dates in filenames

## Routing-by-task

| When you need to... | Read |
|---|---|
| Know what we're working on now | `CONTEXT.md` |
| Know what just shipped + what's next | `SESSION-HANDOFF.md` |
| Pick up a planned task | `ROADMAP.md` |
| Look up an external URL, infra ID, naming convention | `REFERENCES.md` |
| Look up a SOT file (canonical source) | `REFERENCES.md` → SOT section |
| Follow a process runbook | `REFERENCES.md` → Playbooks section → `docs/playbooks/*.md` |
| Look up a verified-state config (OAuth, env vars, infra) | `docs/reference/*.md` |
| Find a file in `docs/` by location | `docs/DOCS-INDEX.md` |

## Tech Stack

Versioned table. Every fact cites the file it came from (Source column). The Detail column is OPTIONAL — include only when per-tech deep docs exist; OMIT individual rows' Detail entries when no deep doc exists yet.

| Layer | Technology | Version | Source | Detail |
|---|---|---|---|---|
| [e.g., Framework] | [e.g., Next.js] | [version] | [package.json] | [docs/dev/<tech>/ or — if none yet] |
| [e.g., Database] | [e.g., Supabase] | [version] | [package.json (@supabase/supabase-js)] | [— or path to deep docs] |

## Environment Variables

Names only. Values live in `.env.local` (never commit).

```bash
# Core
[NEXT_PUBLIC_API_URL]=
[DATABASE_URL]=

# [Service-specific]
[SERVICE_API_KEY]=
```

## Development Commands

Only commands TRACEABLE to actual scripts in your manifest (`package.json`'s `scripts`, `Cargo.toml`'s `[bin]`, etc.). If your manifest has no scripts defined, OMIT this section entirely.

```bash
# Examples
npm run dev          # local development
npm run build        # production build
npm test             # run tests
```

## Important Notes / Gotchas

[Numbered list, persistent warnings — added as discovered. OMIT this section entirely on greenfield.]

1. [Gotcha or non-obvious behavior — use Burned: format if incident-driven]
2. [Rate limits or constraints]

---

*Read this file first on every new task. When unsure, verify via the source-of-truth file (see REFERENCES.md) before asking.*
