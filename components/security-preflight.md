# Component: Security Preflight

## Purpose

Prevent credentials, secrets, and private business data from being copied into reusable docs or surfaced in summaries.

## Used By

- `doc-update` when docs or copied content changed
- `doc-audit` before structural moves
- `inventory and extract`
- `push` or code-review workflows as a deeper security gate

## Workflow

1. Run filename/path-level checks first.
2. In broad copy, inventory, or test-run scans, exclude private, dependency, generated, deployment, and VCS folders before including Markdown:
   - `.git/`
   - `.next/`
   - `.vercel/`
   - `.pnpm-store/`
   - `node_modules/`
   - `coverage/`
   - `dist/`
   - `build/`
   - `.venv/`
   - `venv/`
   - `zzz_private/`
   - `_private/`
   - `docs/_private/`
   - `zzz_archive/`
2a. **Exclude by PATTERN, not just canonical name.** The named directories above only catch exact canonical names — a project can carry ad hoc variants. Before any rsync/cp-based fixture duplication or test-run copy, also glob-exclude any directory or filename matching `*private*` or `*archive*`, and separately any filename matching `*secret*`/`*credential*`/`*key*`/`*token*`/`*template*` (e.g. `secrets_template.md`). After the copy, run a 4-dimension scan — credential filenames, protected dirs, secret/credential/key/token filename patterns, unexpected non-doc files — BEFORE reading any copied file. If a protected file is found post-copy: delete the fixture (`mv` it to a trash location — `rm -rf` is harness-gated), rebuild with the broadened excludes, and re-scan clean before proceeding (ties to Anchor Point Mode 6 rule 4). Origin: 2026-07 fixture-duplication incident — a copy op used an ad-hoc exclude list rather than this one, so quicksinq's `zzz_private/` financial docs and overdrive's `zzz_archive/.../secrets_template.md` were duplicated into a test-runs dir; adding `zzz_archive/` above and the pattern-exclude here closes the gap.
3. Look for candidate terms:
   - key
   - token
   - secret
   - private key
   - access token
   - API key
   - app secret
4. Run strict token-pattern checks separately from broad keyword checks.
   Include common service-specific runtime URL secrets such as monitoring DSN strings, not only API-key prefixes.
5. Treat broad keyword hits as filename-only candidates until inspection is necessary.
6. If a candidate file must be inspected, inspect only enough surrounding structure to classify it.
7. Never copy or quote secret values.
8. Add sensitive candidates to your shared security watchlist (path bound via `internal-overlay.md`; e.g., `+vantage-point/docs/security/sensitive-doc-watchlist.md` for vantage-point monorepos).
9. Recommend rotation if a live secret is committed, shared, or stored outside private ignored docs.

## Output

```text
Sensitive candidates:
Likely false positives:
Needs review:
Recommended remediation:
```

## Boundary

This is a documentation preflight, not a full code security review.

Full code security review belongs in Quality Security and the push/code-review workflow.
