# New-Project Wiring (post-Init checklist)

After Mode 1 (Init) generates the 5 root files + docs/ ecosystem, a project in Jay-J's workspace needs 5 more wiring steps before the routing chain and enforcement work. Mode 1 is doc-only; this closes the gap (origin: 2026-06-11 review, ai-health-export-android had none of these).

1. **Inherits-from block** - append to the new `AGENTS.md`:
   > ## Inherits from
   > Workspace rules: `/Users/jay-j/ai-projects/+agent-ops/AGENTS.md` (canonical for all agent hosts). One-page preflight: `/Users/jay-j/ai-projects/+agent-ops/PREFLIGHT.md`. Read both before code or service work.
2. **Quality-gate block** - append to the AGENTS.md project rules:
   > **Quality gate:** never raw `git commit` / `git push`. Every commit goes through `/push` (`+agent-ops/skills/push/SKILL.md`).
3. **Infisical binding** - when the project needs project-scoped credentials, run `infisical init` in the project root or record its project ID in `AGENTS.md`. If the project has no project-scoped credentials yet, record that fact instead of inventing a vault binding. Org-wide credentials continue to use the explicit AgentOps Infisical command form.
4. **Tracked pre-commit hook** - copy the canonical `+agent-ops/.githooks/pre-commit` into the new repo at `.githooks/pre-commit`, preserve mode `100755`, copy `+agent-ops/scripts/install-hooks.sh` into the repo, and run the installed script once to set `core.hooksPath=.githooks`. Do not install an untracked `.git/hooks/pre-commit` copy. Also copy the Cursor denylist: `mkdir -p .cursor/rules && cp +agent-ops/scripts/cursor-credential-deny.mdc .cursor/rules/credential-deny.mdc`.
5. **Project overlay (only if needed)** - if cross-project agent facts exist that don't belong in the repo, create `+agent-ops/project-overlays/<project>/` and register the project in the workspace `AGENTS.md` Active Projects table.

Total: under 5 minutes. Skip the Git hook only for a non-Git folder. Skip Infisical binding only when the project has no project-scoped credentials, and say so in the Init summary.
