---
task: REK-1916 — ship profile-wizard key-collision + gating durability fix to staging, then finalise
date: 2026-06-29
prompts_used:
  - id: testing-status
    version: 1
  - id: ship-to-staging
    version: 1
---

## What went well

- [testing-status] Stage-detection + the AGENT-CLOUD/YOU/AGENT-DESKTOP actor tagging produced a crisp, genuinely actionable status. The "if unsure a cheap cloud check is still green, re-run it" instruction was right — re-running tsc/vitest/lint/detector + a fresh staging dry-run gave current signal instead of stale claims.
- [ship-to-staging] The finalisation's two strongest asks paid off: the failure-notice reconciliation requirement forced an explicit "no red marks left unexplained" audit, and "merged ≠ on staging — watch the post-merge run" correctly pushed me to confirm the Supabase-staging deploy job + staging E2E rather than stopping at the merge.

## Frictions / issues

- [ ] [ship-to-staging] The Finalisation section omits the **QA Notion Sign-Off** step, even though `qa-checklist.mdc` makes a completed Sign-Off the *precondition* for moving Linear to Ready to Deploy. The prompt jumps straight to "post proof comment → reconcile notices → set Ready to Deploy" and never says to update the QA page. I only caught the gap because the human nudged "I think there's something to do with notion… check the conventions." Without that nudge the QA artifact would have been left stale.
- [ ] [ship-to-staging] No "resume at finalisation" entry point. The prompt is written as a linear start→staging flow (sync main → open PR → green checks → merge → verify), but here it was pasted **after** the PR was already merged and deployed; the human had to explicitly say "look at the finalisation section." Re-running it as written re-asserts moot precondition/merge machinery for an already-shipped change.
- [ ] [ship-to-staging] Internal contradiction on merge authorization. Item 3 says "auto-merge to staging by default — do NOT wait for an explicit 'merge it'," but `pr-lifecycle.mdc` Phase 5 says merge only on explicit verbal authorization — and the prompt also states "if anything conflicts with the docs, the docs win." So the prompt's own default is self-defeating; an agent that follows it literally violates the repo guardrail. (In this run it didn't bite because the merge had already happened under explicit "go ahead," but a from-scratch run would hit it.)
- [ ] [testing-status] The actor model can't express **cross-actor sequencing**. A [YOU] test that only unblocks *after* an [AGENT-DESKTOP] setup step (here: the manual wizard walk needs the local stack the desktop prompt provisions, OR a staging deploy) had no clean home — it isn't purely "blocked" (G) nor immediately runnable (E), and I had to shoehorn the dependency into prose.

## Suggested improvements

- [ ] [ship-to-staging] Add the QA Notion Sign-Off to Finalisation as an explicit step gating the status change, e.g.: "Before setting Ready to Deploy, update the QA page Sign-Off per `qa-checklist.mdc` (fill the Tester/Date/Result/Notes row; tick verified items) — a completed Sign-Off is the precondition for Ready to Deploy." This removes reliance on a human nudge.
- [ ] [ship-to-staging] Add a stage-detect preamble mirroring testing-status: "Detect current stage first. If the branch is already merged and on staging, SKIP to Finalisation." Lets the prompt be safely (re-)invoked just for closeout.
- [ ] [ship-to-staging] Reconcile item 3 with `pr-lifecycle.mdc`: either change the default to "merge on explicit authorization (per pr-lifecycle Phase 5)," or, if auto-merge-to-staging is genuinely intended, state that it intentionally overrides Phase 5 *for staging only* and why — so the agent isn't left resolving a contradiction the prompt says the docs win.
- [ ] [testing-status] Let a test in section C/E carry a "prereq:" note (actor + step) so cross-actor dependencies are first-class, e.g. "[YOU] wizard walk — prereq: [AGENT-DESKTOP] local stack (F) OR staging deploy."
