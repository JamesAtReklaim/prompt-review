# Changelog — testing-status

## v3 — 2026-06-29
- Added a cross-actor sequencing convention: a test that only unblocks after another actor's step carries a `prereq: <actor> <step>` note, rather than being forced into "blocked" (G) or "runnable" (D/E) (review: 2026-06-29-ship-to-staging-finalisation-gaps.md, friction: "The actor model can't express cross-actor sequencing. A [YOU] test that only unblocks after an [AGENT-DESKTOP] setup step ... had no clean home ... I had to shoehorn the dependency into prose").

## v2 — 2026-06-29
- Corrected the [AGENT-CLOUD] capability list: don't assert `deno` is present — verify tool availability (e.g. node/deno via `command -v`) and install any missing one before relying on it (review: 2026-06-29-rek-1930-login-rate-limit.md, friction: "The [AGENT-CLOUD] actor definition states it 'has repo, node, deno' — but deno was not installed in this cloud environment ... caused edge-function/deno checks to be initially mis-classified as runnable-now").

## v1 — 2026-06-26
- Initial version (re-runnable testing status & next-actions report: detect current stage, bucket outstanding tests by actor [AGENT-CLOUD]/[YOU]/[AGENT-DESKTOP]; no product-code changes).
