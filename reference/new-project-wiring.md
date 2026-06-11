# New-Project Wiring (post-Init checklist)

After Mode 1 (Init) generates the 5 root files + docs/ ecosystem, a project in Jay-J's workspace needs 5 more wiring steps before the routing chain and enforcement work. Mode 1 is doc-only; this closes the gap (origin: 2026-06-11 review — ai-health-export-android had none of these).

1. **Inherits-from block** — append to the new `AGENTS.md`:
   > ## Inherits from
   > Workspace rules: `/Users/jay-j/Desktop/ai-projects/+agent-ops/AGENTS.md` (19 Hard Rules — canonical for all agent hosts). One-page preflight: `+agent-ops/PREFLIGHT.md`. Read both before code or service work.
2. **Quality-gate block** — append to the AGENTS.md project rules:
   > **Quality gate:** never raw `git commit` / `git push`. Every commit goes through `/push` (`+agent-ops/skills/push/SKILL.md`).
3. **Infisical binding** — `infisical init` in the project root (binds the folder to its Infisical workspace so `infisical run` resolves project secrets without flags), or note the project's projectId in its AGENTS.md.
4. **Pre-commit hook** — `cp +agent-ops/scripts/pre-commit .git/hooks/pre-commit && chmod +x .git/hooks/pre-commit` (blocks staged secrets for EVERY agent host + manual commits). Also copy the Cursor denylist: `mkdir -p .cursor/rules && cp +agent-ops/scripts/cursor-credential-deny.mdc .cursor/rules/credential-deny.mdc`.
5. **Project overlay (only if needed)** — if cross-project agent facts exist that don't belong in the repo, create `+agent-ops/project-overlays/<project>/` and register the project in the workspace `AGENTS.md` Active Projects table.

Total: under 5 minutes. Skip steps 3-4 for non-git/doc-only folders, but say so in the Init summary.
