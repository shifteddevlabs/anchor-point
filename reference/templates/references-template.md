# References — {{PROJECT_NAME}}

Topic-driven lookup hub. Tables of pointers, not original content. For file-by-file inventory see `docs/DOCS-INDEX.md`.

---

## SOT registry

Verified-state source-of-truth files. Whenever a config, schema, or value would otherwise drift across docs, route here.

| Topic | File | Last verified |
|---|---|---|
| {{TOPIC_1}} | `docs/reference/{{file1}}.md` | {{YYYY-MM-DD}} |
| {{TOPIC_2}} | `docs/reference/{{file2}}.md` | {{YYYY-MM-DD}} |

---

## Playbooks index

Process runbooks. "When X happens, do Y."

| Topic | Playbook | When to run |
|---|---|---|
| {{PLAYBOOK_TOPIC_1}} | `docs/playbooks/{{playbook1}}.md` | {{TRIGGER_CONDITION}} |
| {{PLAYBOOK_TOPIC_2}} | `docs/playbooks/{{playbook2}}.md` | {{TRIGGER_CONDITION}} |

---

## API constraints

Integration-specific facts that don't change often but are easy to forget.

| Service | Constraint | Source |
|---|---|---|
| {{SERVICE_1}} | {{CONSTRAINT_1}} | {{DOC_LINK_OR_VERIFIED_DATE}} |

---

## External docs

Links to SDK / framework / vendor documentation.

| Tech | Doc | Notes |
|---|---|---|
| {{TECH_1}} | {{URL_1}} | {{VERSION_OR_NOTE}} |

---

## External infrastructure

GCP project IDs, billing IDs, dashboard URLs. **NEVER secrets.**

| Resource | Identifier | Owner / Notes |
|---|---|---|
| {{RESOURCE_1}} | {{ID_OR_URL}} | {{NOTES}} |

---

## Skill ecosystem

Which shared skills apply to this project (path bound via `{{LAYER_0_HOME}}`; e.g., `+vantage-point/skills/*`).

| Skill | Use for | Trigger |
|---|---|---|
| {{SKILL_1}} | {{USE_CASE}} | {{TRIGGER_PHRASE}} |

---

*REFERENCES.md is HOT — loaded every session. Keep it ≤ 150 lines. Single-owner discipline: persistent rules → `AGENTS.md`; session-volatile hazards → `SESSION-HANDOFF.md`; cross-project conventions → `{{LAYER_0_HOME}}` (replace with your Layer 0 path; e.g., `+vantage-point/AGENTS.md`) (inherited, never duplicated). File inventory → `docs/DOCS-INDEX.md` only (anti-pattern A10).*
