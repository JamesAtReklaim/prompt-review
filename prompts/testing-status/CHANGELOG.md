# Changelog — testing-status

## v4 — 2026-06-30
- Section F now requires the desktop hand-off prompt to **end by emitting a single paste-able return prompt addressed to the cloud agent** (results table per step, env notes, what it couldn't do), so the human only ferries prompts both ways (review: 2026-06-30-rek-1924-survey-deeplinks.md, friction: "The desktop (section F) hand-off is one-directional ... The human ended up manually translating desktop results back into chat").
- Section F now tells the desktop agent to **NOT request or block on secrets/keys** — run only what works without them, skip + hand back anything needing keys/human eyes — and requires the cloud agent to **name the [YOU]-manual tests explicitly and up front** before the hand-off (review: 2026-06-30-rek-1924-survey-deeplinks.md, friction: "The desktop agent chased unavailable secrets instead of skipping ... the cloud agent did not pre-declare up front which tests are [YOU]-manual").
- Added a required report shape (new lead section): a one-sentence headline + a glanceable `Risk | Failure-chance | Overall` table, with a severity legend anchored on irreversibility/production-impact (not blast radius) and `Overall = Risk × Failure-chance` (review: 2026-06-30-rek-1924-survey-deeplinks.md, friction: "No required risk-communication format ... The prompt doesn't specify how to express per-test risk, so it took several turns to converge").

## v3 — 2026-06-29
- Added a cross-actor sequencing convention: a test that only unblocks after another actor's step carries a `prereq: <actor> <step>` note, rather than being forced into "blocked" (G) or "runnable" (D/E) (review: 2026-06-29-ship-to-staging-finalisation-gaps.md, friction: "The actor model can't express cross-actor sequencing. A [YOU] test that only unblocks after an [AGENT-DESKTOP] setup step ... had no clean home ... I had to shoehorn the dependency into prose").

## v2 — 2026-06-29
- Corrected the [AGENT-CLOUD] capability list: don't assert `deno` is present — verify tool availability (e.g. node/deno via `command -v`) and install any missing one before relying on it (review: 2026-06-29-rek-1930-login-rate-limit.md, friction: "The [AGENT-CLOUD] actor definition states it 'has repo, node, deno' — but deno was not installed in this cloud environment ... caused edge-function/deno checks to be initially mis-classified as runnable-now").

## v1 — 2026-06-26
- Initial version (re-runnable testing status & next-actions report: detect current stage, bucket outstanding tests by actor [AGENT-CLOUD]/[YOU]/[AGENT-DESKTOP]; no product-code changes).
