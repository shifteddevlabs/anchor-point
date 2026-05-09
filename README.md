# Anchor Point

**An AI Documentation Architect for solo developers running multiple projects.**

Drop this folder into a Claude project. Claude becomes a doc-system specialist who knows where every file goes, what every file is for, and how to keep your project's docs from drifting.

---

## Who this is for

You have several projects you switch between. You waste time at the start of every session re-explaining what you're working on. Your docs go stale, get inconsistent across projects, and AI agents waste cycles re-discovering the same conventions you already wrote down somewhere.

You don't need another tool. You need a system.

## What you get

Anchor Point gives Claude:

- A **7-bucket architecture** for project docs (6 root files + a `docs/` ecosystem)
- **Naming conventions** that work across every project
- An **anti-pattern catalog** (A1-A8) for detecting doc drift
- **Templates** for the 6 canonical root files
- **Four operating modes:** Bootstrap (new project), Review (start of session), Update (end of session), Audit (periodic deep alignment)

## Quick start (under 5 minutes)

1. **Drop this folder into a Claude Project** (claude.ai → Projects → upload folder, or paste the files into the project's knowledge)
2. **Start a chat in that project**
3. **Try one of these to see it in action:**
   - *"Bootstrap docs for a new Next.js project called my-app"*
   - *"My project's CLAUDE.md is at /path. Review it and tell me what's drifting."*
   - *"I just finished a coding session. Help me write the SESSION-HANDOFF entry."*
   - *"My docs/ folder is a mess. What's the cleanup plan?"*

Claude will respond as a doc-system specialist, walking you through the right operating mode.

## What's in this folder

| File | What it does |
|---|---|
| `identity.md` | Who the specialist is — background, strengths, what they don't cover |
| `rules.md` | How the specialist operates — hard rules, the 4 modes, naming conventions, when to write where |
| `examples.md` | Two example interactions showing the specialist in action |
| `reference/` | Source material: the 7-bucket architecture spec, anti-pattern catalog, naming conventions, canonical file set, and the 5 doc templates |
| `README.md` | This file |

## The methodology in one sentence

Every project needs the same 6 root docs (`README`, `CLAUDE.md`, `CONTEXT.md`, `SESSION-HANDOFF.md`, `ROADMAP.md`, `REFERENCES.md`) and a `docs/` folder for everything else. Each file has ONE job. Anchor Point keeps you from drifting from that.

## Why "Anchor Point"

Docs drift. Across sessions, across projects, across AI tools. The same conventions get re-explained. The same gotchas get re-discovered. The same questions get re-asked.

An anchor point is a fixed reference everything else ties to. That's what this folder is for your project docs.

## License

MIT. Use it, fork it, adapt it, ship your own variant.

## Built using the Interpretable Context Methodology

This folder follows the Clief Notes "folders as architecture" methodology: each file does one job; the structure tells you what's where; someone with no context can clone it and use it cold.
