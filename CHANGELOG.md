# Changelog

## v3 — 2026-05-18

Versioning policy reset. Anchor Point now uses whole-number public versions instead of semantic or decimal-style versioning.

### Why

Anchor Point is a living methodology, not an API library. Decimal labels like `v3.4.1` created fake precision and made agents distinguish between structural spec versions, skill-driver naming patches, and historical migration labels. The user chose the simpler rule: current version is `v3`; the next accepted edit is `v4`, no matter how small.

### Changed

- Public/current labels now say **v3** instead of **v3.4.1**.
- `SKILL.md` frontmatter now uses `version: 3`.
- `reference/doc-architecture.md` now declares `version: 3` and titles the spec "Doc Architecture v3."
- `README.md` now documents the whole-number versioning policy.
- Legacy decimal history remains below for audit context only.

### Versioning rule going forward

- Every accepted edit increments the public version by one.
- The changelog explains whether the change was tiny, naming-only, structural, or a major rethink.
- Do not introduce patch/minor semantics back into Anchor Point unless the user explicitly reverses this policy.

## v3.4.1 — 2026-05-16

Naming-clarity patch. Renames Mode 6 and Mode 7 to make their targets explicit, after a session where the user (skill author) couldn't recall which mode did what.

### Why

Both modes are Karpathy-style ratchet loops, but the noun being ratcheted differs:
- Mode 6 ratchets **one project's docs** against the existing spec.
- Mode 7 ratchets **the spec itself** using evidence from Mode 6 runs.

Pre-v3.4.1 names ("Ratchet" / "Workflow Autoresearch") obscured that distinction. "Doc Ratchet" in particular gave no signal about which doc was being ratcheted (one project's vs the workflow's). The new names spell out the target.

### Changed

- `SKILL.md` Mode 6 heading: "Ratchet" → "**Project Ratchet**". One-sentence target clarification added.
- `SKILL.md` Mode 7 heading: "Workflow Autoresearch" → "**Doc Workflow Ratchet**". One-sentence target clarification added, including the Mode 6 → Mode 7 evidence flow.
- `SKILL.md` body line referring to "workflow-autoresearch mode" → "Doc Workflow Ratchet mode".
- `SKILL.md` Reference Docs table: descriptions for `reference/doc-ratchet.md` and `reference/workflow-autoresearch.md` updated to lead with the mode name.
- `SKILL.md` Component Chains table: `doc-ratchet` row renamed to `project-ratchet`.
- `SKILL.md` frontmatter triggers: added `project ratchet` and `doc workflow ratchet`; kept `doc ratchet` for backward compatibility (natural-language invocation is sticky).
- `README.md`: 6-workflows list, vocabulary table, and lifecycle ASCII updated to new names.
- `reference/doc-ratchet.md` title: "Doc Ratchet Workflow" → "Project Ratchet (Mode 6) — Reference". Lead-in paragraph updated; filename retained to avoid inbound-link churn.
- `reference/workflow-autoresearch.md` title: "Workflow Autoresearch" → "Doc Workflow Ratchet (Mode 7) — Reference". Lead-in paragraph updated; filename retained.

### Not changed

- Mode numbers (still Mode 6 and Mode 7). Anything referring to "Mode 6" or "Mode 7" remains valid.
- Mode workflows, rules, rubric, drift checks, hard gates — all spec content identical.
- Reference filenames (`reference/doc-ratchet.md`, `reference/workflow-autoresearch.md`) — kept to avoid breaking inbound links across `docs/_archive/`, `docs/inventories/`, test-run reports, and project AGENTS files.
- Trigger phrase `doc ratchet` — retained alongside the new triggers.

### Why patch, not minor

No spec change. No new rule. No new anti-pattern. Wording change in published workflow files only. Matches the v3.3.1 / v3.3.2 cleanup-patch pattern.

### Downstream sweep needed (Phase 7)

- `+vantage-point/AGENTS.md` mode list — done in this PR.
- `+vantage-point/README.md`, `program.md`, `STATUS.md`, `ROADMAP.md`, `routines/README.md`, `routines/doc-migration.md`, `docs/DOCS-INDEX.md`, `docs/README.md` — done in this PR.
- Private memory (`MEMORY.md`) — done in this PR.
- Project AGENTS files in other repos — none currently reference these mode names by their old form (verified by grep).

## v3.4.0 — 2026-05-16

Spec extension. Adds **A20 (Inherits-from wording drift)** as a deterministic Mode 4 / Mode 2 drift check, closing the propagation gap flagged in the v3.3.2 patch.

### Why

v3.3.2 strengthened the `agents-template.md` "Inherits from" wording (canonical destination summary instead of a narrow 3-item list), and its CHANGELOG explicitly noted: "If audit-time enforcement of template-wording drift is wanted, that's a v3.4 spec extension (add a Mode 4 drift check that compares each project's Inherits-from text against the canonical template wording)."

Without that check, projects migrated under the earlier wording (smg-ep, split-sheet, etc.) keep the narrow phrasing unless updated manually. v3.4 makes the drift visible to `/doc-audit` so future Mode 4 / Mode 2 runs flag it for rewrite.

### What A20 detects

A project AGENTS.md has a `## Inherits from` section, but its body fails ONE OR BOTH of:

- Bullet count ≥ 4 (lines starting with `- ` or `* `)
- Keyword coverage ≥ 5 of: `Routing-by-task`, `Hard Rules`, `Skills`, `Operating Agreements`, `Infrastructure`, `Connection-first`, `File naming` (case-insensitive substring)

**Severity:** soft warning at bullet count 3 or keyword coverage 4; hard fix below those.

### Why this is a minor bump, not a patch

A20 is a new anti-pattern code. The catalog grew (A1-A19 → A1-A20). Minor bump is appropriate; patches are wording fixes within an existing code.

### Changed

- `reference/drift-checks.md` — added A20 detection row after A11 (related: both Inherits-from anti-patterns) plus RT-12 routing test case.
- `reference/anti-patterns.md` — added A20 rationale section after A19; severity table row; "How Init pre-empts these" bullet.
- `reference/doc-architecture.md` — added A20 row to the anti-patterns table; bumped frontmatter version + supersedes; spec title v3.3 → v3.4; v3.4 row added to the Versioning table.

### Not changed

- No new mode. Mode 4 (Audit) detects A20 via the existing catalog files; same pattern v3.2 used to add A14-A18.
- No template change. The canonical wording in `reference/templates/agents-template.md` is what A20 compares against (unchanged since v3.3.2).
- No schema change. 5 root files + 9 docs/ subfolders unchanged.
- All v3.3 / v3.3.1 / v3.3.2 content intact.

### Propagation

Existing projects with narrow Inherits-from wording will be flagged by the next `/doc-audit` run. Per the standard Mode 4 atomic-approval pattern, the user approves each rewrite individually before changes apply.

## v3.3.2 — 2026-05-16

Template-only patch. The `agents-template.md` "Inherits from" section was understating what's at the Layer 0 home destination. The previous wording said "Hard Rules baseline + naming + routing conventions" — that sounds like 3 narrow things. The reality is the Layer 0 AGENTS file holds the canonical workspace operating system (routing, Hard Rules incl. credential injection, skills + operating agreements + infrastructure maps, connection-first rule).

### Why

An agent that reads a project's AGENTS.md "Inherits from" pointer needs to know that the destination is COMPREHENSIVE — otherwise they may not bother opening the workspace file before doing work. The risk in practice: agent reads project AGENTS, doesn't re-read workspace AGENTS, then violates a workspace Hard Rule (e.g., raw `git push` falling through to keychain instead of using Infisical injection per Rule #12).

The fix is a wording change, not duplication. The new wording labels what's at the destination so the agent knows it's worth opening, without restating the content.

### Changed

- `reference/templates/agents-template.md` Inherits-from section — replaced the narrow 3-item list with a bulleted summary of what's at the workspace AGENTS destination (Routing-by-task, Hard Rules baseline, Skills + Operating Agreements + Infrastructure maps, Connection-first rule, File naming conventions). Parameterization with `{{LAYER_0_HOME}}` preserved.

### Not changed

- The single-pointer model from project AGENTS to workspace AGENTS — unchanged. Still one pointer, still routes everything through the workspace hot file. v3.x layered model intact.
- No new rules, no new anti-patterns, no schema changes.
- All v3.3 content (Subfolder Discipline, A14-A19, worked examples) — unchanged.

### Propagation note

Existing v3.x-migrated projects do NOT automatically inherit this new wording — `/doc-audit` Mode 4 doesn't currently detect "Inherits-from wording drift" as an anti-pattern. Manual updates to existing projects' AGENTS.md (e.g., shifteddevlabs/smg-ep#TBD, shifteddevlabs/split-sheet#TBD) accompany this template patch. Future `/doc-init` greenfield bootstraps (or refresh-mode runs) will pick up the new wording from the template.

If audit-time enforcement of template-wording drift is wanted, that's a v3.4 spec extension (add a Mode 4 drift check that compares each project's Inherits-from text against the canonical template wording). Not blocking for v3.3.2.

## v3.3.1 — 2026-05-16

Cleanup-only patch. Propagates the v3.3 ONE-hot-file principle into 4 published-skill workflow files that still pointed at `ROUTER.md` as the default place to update routing after extraction/promotion. Updated to point at Layer 0 AGENTS.md "Routing-by-task" section, with ROUTER.md noted as the Rule-5-justified exception (workspace-only, all thresholds met).

### Changed

- `SKILL.md` Mode 5 step 12 — was "Update `ROUTER.md` only after the reusable target exists." Now: "Update the Layer 0 home's `AGENTS.md` 'Routing-by-task' section (or, in the rare case a workspace ROUTER.md is justified per Rule 5, that file) only after the reusable target exists."
- `components/harvest.md` step 6 — same pattern, updated to point at AGENTS.md as default.
- `components/promote-memory.md` routing chart — final step was "→ ROUTER.md"; now "→ Layer 0 AGENTS.md 'Routing-by-task' section".
- `reference/templates/agents-template.md` Inherits-from section — was "Routing conventions (see `{{LAYER_0_HOME}}/ROUTER.md`)"; now points at AGENTS.md "Routing-by-task" with ROUTER.md as the Rule-5-justified exception.

### Why this is a patch, not a minor bump

No new rules. No new anti-patterns. No schema changes. The v3.3 principle was already correct; v3.3.1 just propagates it consistently to the workflow files that operationalize it.

### Not changed

- All v3.3 spec content (Rules 1-6, A14-A19, worked examples) — unchanged
- AGENTS.md sections, 5 root files, 9 docs/ subfolders — unchanged
- Token tiers, anti-patterns scoring — unchanged

## v3.3.0 — 2026-05-16

v3.3 promotes the foundational principle of the hot-warm split — "ONE hot file" — to first-class status in the spec, and adds A19 to detect overlapping hot files in general (the broader case beyond v3.2's workspace-specific Rule 6). Driven by the architectural reset that surfaced when implementing the v3.2 vantage-point cleanup: the question "should we keep ROUTER.md or fold it into AGENTS.md?" exposed that v3.2 had stated the rule implicitly (via Rule 4 hot-vs-warm test) but never elevated it to a principle.

### Added

- **Foundational principle "ONE hot file"** at the head of the Subfolder Discipline section in `reference/doc-architecture.md`. Reads as the principle Rules 1-6 derive from rather than a derived rule itself.
- **A19: Two hot files hold overlapping content at the same level.** Generalizes beyond ROUTER.md to cover any future variant (separate `gotchas.md`, separate `rules.md`, etc.). Rationale + fix + worked-example pointer in `reference/anti-patterns.md`; one-line row in the doc-architecture anti-patterns table.
- **Vantage-point worked example** in `reference/doc-architecture.md` appendix. Demonstrates A19 collapse: 2 hot files (92 + 76 lines, 3-hop boot) → 1 hot file (~165 lines, 2-hop boot). Companion to the v3.2 overdrive-lab worked example (different problem class — that was over-cap single file; this is two-files-at-same-level).

### Sharpened

- **Rule 5 thresholds** rewritten from 3 conditions to 5 concrete AND conditions, with explicit ordering (warm extraction must come first, then check routing-table size + task families). Removes the previous "rare" language in favor of a hard checklist.
- **A18 cross-reference to A19.** A18 retains its specificity (project-level ROUTER.md without justification); A19 covers the general case. The two anti-patterns reference each other so audit detection picks up both narrow and broad cases.
- **Workspace exemption language.** Workspace roots are no longer "automatically exempt" from project-level rules — same thresholds apply, scale just makes them easier to meet. The vantage-point case (workspace with only 92+76 lines, well under threshold) proved the previous blanket exemption was wrong.

### Why elevated to a principle

v3.2 had Rule 4 (hot-vs-warm test for AGENTS.md sizing) and Rule 6 (workspace cheat-sheet ↔ ROUTER deduplication). Both implied "one hot file" without saying it. When evaluating the vantage-point ROUTER cleanup, the question "should we keep ROUTER for a smaller AGENTS, or fold it in?" surfaced that an agent reading v3.2 could still arrive at "two hot files might be better for size." That ambiguity is what v3.3 closes. The principle is now the lead, with Rules 1-6 (and A19) presented as direct applications.

### Not changed

- Hot/warm test from v3.2 Rule 4 — unchanged
- 5 root files, 9 docs/ subfolders, 8 AGENTS.md sections — unchanged
- 200-line AGENTS.md cap — unchanged
- Gotchas-stay-hot guidance from v3.2 — unchanged

### Migration impact

- **vantage-point ROUTER.md collapse** — this is the worked example. Tracked as a separate PR (`+vantage-point/` workspace repo).
- **Other projects** — no project currently has two hot files (verified: smg-ep, split-sheet, ai-health-export, overdrive-lab, quicksinq all have only AGENTS.md as hot). A19 protects against future drift.

## v3.2.0 — 2026-05-16

v3.2 codifies how `docs/` subfolders, DOCS-INDEX.md, and CONTEXT.md naming interact. Origin: 2026-05-16 evaluation against Jake Van Cleef's CONTEXT.md proposal and Codex's progressive-disclosure framing surfaced three unaddressed gaps in v3.1's subfolder shape. No root file changes; v3.2 is purely additive rules + 5 new anti-patterns + a worked example.

### Added

- **Subfolder discipline section (6 rules):**
  - **Rule 1:** Subfolder READMEs contain Purpose + What goes here + Out of scope only. No file lists (DOCS-INDEX owns inventory). Skip the README if <3 files.
  - **Rule 2:** DOCS-INDEX.md is location-only. No Quick Start / task-routing table (AGENTS.md §3 owns that).
  - **Rule 3:** CONTEXT.md is reserved for agent-only working folders that a parent file explicitly routes to. Not a default file type. README.md is the convention for human-visible folders.
  - **Rule 4:** AGENTS.md sizing uses a hot-vs-warm test. HOT sections (Identity, Current Phase, Routing, Where files go, Project Rules, **Gotchas**, Inherits-from) stay regardless of length — gotchas are proactive warnings the agent must absorb at session start. WARM extracts first (Tech Stack → docs/reference/, code-pattern sections → docs/dev/, file-structure → docs/dev/file-structure.md).
  - **Rule 5:** Project-level ROUTER.md split is justified only when routing table >25 rows AND ≥3 distinct task families AND warm-content extraction is exhausted. Workspace roots are exempt.
  - **Rule 6:** Workspace cheat-sheet ↔ ROUTER.md must not duplicate task→load routing.
- **Five new anti-patterns:** A14 (subfolder README lists files), A15 (CONTEXT.md in human folder), A16 (patterns in AGENTS.md), A17 (DOCS-INDEX has routing table), A18 (premature ROUTER.md split).
- **Worked-example appendix** in `reference/doc-architecture.md`: extracting an over-cap AGENTS.md using overdrive-lab (552 lines → projected ~160-180) as the case. Includes "what NOT to do" — explicitly: do not move Gotchas to reference, do not split routing prematurely.

### Why gotchas stay hot (the design moment that shaped v3.2)

An early draft of Rule 4 proposed extracting Gotchas to `docs/reference/gotchas.md`. That was wrong: the agent doesn't open `gotchas.md` proactively because they don't know they need to. Gotchas only function if they're in context at session start so the agent recognizes the hazard before stepping on it. v3.2's hot-vs-warm test is the explicit framework that produces the right answer here.

### Not changed

- 5 root files (README, AGENTS, STATUS, ROADMAP, LOOKUP) — same shape
- 9 docs/ subfolders at bootstrap — same set
- 8 AGENTS.md sections — same schema
- AGENTS.md 200-line cap — same number; Rule 4 governs how to stay under it
- Cold-start token budget — still ~4.5K when projects conform

### Migration impact

- **smg-ep** and **split-sheet** (the two projects already on v3.1): need subfolder README trim (Rule 1) + DOCS-INDEX Quick Start strip (Rule 2). Small surface cleanup; no structural changes.
- **overdrive-lab** (552-line AGENTS.md): the worked-example appendix in `reference/doc-architecture.md` is its extraction recipe. Track as separate task.
- **quicksinq** (12-line stub): complete the v3.0/3.1 migration first; v3.2 rules apply naturally.
- **ai-health-export, essential, lyric-sheet** (not yet on v3.x): bootstrap directly to v3.2; the Init template will already conform.

### Workspace-level follow-up flagged

`+vantage-point/AGENTS.md` "Common Tasks Cheat Sheet" overlaps with `+vantage-point/ROUTER.md`. Rule 6 mandates resolution. Tracked as a separate task; not handled in this spec patch.

## v3.1.0 — 2026-05-15

Anchor Point v3.x is a foundational shape change. v1.x described a 6-root-file system (`README`, `CLAUDE`, `CONTEXT`, `SESSION-HANDOFF`, `ROADMAP`, `REFERENCES`); v3.x collapses that to 5 (`README`, `AGENTS`, `STATUS`, `ROADMAP`, `LOOKUP`) and treats session handoff as a pure-function operation. v3.1 is a refinement of v3.0 with one additive bucket and a packaging merger.

### Changed (v1.x → v3.0, also in v3.1)

- **5 root files instead of 6.** `CONTEXT.md` is absorbed into `AGENTS.md` as a "Current Phase" section. `SESSION-HANDOFF.md` renamed to `STATUS.md`. `REFERENCES.md` renamed to `LOOKUP.md`. `CLAUDE.md` becomes a 1-line stub pointing to `AGENTS.md`.
- **`AGENTS.md` is the canonical AI bootstrap.** Eight fixed sections in a fixed order: Identity, Current Phase, Routing-by-task, Where new files go, Project Rules, Tech Stack, Gotchas, Inherits from.
- **Routing-by-task table holds direct file paths.** Eliminates the v1.x "look up which SOT file to consult" intermediate hop.
- **`doc-update` is a pure function.** Given the same inputs (git log, file diffs, pre-routed Asks), it produces a byte-identical `STATUS.md`. No accumulated state, no judgment calls at handoff time.
- **Pre-routed Asks.** Mid-session asks are tagged with their destination home (`[AGENTS.md: never log raw HealthKit data]`) at write-time, not deferred to session end.
- **Decisions get their own folder.** `docs/decisions/<YYYY-MM-DD-topic>.md` per decision; ROADMAP keeps a 1-line pointer.
- **Layer 0 inheritance.** Cross-project Hard Rules live ONCE at a Layer 0 home (e.g., a monorepo's shared rules file); project AGENTS.md files use `## Inherits from` instead of duplicating the baseline. Workspace-specific binding lives in a gitignored `internal-overlay.md`.

### Added in v3.0

- **Three new anti-patterns:** A11 (Layer 0 duplication), A12 (Asks pile up without routing tags), A13 (handoff output drifts on rerun).
- **Single-owner facts via imperative/declarative test.** Sentence form determines home: imperative → Project Rules; declarative → Tech Stack or reference; conditional → Debugging Playbook.

### Added in v3.1

- **`docs/roadmap-history/` bucket.** Parallel to `status-history/` for rotated ROADMAP snapshots when ROADMAP exceeds 300 lines.
- **Expanded MG2 to a two-part fix:** ROADMAP > 300 lines now requires both (a) extracting Decision Log entries to `docs/decisions/` AND (b) rotating completed-priority sections to `docs/roadmap-history/`.
- **9 canonical `docs/` subfolders created at bootstrap with README stubs.** Empty folders carry signal (the README defines purpose), not noise. Resolves a v3.0 spec contradiction between "all at bootstrap" and "lazy on-demand."
- **Packaging merger.** The previously-separate `doc-system` workflow capability was merged into `anchor-point`. Anchor Point now owns all 7 modes (Init, Review, Update, Audit, Inventory & Extract, Ratchet, Workflow Autoresearch) instead of pointing at a separate spec.

### Migrated

The v1.x → v3.0 migration is automatable. `doc-audit` Mode 4 detects all v1.x patterns and proposes the atomic migration. `CONTEXT.md` → `AGENTS.md` §2, `SESSION-HANDOFF.md` → `STATUS.md` (with A9 priority-list cleanup), `REFERENCES.md` → `LOOKUP.md` (with A10 file-inventory strip), Decision Log entries → `docs/decisions/`.

### Token cost

Cold start dropped from ~8-10K tokens (v1.2) to ~4.5K tokens (v3.x): routing tables pre-load all paths so subsequent file lookups are direct.

## v1.1.0 — 2026-05-10

This release hardens Anchor Point from a Claude-centered documentation specialist into a repoed, model-agnostic documentation capability.

### Hardened

- Made `AGENTS.md` the canonical AI bootstrap file.
- Reframed tool-specific files (`CLAUDE.md`, slash commands, Codex skills, Cursor rules, schedulers) as adapters that point back to the capability.
- Expanded the lifecycle from 4 modes to 6 workflows: Init, Review, Update, Audit, Ratchet, Workflow Autoresearch.
- Added duplicate-only doc ratchet rules so messy projects are tested on fixtures before real project edits.
- Added workflow-autoresearch rules for improving the doc system itself from multiple project runs.
- Added a workflow score rubric to judge fixture safety, scoring integrity, context preservation, generalization, rule promotion, real-project readiness, and automation readiness.
- Expanded the drift catalog from A1-A8 to A1-A12.
- Added explicit checks for private-path dependency, product/content Markdown misclassification, archive/history link breakage, and ROADMAP-as-archive drift.
- Changed `docs/history/` semantics from rotated handoff backups to completed work, old handoffs, and retired roadmap detail.
- Added model-agnostic vocabulary: capability, workflow, routine, component, adapter, overlay.

### Versioning Notes

- Historical note: the early package used decimal labels such as `v1.0.x`, `v1.1.0`, and `v3.4.1`.
- Superseded on 2026-05-18 by the whole-number public policy introduced in `v3`.
- Future accepted edits increment by one public version (`v4`, `v5`, `v6`), regardless of whether the change is small or large.
