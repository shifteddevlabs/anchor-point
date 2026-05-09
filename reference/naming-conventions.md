# Naming Conventions

Universal naming rules across all projects. Anchor Point applies these in Bootstrap mode (at file creation), enforces them in Audit mode (renames violators), and detects them in Review mode (flags violations + suggests Audit).

---

## The rules

**Root-level files (the 6 canonical):** `ALL-CAPS-KEBAB.md`
- `README.md`, `CLAUDE.md`, `CONTEXT.md`, `SESSION-HANDOFF.md`, `ROADMAP.md`, `REFERENCES.md`
- Hyphens between words (no spaces, no underscores, no camelCase)
- Always `.md` extension

**`docs/` files:** `lowercase-kebab.md`
- `meta-app-capabilities.md`, `auth-flow-design.md`, `stripe-webhook-flow.md`
- Hyphens between words
- Always `.md` extension
- **Exception:** `docs/DOCS-INDEX.md` follows root convention (ALL-CAPS-KEBAB) because it functions as a top-level navigation file within `docs/`
- **Exception:** subfolder `README.md` files (e.g., `docs/handoff-history/README.md`, `docs/_archive/<batch>/ARCHIVE-README.md`) follow GitHub-standard ALL-CAPS naming

**Folders:** `lowercase-kebab/`
- `handoff-history/`, `reference/`, `playbooks/`, `dev/`, `release/`, `reviews/`, `research/`
- Hyphens between words
- **Exception:** sort-to-bottom folders use leading underscore: `_archive/`, `_private/`
- **Legacy:** old `zzz-archive/` and `zzz-private/` get migrated by Audit mode → `docs/_archive/` and `docs/_private/`

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

## Detection signatures (used by Audit mode)

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
├── CLAUDE.md
├── CONTEXT.md
├── SESSION-HANDOFF.md
├── ROADMAP.md
├── REFERENCES.md
└── docs/
    ├── DOCS-INDEX.md
    ├── handoff-history/
    │   ├── README.md
    │   └── 2026-04-19-session-25.md
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
├── claude.md                           ✗ should be CLAUDE.md
├── My Notes.md                         ✗ space + camelCase + at root
├── session_handoff.md                  ✗ underscore + lowercase
├── 2026.04.19-release-plan.md          ✗ dots in date format
└── docs/
    ├── AuthFlow.md                     ✗ camelCase in docs/
    ├── deployment_playbook.md          ✗ underscore
    └── zzz-archive/                    ✗ legacy prefix; should be _archive/ INSIDE docs/
```
