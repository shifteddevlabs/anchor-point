# Anchor Point

**A model-agnostic documentation operating system for solo devs running multiple side projects.**

Current version: **v0.2.0**. See `CHANGELOG.md` for what changed.

Drop this folder into an AI project, repo, or knowledge base, then point tool-specific adapters at it. The agent becomes the doc specialist. Knows where everything goes. Keeps your docs from getting messy. Battle-tested, distilled from cross-project mining of real-world doc drift incidents.

---

## Who this is for

You bounce between a handful of projects. Every agent session starts with you re-explaining what you're working on for ten minutes. Your docs are stale. They look different in every project. And the model keeps re-discovering the same stuff you already wrote down somewhere.

Sound familiar? You don't need another tool. You need a system.

## What's in here

- A clean way to organize your docs. 6 main files at the root, plus a `docs/` folder for everything else.
- Naming rules that work the same way in every project (so you stop having "is it `auth.md` or `Auth.md` or `auth_flow.md`?" arguments with yourself).
- A checklist of the common ways docs go sideways, so you catch them early.
- Templates for each of the 6 main files. Drop-in, fill-in.
- Six workflows for the project lifecycle: **Init** (new project), **Review** (start of session), **Update** (end of session), **Audit** (periodic cleanup), **Ratchet** (duplicate-only improvement loop), and **Workflow Autoresearch** (hardening the doc system itself).

## Quick start (under 5 minutes)

1. Drop this folder into an AI project or repo knowledge folder (Claude Project, Codex skill folder, Cursor rules folder, or a plain docs repo).
2. Start a chat in that project.
3. Try one of these to see it work:
   - *"Init docs for a new Next.js project called my-app"*
   - *"My project's AGENTS.md is at /path. Review it and tell me what's drifting."*
   - *"Just finished a coding session. Help me write the handoff."*
   - *"My docs are a mess. What's the cleanup plan?"*

The agent will respond as a doc specialist and walk you through the right workflow. That's it.

## What's in this folder

| File | What it's for |
|---|---|
| `identity.md` | Who the specialist is. Their background, what they're good at, what they won't touch. |
| `rules.md` | How they work. The hard rules, the 6 workflows, naming, where stuff goes. |
| `examples.md` | A few example interactions. Shows you what good output looks like. |
| `reference/` | The full spec, naming rules, drift checklist, where-does-this-file-go decision tree, workflow scoring, ratchet/autoresearch specs, and templates. |
| `README.md` | This file. |

## Vocabulary

Use these names going forward:

| Term | Meaning |
|---|---|
| **Capability** | A repoed, model-agnostic package of docs, rules, routines, and references. Anchor Point is a capability. |
| **Workflow** | A user-facing process such as Init, Review, Update, Audit, Ratchet, or Workflow Autoresearch. |
| **Routine** | A chainable repeatable block inside a workflow, such as harvest, verify, snapshot, score, or security preflight. |
| **Component** | A reusable internal checklist, rubric, template, or scoring rule. |
| **Adapter** | A tool-specific wrapper, slash command, installed skill, prompt, or scheduler entry that points back to the capability. |
| **Overlay** | Project-specific context that stays local to the project instead of becoming a global rule. |

## Tool-Specific Adapters

`AGENTS.md` is the canonical AI bootstrap file. If a platform needs a different entrypoint, create a thin adapter that points back to `AGENTS.md` and this capability rather than forking the rules.

Examples: Claude Project knowledge, a `CLAUDE.md` shim, Codex installed skill metadata, Cursor rules, slash commands, or scheduled automations.

## The whole thing in one sentence

Every project gets the same 6 root docs (`README`, `AGENTS.md`, `CONTEXT.md`, `SESSION-HANDOFF.md`, `ROADMAP.md`, `REFERENCES.md`), plus a `docs/` folder for everything else. Each file does one job. Anchor Point keeps you from drifting from that.

## What your project looks like after Init

```
your-project/
├── README.md              ← the public face
├── AGENTS.md              ← rules + folder map (AI reads first every session)
├── CONTEXT.md             ← what we're working on right now (current phase)
├── SESSION-HANDOFF.md     ← what just shipped + what's next
├── ROADMAP.md             ← priorities + decision log
├── REFERENCES.md          ← topic-driven lookup ("where do I find X?")
└── docs/
    ├── DOCS-INDEX.md      ← location-driven file map (created when docs/ has >10 files)
    ├── history/   ← completed work, old handoffs, roadmap detail
    ├── reference/         ← verified-state files (config truths, capability matrices)
    ├── playbooks/         ← process runbooks ("when X happens, do Y")
    ├── dev/               ← architecture, feature design, schema docs
    ├── release/           ← release notes, migration steps
    ├── reviews/           ← code review reports, security audits
    ├── research/          ← experiments, exploration drops
    ├── _archive/          ← historical (dated batches with READMEs)
    └── _private/          ← gitignored sensitive content
```

Standard subfolders inside `docs/` get created on demand (Anchor Point handles it). You don't pre-create empty placeholders.

## The 6 workflows (lifecycle)

```
NEW PROJECT        →  Init        bootstrap all files
START OF SESSION   →  Review      read state, suggest next steps (read-only)
END OF SESSION     →  Update      handoff, roadmap sync, harvest, verify, snapshot
PERIODIC CLEANUP   →  Audit       deep alignment, file relocation, drift score
LOW SCORE / MESSY  →  Ratchet     duplicate-only cleanup loop, score, keep best
SYSTEM HARDENING   →  Autoresearch improve the workflows themselves from evidence
```

You don't memorize commands. Just talk naturally ("catch me up", "wrap this session", "my docs are messy") and the right workflow kicks in.

## Why the name

Docs drift. Across sessions, across projects, across AI tools. Same conventions get re-explained. Same gotchas get re-discovered. Same questions get re-asked.

An anchor point is the fixed thing everything else ties to. That's what this folder is, but for your docs.

## License

MIT. Take it, fork it, change it, ship your own.

## Built on the Interpretable Context Methodology

This follows the Clief Notes "folders as architecture" idea. Each file does one job. The structure tells you what's where. Someone with no context can clone it and use it cold.
