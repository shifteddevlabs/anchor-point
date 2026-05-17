# Naming Conventions

Universal naming rules across all projects. Anchor Point applies these in Init workflow (at file creation), enforces them in Audit workflow (renames violators), and detects them in Review workflow (flags violations + suggests Audit).

---

## The rules

**Root-level files (the 5 canonical, v3.0+):** `ALL-CAPS-KEBAB.md`
- `README.md`, `AGENTS.md`, `STATUS.md`, `ROADMAP.md`, `LOOKUP.md`
- Hyphens between words (no spaces, no underscores, no camelCase)
- Always `.md` extension
- `CLAUDE.md` (and other vendor-specific bootstrap files) become 1-line stubs pointing to `AGENTS.md` when present
- **Legacy v1.x:** projects on the old shape had `CONTEXT.md` (now absorbed into AGENTS.md §2), `SESSION-HANDOFF.md` (renamed to `STATUS.md`), and `REFERENCES.md` (renamed to `LOOKUP.md`). Migrated by Audit (Mode 4)

**`docs/` files:** `lowercase-kebab.md`
- `meta-app-capabilities.md`, `auth-flow-design.md`, `stripe-webhook-flow.md`
- Hyphens between words
- Always `.md` extension
- **Exception:** `docs/DOCS-INDEX.md` follows root convention (ALL-CAPS-KEBAB) because it functions as a top-level navigation file within `docs/`
- **Exception:** subfolder `README.md` files (e.g., `docs/status-history/README.md`, `docs/_archive/<batch>/ARCHIVE-README.md`) follow GitHub-standard ALL-CAPS naming

**Folders (the 9 canonical docs/ subfolders, v3.4.1):** `lowercase-kebab/`
- `status-history/`, `roadmap-history/`, `decisions/`, `reference/`, `playbooks/`, `dev/`, `release/`, `reviews/`, `research/`
- Hyphens between words
- **Exception:** sort-to-bottom folders use leading underscore: `_archive/`, `_private/`
- **Legacy:** old `zzz-archive/` and `zzz-private/` get migrated by the Audit workflow → `docs/_archive/` and `docs/_private/`. Old `history/` was split into `status-history/` and `roadmap-history/` in v3.1.

**Date format in filenames:** `YYYY-MM-DD` (ISO-8601, sortable)
- Session files: `YYYY-MM-DD-session-NN.md` (e.g., `2026-04-19-session-25.md`)
- Archive batches: `_archive/YYYY-MM-DD-<description>/` (e.g., `_archive/2026-05-08-stale-cleanup/`)

**Forbidden in any filename or folder name:**
- Spaces (`my file.md` → `my-file.md`)
- Underscores in word separation (`my_file.md` → `my-file.md`; underscore is reserved for the `_archive/`/`_private/` sort prefix only)
- camelCase or PascalCase (`MyFile.md` → `my-file.md`; root files use ALL-CAPS-KEBAB instead)
- Mixed conventions in the same directory

---

## Why this matters

Consistent naming makes the file tree scannable for both humans and AI tools.

- **ALL-CAPS at root** signals "important top-level doc"
- **lowercase-kebab in `docs/`** signals "detail file"
- **Date prefixes** sort chronologically, automatically
- **Leading underscore** sorts archive/private folders to the bottom of every directory listing

When violated, agents waste cycles guessing the canonical filename. ("Is it `auth.md` or `Auth.md` or `auth_flow.md`?")

---

## Detection signatures (used by Audit workflow)

| Signature | Indicates | Fix |
|---|---|---|
| Root file not matching `^[A-Z][A-Z0-9-]*\.md$` (or one of the 6 canonical names) | Root naming violation | Rename to ALL-CAPS-KEBAB.md or move to docs/ if not canonical |
| `docs/` file containing uppercase letters (except DOCS-INDEX.md and subfolder README.md) | docs/ naming violation | Rename to lowercase-kebab.md |
| Filename or folder containing space, underscore (outside leading `_` exception), or camelCase | Generic naming violation | Rename to use hyphens / drop the underscore / convert camelCase |
| Mixed naming in same directory | Inconsistent convention | Pick the majority convention; rename the outliers |
| Date format other than YYYY-MM-DD in any filename | Non-ISO date | Reformat to YYYY-MM-DD |
| Legacy `zzz-` prefix anywhere | Legacy archive/private folder | Migrate to `docs/_archive/` or `docs/_private/` |

---

## Examples

**Good:**
```
my-project/
├── README.md
├── AGENTS.md
├── STATUS.md
├── ROADMAP.md
├── LOOKUP.md
└── docs/
    ├── DOCS-INDEX.md
    ├── status-history/
    │   ├── README.md
    │   └── 2026-04-19-session-25.md
    ├── roadmap-history/
    │   ├── README.md
    │   └── 2026-05-15-roadmap-snapshot.md
    ├── decisions/
    │   ├── README.md
    │   └── 2026-04-19-adopt-resend.md
    ├── reference/
    │   └── meta-app-capabilities.md
    ├── playbooks/
    │   └── deployment-playbook.md
    ├── dev/
    │   └── auth-flow-design.md
    ├── _archive/
    │   └── 2026-03-15-pre-rebrand-cleanup/
    │       └── ARCHIVE-README.md
    └── _private/
        └── pricing-strategy-q3.md
```

**Bad:**
```
my-project/
├── README.md                           ✓ correct
├── agents.md                           ✗ should be AGENTS.md
├── My Notes.md                         ✗ space + camelCase + at root
├── status_v2.md                        ✗ underscore + lowercase (also: STATUS.md is canonical, no v2 suffix)
├── 2026.04.19-release-plan.md          ✗ dots in date format
└── docs/
    ├── AuthFlow.md                     ✗ camelCase in docs/
    ├── deployment_playbook.md          ✗ underscore
    └── zzz-archive/                    ✗ legacy prefix; should be _archive/ INSIDE docs/
```
