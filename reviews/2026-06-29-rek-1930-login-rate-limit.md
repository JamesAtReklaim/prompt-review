---
task: REK-1930 — improve login OTP rate-limit message (truthful live cooldown countdown)
date: 2026-06-29
prompts_used:
  - id: redesign
    version: 1
  - id: testing-status
    version: 1
  - id: ship-to-staging
    version: 1
---

## What went well

- [redesign] The "no design exists → use judgment + match the design language" branch handled a task that was really a behavior/copy change (no mockup), so the prompt didn't force a phantom visual-fidelity exercise. The COPY-TRUTH guardrail directly shaped the right call (truthful countdown, not a fabricated one).
- [redesign] The MISSING-BACKEND rule forced an explicit, surfaced judgment instead of a silent placeholder — the agent distinguished "surface an already-tracked value" from "fabricate missing data" and flagged it, which is exactly the intended behavior.
- [ship-to-staging] The auth-surface risk-pause + "merge to staging is reversible, prod is separate" framing produced a clean stop-and-confirm at the right moment, and the hard-stop-at-Ready-to-Deploy kept scope contained.
- [ship-to-staging] The Linear proof flow (upload bytes → embed assetUrl → verify render) and the failure-notice reconciliation step gave a tidy, auditable close-out.

## Frictions / issues

- [ ] [ship-to-staging] The prescribed babysitting command `gh pr checks <N>` **failed with 403** ("Resource not accessible by personal access token"), as did the REST `commits/<sha>/check-runs` endpoint — the repo PAT lacks `checks:read`. Had to fall back to `gh run list` / `gh run watch` and the GitHub MCP `get_status`. The prompt presents `gh pr checks` as the canonical check-rollup tool without a documented fallback for restricted tokens.
- [ ] [ship-to-staging] `gh run watch <id> --exit-status` repeatedly **returned a non-zero exit while the run was still `in_progress`** (dropped connection), i.e. a false "failure" signal. This happened 2–3×; each time the real status was still running/eventually-success. The prompt's "watch to completion" guidance implicitly trusts the watch exit code.
- [ ] [ship-to-staging] The Copilot two-round gate turned into **6 rounds**, several of which were trivial doc-nits *introduced by the previous fix* (e.g. a JSDoc block ending up above the wrong function after a refactor, then re-flagged the next round). Real bugs surfaced too (cross-origin `Retry-After` not exposed), but the cosmetic-nit churn was a large time sink with no stop rule for diminishing-value rounds.
- [ ] [testing-status] The `[AGENT-CLOUD]` actor definition states it "has repo, node, deno" — but **deno was not installed** in this cloud environment (`command -v deno` → not found). This caused edge-function/deno checks to be initially mis-classified as runnable-now; they only became runnable after a manual `deno` install later.
- [ ] [redesign] Phase 4's "load the ACTUAL changed screen … VISUAL DIFF" assumes the target state is directly reachable. Here the state was **gated behind a backend/transient condition** (a 3-per-15-min, per-IP 429) that can't be triggered on demand locally — the agent had to inject/stub the 429 to render it. The prompt has no guidance for verifying states you can't reach by normal interaction.
- [ ] [redesign] The MISSING-BACKEND rule ("do NOT add backend yourself unless the task explicitly includes it or the user approves") **nearly conflicted with the core requirement**: a truthful countdown was impossible without a tiny additive backend change (surfacing the already-tracked remaining window). The boundary between "forbidden fabrication/new mechanism" and "allowed plumbing of an existing value" had to be reasoned out from scratch.

## Suggested improvements

- [ ] [ship-to-staging] Add a "monitoring fallback" note: if `gh pr checks`/check-runs return 403 (token without `checks:read`), use `gh run list --branch <b>` + `gh run watch <run-id>` and/or the GitHub MCP `get_status`; and explicitly warn that **`gh run watch` can exit non-zero on a dropped connection — re-query `gh run view <id> --json status,conclusion` before treating it as a failure.**
- [ ] [ship-to-staging] Add a stop rule for review iteration: after substantive findings are resolved and a round yields only **cosmetic/doc nits or nits re-introduced by the prior fix**, batch-resolve and stop rather than looping; treat "two consecutive rounds with no *substantive* findings" as the gate, not "zero comments."
- [ ] [testing-status] Correct the `[AGENT-CLOUD]` capability list — don't assert `deno` is present; say "verify tool availability (node/deno/etc.) and install if missing," since deno was absent here.
- [ ] [redesign] Add a Phase 4 clause for **backend-/condition-gated UI states**: it's acceptable (and expected) to inject/stub the triggering response to render and screenshot the state, clearly labeling the artifact as "state injected; real path verified separately."
- [ ] [redesign] Clarify the MISSING-BACKEND rule's boundary: surfacing an **already-tracked value** through an existing mechanism (additive, non-breaking) is *not* the forbidden "fabricated stand-in," and is allowed when it's the only truthful way to meet an explicit requirement — still flag it for sign-off. Give the rate-limit `retryAfter` case as the worked example.
