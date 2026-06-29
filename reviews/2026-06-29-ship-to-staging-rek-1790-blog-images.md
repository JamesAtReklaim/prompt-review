---
task: Ship REK-1790 (drop over-permissive blog-images storage RLS policy) to staging
date: 2026-06-29
prompts_used:
  - id: ship-to-staging
    version: 1
---

## What went well

- The `main`-sync precondition + the `mergeable_state: dirty` explanation [ship-to-staging] paid off conceptually: the branch was 32 commits behind, so the merge-first step was clearly the right discipline even though it merged cleanly with no conflicts. The "don't chase token/identity theories before confirming mergeability" guidance kept diagnosis honest.
- The risky-surface pause rule (item 3c: migrations, security/RLS) [ship-to-staging] worked exactly as intended — it correctly forced a stop-and-confirm before merging a `DROP POLICY` migration instead of auto-merging.
- "Merged ≠ on staging" [ship-to-staging] drove watching the post-merge `main` run to completion (staging deploy + staging E2E) plus a DB re-query, which is the right bar for "actually shipped."
- The hard-stop-at-staging section [ship-to-staging] was unambiguous and held cleanly through sign-off.

## Frictions / issues

- [ ] [ship-to-staging] The "How to test on staging" deliverable produced a verification step the human director could not action and had not seen before. For a backend/security/RLS change, the natural "3–6 concrete things to verify" included an attacker-style **anon write** (raw `curl` to the Storage API with the public key) — which has no UI and isn't something a director clicks through. This caused two rounds of "what is this / where is it / I've never used it" before the agent pivoted to *running the exploit itself* and presenting the `403` as proof. The prompt frames the test section as instructions *for the human*, but security/backend proofs often must be *executed by the agent*.
- [ ] [ship-to-staging] The CI-pitfalls list does not mention the **Supabase preview-branch bot**, which posted a scary `⚠️ Applied out-of-order migrations` warning that looks like a migration failure but is a benign preview artifact (independent of the authoritative "Deploy to Supabase Staging" job). It took manual reasoning to dismiss; a one-liner in the pitfalls section would pre-empt that.
- [ ] [ship-to-staging] Minor: item 3 reads "auto-merge by default," but any security/RLS/migration change *always* trips the 3c pause carve-out, so the stated default never actually applies to that whole class. The interaction between "default auto-merge" and "always-pause surfaces" could be stated more directly so an agent doesn't present it as a surprise each time.

## Suggested improvements

- [ ] [ship-to-staging] In the "How to test on staging" section, split guidance by change type: for **UI changes** → steps the human performs; for **backend / security / RLS / data** changes → the agent should *run the proof itself* (e.g. fire the blocked exploit, show before/after request output, query the DB) and present evidence, then give the human only the optional/UI-level checks. Explicitly note that an "anon write"-style negative test has no UI and must be executed by the agent, not handed to the director.
- [ ] [ship-to-staging] Add a CI-pitfall bullet: "The Supabase **preview-branch** bot comments (Migrations ⚠️ / out-of-order warnings) are NOT the staging deploy. Treat the `Deploy to Supabase Staging` job in the post-merge `main` run as authoritative; an out-of-order warning from interleaved independent migrations is benign — confirm independence rather than blocking."
- [ ] [ship-to-staging] Reword item 3 so the pause carve-outs are framed as "these classes always pause" up front, rather than "auto-merge by default … except," to set expectations for security/migration work from the start.
