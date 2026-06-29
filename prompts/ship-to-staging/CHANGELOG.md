# Changelog — ship-to-staging

## v4 — 2026-06-29
- Added a CI-pitfall bullet for permission-blocked check tooling + a false-failure warning: `gh pr checks`/`check-runs` can 403 without `checks:read` (fall back to `gh run list`/`gh run watch` or the GitHub MCP `get_status`), and `gh run watch --exit-status` can exit non-zero on a dropped connection mid-run — re-query `gh run view --json status,conclusion` before trusting it (review: 2026-06-29-rek-1930-login-rate-limit.md, frictions: "`gh pr checks <N>` failed with 403 ... the prompt presents `gh pr checks` as the canonical check-rollup tool without a documented fallback" and "`gh run watch <id> --exit-status` repeatedly returned a non-zero exit while the run was still `in_progress` ... a false 'failure' signal").
- Added item 6, a review-round stop rule: resolve substantive findings, but stop after two consecutive rounds with no new substantive findings rather than chasing cosmetic/doc-nit churn (review: 2026-06-29-rek-1930-login-rate-limit.md, friction: "The Copilot two-round gate turned into 6 rounds, several of which were trivial doc-nits introduced by the previous fix ... no stop rule for diminishing-value rounds").

## v3 — 2026-06-29
- "How to test on staging" now splits guidance by change type: UI → steps for me; backend/security/RLS/data → the agent runs the proof itself (e.g. the no-UI "anon write" negative test) and presents evidence (review: 2026-06-29-ship-to-staging-rek-1790-blog-images.md, friction: "The 'How to test on staging' deliverable produced a verification step the human director could not action ... an attacker-style anon write ... has no UI").
- Added a CI-pitfall bullet: the Supabase preview-branch bot's "out-of-order migrations" warning is a benign preview artifact, not the authoritative Deploy to Supabase Staging job (review: 2026-06-29-ship-to-staging-rek-1790-blog-images.md, friction: "The CI-pitfalls list does not mention the Supabase preview-branch bot ... a benign preview artifact").
- Reworded item 3 so the risky-surface pause carve-outs are framed up front as classes that ALWAYS pause (review: 2026-06-29-ship-to-staging-rek-1790-blog-images.md, friction: "item 3 reads 'auto-merge by default,' but any security/RLS/migration change always trips the 3c pause carve-out").
- These re-apply, onto the v2 split, the improvements the Improver had drafted in PR #1 before the split landed. The review's 4th friction (mandatory visual proof) was applied to staging-finalise v2 instead, since that proof content moved there in the split.

## v2 — 2026-06-29
- Split into two prompts. This one now stops at "verified on staging (deploy + staging E2E green) + reported, awaiting manual testing" and no longer sets the Linear status. The post-manual-testing finalisation (proof comment, reconciling failure notices, set Ready to Deploy) moved to the new `staging-finalise` prompt. Updated SCOPE, Final deliverable, and Hard stop accordingly. (Manual human edit, requested by James.)

## v1 — 2026-06-26
- Initial version (ship the current branch to staging via the company-conventional PR/CI process; terminal state is the change verified on staging with the Linear issue at Ready to Deploy — never production).
