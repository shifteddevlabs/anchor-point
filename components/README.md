# Anchor Point Components

Components are small reusable process blocks used by the main Anchor Point modes.

They are not meant to create command sprawl. A user can ask for `doc-update`, and that workflow can internally run `snapshot`, `verify`, `promote-memory`, `harvest`, and `security-preflight` as needed.

## Components

| Component | Purpose |
|---|---|
| `snapshot.md` | Capture what changed and the current project state |
| `verify.md` | Confirm volatile or high-risk claims before writing them as facts |
| `promote-memory.md` | Move durable learnings to the right memory layer |
| `harvest.md` | Extract reusable process from project-local work |
| `security-preflight.md` | Flag sensitive docs before quoting, moving, or extracting |
| `workflow-score.md` | Score the reusable workflow separately from a project's docs |

## Rule

Load components only when the active mode needs them.
