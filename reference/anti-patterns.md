# Anti-Patterns — Doc Architecture Drift Catalog (A1-A8)

Eight drift signatures that indicate the doc architecture has slipped. Each has a detection rule and a fix. Anchor Point uses this catalog in Review mode (detection) and Audit mode (scoring + fixing).

---

## A1. Duplicate "rules" sections

**Signature:** A section labeled "Important behavioral reminders," "Hard rules," or "Don't do this" appears in TWO root files (e.g., both CLAUDE.md and REFERENCES.md).

**Most common version:** CLAUDE.md "Important Notes" + REFERENCES.md "Important behavioral reminders" — REFERENCES.md sometimes self-admits the duplicate.

**Detection:** grep root files for `^## (Important|Behavioral|Rules|Don't|Hard rules)` headings; if 2+ files match, flag.

**Fix:** Keep in CLAUDE.md only. Delete the duplicate. Don't even leave a "For rules, see CLAUDE.md" pointer — that's also a form of duplication that creates the impression of two homes for rules.

---

## A2. Architectural docs hiding in SESSION-HANDOFF.md

**Signature:** SESSION-HANDOFF.md contains H2/H3 sections like "X Feature Architecture", "Y Routing Flow", "Z Database Schema" — content that's stable, not session-volatile.

**Why it happens:** Engineer wrote the architecture during a session, dropped it in the handoff doc, never moved it.

**Detection:** SESSION-HANDOFF.md sections matching `(Architecture|Schema|Flow|Diagram|Design)` patterns; especially flag if section is >40 lines.

**Fix:** Move to `docs/dev/<feature>.md` or `docs/release/<feature>.md`. Add pointer from SESSION-HANDOFF.md if still needed.

---

## A3. Session-volatile content in CONTEXT.md

**Signature:** CONTEXT.md "What we are building" mentions specific session numbers, recent commit hashes, or "just shipped X" language.

**Why it matters:** CONTEXT.md is for phase-level state ("we're shipping v2.0"). Session-volatile content drifts in days.

**Detection:** CONTEXT.md content matching `(session \d+|commit [a-f0-9]+|just shipped|yesterday|today)`.

**Fix:** Strip to phase-level only ("we're shipping v2.0" not "Session 34 just fixed the chat regression"). Move volatile content to SESSION-HANDOFF.md.

---

## A4. SESSION-HANDOFF.md > 200 lines

**Signature:** Live handoff has accumulated multiple session-log entries.

**Detection:** Line count of `SESSION-HANDOFF.md` exceeds 200.

**Fix:** Rotate sessions older than 2 weeks → `docs/handoff-history/<YYYY-MM-DD>-session-<NN>.md`. Update the handoff-history index README.

**Soft warning:** at >150 lines, suggest rotation. **Hard fix:** at >200 lines, rotate.

---

## A5. Stale tech-stack claims

**Signature:** README.md or CLAUDE.md tech-stack table lists a version number that doesn't match what's actually in code.

**Detection:** grep code for the actual version (e.g. `"next": "16.1.5"` in `package.json`) and compare against the claimed value in docs.

**Fix:** Update both the docs and the source-of-truth references. Treat docs as DOWNSTREAM of code, not the other way around.

---

## A6. Pointer-only files (no original content)

**Signature:** A root file is 100% pointers to other docs with zero original content of its own.

**Likely culprits:** CLAUDE.md missing rules, CONTEXT.md missing the current phase, ROADMAP.md missing actual priorities.

**Detection:** File is <30 lines AND >70% of non-blank lines start with bullet/link patterns.

**Fix:** Add the missing original content. Top-level files should SUMMARIZE before pointing. A pointer-only file means the summary work was skipped.

---

## A7. Detail leakage into top-level

**Signature:** ROADMAP.md item has 200+ words of implementation detail inline.

**Why it matters:** Top-level files are summaries + pointers. Detail belongs in the leaf docs (`docs/release/<item>.md`, `docs/dev/<item>.md`).

**Detection:** ROADMAP.md item text exceeds ~300 words; or a single bullet item exceeds ~50 words.

**Fix:** Extract detail to `docs/release/<item>.md` or `docs/dev/<item>.md`. Replace with one-line summary + pointer.

---

## A8. Hypothesis presented as fact

**Signature:** A doc states X causes Y when only correlation has been observed. Look for "because" / "due to" / "this is why" without a citation, test result, or "verified by" pointer.

**Why it happens:** Pattern-matching from training data + a tidy correlation + the urge to give a confident answer.

**Canonical incident:** A team noticed Facebook posts published from their app got 0-9 reach while manual posts got 500+. They claimed "Dev mode silently throttles reach" as fact in their docs. Meta's docs do not confirm it. Burned 4 hours debugging the metrics parser when the metrics were correct and the cause was unproven.

**Fix:** Label the claim as "Leading hypothesis (not confirmed by docs)" and add a verification path ("test by publishing app + comparing 48h later"). Promote to fact only after the test runs.

---

## Severity + scoring

When Audit mode scores doc health (100-point system), each anti-pattern affects different rubric dimensions:

| Anti-pattern | Primary impact |
|---|---|
| A1 (duplicate rules) | Scope discipline (-) |
| A2 (architecture in SESSION-HANDOFF) | Scope discipline (-), Concision (-) |
| A3 (session-volatile in CONTEXT) | Scope discipline (-), Anti-fabrication (-) |
| A4 (handoff > 200 lines) | Concision (-) |
| A5 (stale tech-stack) | Specificity (-), Anti-fabrication (-) |
| A6 (pointer-only file) | AI-actionability (-), Specificity (-) |
| A7 (detail in top-level) | Scope discipline (-), Concision (-) |
| A8 (hypothesis as fact) | Anti-fabrication (-), Trustworthiness (-) |

---

## How Init mode pre-empts A1-A8

When generating the initial 6-file doc set, Anchor Point AVOIDS creating A1-A8 patterns from day one:

- **A1:** put all hard rules in CLAUDE.md only; never duplicate in REFERENCES.md
- **A2:** SESSION-HANDOFF.md skeleton has only Current State + Last Session + Next Session sections
- **A3:** CONTEXT.md mentions only phase-level state, not session-specific events
- **A4:** Generated SESSION-HANDOFF.md is short (~30-50 lines) by design
- **A5:** Tech-stack table only includes versions confirmed by reading manifest files directly
- **A6:** Each file has summary content, not just pointers
- **A7:** Generated ROADMAP.md items are concise summaries
- **A8:** Generated content cites sources for any cause-effect claim
