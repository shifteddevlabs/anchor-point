# Canonical File Set + Where-Things-Go Decision Tree

The 9 canonical surfaces (6 root files + 3 docs/ subfolders that are always relevant) plus a complete decision tree for "where does this new file go?"

---

## The canonical 6 root files

Every project should have these. They map 1-to-1 to the 7-bucket architecture (bucket 6 lives under `docs/`).

| File | Purpose | Length budget |
|---|---|---|
| `README.md` | Public face — what is this, quick start, tech stack | 50-100 lines |
| `AGENTS.md` | AI bootstrap — identity, rules, folder map, dev commands, gotchas | 100-200 lines max |
| `CONTEXT.md` | Current phase — what we're building, success criteria, out-of-bounds | 30-50 lines |
| `SESSION-HANDOFF.md` | Latest delta — what shipped, next plan, blockers, gotchas | 50-200 lines (rotate older content out) |
| `ROADMAP.md` | Priorities — current red/yellow/green, vNext, completed, decision log | 50-150 lines |
| `REFERENCES.md` | Topic-driven lookup — SOT registry, playbooks index, gotchas, external docs | 80-150 lines |

---

## The `docs/` ecosystem

| Path | Purpose | When to create |
|---|---|---|
| `docs/DOCS-INDEX.md` | Location-driven navigation aid | Project has >10 docs in `docs/` (OMIT for greenfield) |
| `docs/history/` + `README.md` | Completed work, old handoffs, retired roadmap detail | First real history item to preserve |
| `docs/reference/` | Verified-state SOT files | First incident where summary went stale |
| `docs/playbooks/` | Process runbooks | First documented process worth reusing |
| `docs/dev/` | Architecture / feature design | Internal architecture worth documenting |
| `docs/release/` | Release notes / migration steps | First versioned release |
| `docs/reviews/` | Code review reports / audits | First external review |
| `docs/research/` | Experiments / exploration drops | First research drop |
| `docs/_archive/` | Historical material (dated batches) | First archival action |
| `docs/_private/` | Gitignored sensitive content | First sensitive drop |

---

## The decision tree — where does a new file go?

When a new file needs to be created, walk this table top-to-bottom. First match wins.

| If the file is... | Goes to | Naming | Also do |
|---|---|---|---|
| One of the 6 canonical root docs | Project root | ALL-CAPS-KEBAB.md | None — it's already canonical |
| Architecture / API design / schema doc | `docs/dev/` (or `docs/dev/<tech>/` if 3+ exist for one tech) | lowercase-kebab.md | None |
| Release notes / changelog | `docs/release/` | `vX.Y-release-notes.md` | None |
| Migration plan / per-migration decisions | `docs/dev/migrations/` (group when 3+) | `YYYY-MM-DD-<migration-name>.md` | None |
| Migration runbook / rollback procedure | `docs/playbooks/` | `migration-playbook.md`, `migration-rollback.md` | Add row to REFERENCES.md Playbooks index |
| Migration notes tied to a release | `docs/release/` | `vX.Y-migration-notes.md` | None |
| Code review report / security audit | `docs/reviews/` | lowercase-kebab.md | None |
| Completed work / old handoff detail / retired roadmap detail | `docs/history/` | `YYYY-MM-DD-<topic-or-session>.md` | Update `docs/history/README.md` |
| Verified-state config (OAuth IDs, env vars, exact values) | `docs/reference/` (or `docs/reference/<tech>/` if 3+) | lowercase-kebab.md | Add row to REFERENCES.md SOT registry |
| Process runbook ("when X happens, do Y") | `docs/playbooks/` (or `docs/playbooks/<tech>/` if 3+) | lowercase-kebab.md | Add row to REFERENCES.md Playbooks index |
| Research / experiment / exploration | `docs/research/` | lowercase-kebab.md | None |
| Sensitive (secrets, internal numbers, business docs) | `docs/_private/` | lowercase-kebab.md | None |
| Don't know where it goes | Drop in `docs/research/` and let Audit relocate later, or ask before creating | lowercase-kebab.md | Flag for Audit |

---

## Grouping rule (3+ docs about same topic)

When a topic accumulates 3+ files, group them into a topic-named subfolder under the appropriate type folder:

- 3+ Supabase architecture docs → `docs/dev/supabase/`
- 3+ Stripe runbooks → `docs/playbooks/stripe/`
- 3+ verified-state files for one integration → `docs/reference/<integration>/`

When fewer than 3 exist, leave them flat (e.g., `docs/dev/auth-flow.md` is fine if it's the only auth doc).

Audit enforces this — when 3+ files share a topic prefix or repeated keyword, it proposes creating a subfolder and moving them together.

---

## What gets auto-updated when files move

Audit never just moves files in isolation. After every approved structural change, it auto-syncs:

1. **AGENTS.md "Folder Structure" section** — reflects the new layout
2. **`docs/DOCS-INDEX.md`** — regenerated with the new file map
3. **All cross-references** — scans every doc for pointers to old paths and updates them (link rot prevention)
4. **REFERENCES.md "Project documentation"** section — refreshed if structure changed

This is what makes Audit safe — moves are atomic AND fully reconciled across the doc tree.

---

## Why this organization works

**Predictability:** an agent or human can find any file by walking the tree. Architecture docs? `docs/dev/`. Verified config? `docs/reference/`. Completed work history? `docs/history/`. No guessing.

**Compounds over time:** as the project grows, the structure scales. Three migrations? `docs/dev/migrations/`. Three Stripe docs? `docs/dev/stripe/`. Audit handles the consolidation when thresholds are crossed.

**Easy to audit:** the Audit workflow knows exactly what's expected at each location. Anything unexpected is an orphan that needs relocating.

**Survives team handoffs:** a contractor or new developer can clone the repo and navigate it without onboarding. The conventions are written into the file tree itself.

**Tool-agnostic:** the architecture works whether you use Claude, ChatGPT, Cursor, Cowork, or no AI tool at all. The structure is what matters.

**Adapter-friendly:** tool-specific commands, installed skills, and scheduled jobs should point back to this capability instead of becoming separate sources of truth.
