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
