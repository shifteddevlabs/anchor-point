# Anchor Point

**A Documentation Architect for solo devs running multiple side projects.**

Drop this folder into a Claude project. Claude becomes the doc specialist. Knows where everything goes. Keeps your docs from getting messy.

---

## Who this is for

You bounce between a handful of projects. Every Claude session starts with you re-explaining what you're working on for ten minutes. Your docs are stale. They look different in every project. And Claude keeps re-discovering the same stuff you already wrote down somewhere.

Sound familiar? You don't need another tool. You need a system.

## What's in here

- A clean way to organize your docs. 6 main files at the root, plus a `docs/` folder for everything else.
- Naming rules that work the same way in every project (so you stop having "is it `auth.md` or `Auth.md` or `auth_flow.md`?" arguments with yourself).
- A checklist of the common ways docs go sideways, so you catch them early.
- Templates for each of the 6 main files. Drop-in, fill-in.
- Four modes for the project lifecycle: **Init** (new project), **Review** (start of session), **Update** (end of session), **Audit** (periodic cleanup).

## Quick start (under 5 minutes)

1. Drop this folder into a Claude Project (claude.ai → Projects → upload, or paste the files into the project's knowledge).
2. Start a chat in that project.
3. Try one of these to see it work:
   - *"Init docs for a new Next.js project called my-app"*
   - *"My project's CLAUDE.md is at /path. Review it and tell me what's drifting."*
   - *"Just finished a coding session. Help me write the handoff."*
   - *"My docs are a mess. What's the cleanup plan?"*

Claude will respond as a doc specialist and walk you through the right mode. That's it.

## What's in this folder

| File | What it's for |
|---|---|
| `identity.md` | Who the specialist is. Their background, what they're good at, what they won't touch. |
| `rules.md` | How they work. The hard rules, the 4 modes, naming, where stuff goes. |
| `examples.md` | A few example interactions. Shows you what good output looks like. |
| `reference/` | The full spec, naming rules, the drift checklist, the where-does-this-file-go decision tree, and the 5 templates. |
| `README.md` | This file. |

## The whole thing in one sentence

Every project gets the same 6 root docs (`README`, `CLAUDE.md`, `CONTEXT.md`, `SESSION-HANDOFF.md`, `ROADMAP.md`, `REFERENCES.md`), plus a `docs/` folder for everything else. Each file does one job. Anchor Point keeps you from drifting from that.

## Why the name

Docs drift. Across sessions, across projects, across AI tools. Same conventions get re-explained. Same gotchas get re-discovered. Same questions get re-asked.

An anchor point is the fixed thing everything else ties to. That's what this folder is, but for your docs.

## License

MIT. Take it, fork it, change it, ship your own.

## Built on the Interpretable Context Methodology

This follows the Clief Notes "folders as architecture" idea. Each file does one job. The structure tells you what's where. Someone with no context can clone it and use it cold.
