---
task: REK-1689 — lock campaign-payout-eligibility SECURITY DEFINER RPCs to service_role (IDOR), investigate → ship → finalise to staging
date: 2026-06-30
prompts_used:
  - id: bug-investigation
    version: 1
  - id: task-execution
    version: 1
  - id: testing-status
    version: 3
  - id: testing-status
    version: 4
  - id: ship-to-staging
    version: 5
  - id: staging-finalise
    version: 4
---

## What went well

- [bug-investigation] The SchemaDoctor marker verification protocol paid off: the embedded impact-analysis marker pointed at a comment authored by a human (the issue creator), not the SchemaDoctor service account, so it correctly failed the author check. The prompt's "lean on docs, verify before trusting" framing pushed me to independently re-derive every claim (live ACL, zero callers, attack path) against the real migrations + DB.
- [ship-to-staging] Item 5's CI-pitfall list was high-value and hit almost verbatim: `mergeable_state` had to be reconciled (branch 15 behind main), `gh pr checks` / check-runs API 403'd on the PAT (fell back to `gh run list`), and the Supabase preview-branch ❌ was correctly pre-classified as benign vs. the authoritative `Deploy to Supabase Staging` job. Without that section I'd have chased false leads.
- [staging-finalise] The ordering constraint (QA Notion Sign-Off is a precondition for Ready to Deploy) and the "reconcile every ❌ notice" step produced a clean, auditable closeout; the hard-stop at Ready to Deploy (never Done/prod) was unambiguous.

## Frictions / issues

- [ ] [testing-status] The report is framed around "what tests can I run" rather than "what remains to be proven." It surfaced already-proven / redundant checks as actionable [YOU] items (e.g. an authenticated-JWT attacker curl after the anon curl + locked grants had already proven the grant-layer block). The user flagged this twice: "if the tests i can run are already proven then there's no point to me running them" and "think in terms of what needs to be proven, not what tests can possibly be run." v4 added the Lead risk table but did not fix the underlying framing.
- [ ] [ship-to-staging] Item 3(c) forces a mandatory stop-and-confirm for the ENTIRE risky-surface class (auth/payments/migrations/security/RLS) even when all gates are green, both review rounds are clean, and there is no actual bug to resolve. The user pushed back: "i don't think we need this pause when there's no real issue aside from touching something risky. we always check thoroughly before this point; it would be different if there was a bug you had to resolve." The pause added a round-trip with no risk-reduction value on a clean run.
- [ ] [bug-investigation] / [task-execution] The "Gaps / open questions for me" output (bug-investigation Phase B §7; task-execution §1 questions) encouraged surfacing questions that were actually answerable from (a) the issue's own prescribed fix and (b) Notion. I had only read repo docs, not Notion, and listed decisions as "open" that the issue + documented precedent already settled. The user had to redirect: "have you looked at notion docs and the repo? ... whatever we do we have to fulfil what it asks." Both prompts say "lean on project docs" but don't make Notion a first-class source nor require exhausting the issue's own proposed fix/acceptance before asking.
- [ ] [ship-to-staging] Item 5 covers the out-of-order migration warning but NOT the adjacent failure mode I hit: renaming a migration file after the first push leaves the per-PR Supabase preview branch with an orphaned migration version, surfacing as a red ❌ "Remote migration versions not found in local migrations directory." It looks alarming and needed manual explaining; the prompt doesn't pre-classify it.

## Suggested improvements

- [ ] [testing-status] Reframe sections C/D/E/F around UNPROVEN claims, not runnable tests. Driver question per item: "What is not yet proven?" Once a claim is proven by any actor, it is closed — list it only under "already proven," never in an action bucket, not even as "optional/redundant." Net effect: an item whose property is already established by a cheaper proof must be dropped from [YOU]/[AGENT-DESKTOP], not down-graded to optional.
- [ ] [ship-to-staging] Convert item 3(c) from "risky class ALWAYS pauses" to "pause only when a risky-surface change ALSO has an unresolved concern from 3(a)/3(b)/3(d) — a real bug/regression, a failing gate, or an open valid review finding. Absent that, auto-merge like any other change, explicitly noting in the report that it was a risky surface and why it's safe." Keep the production hard-stop unchanged.
- [ ] [bug-investigation] + [task-execution] Make Notion an explicit primary source alongside repo docs (the repo's own docs-navigation rule points to Notion hubs). Add a directive: before listing ANY "open question," confirm it is not already answered by (1) the issue's own proposed fix / acceptance criteria, (2) Notion specs + precedent QA pages, (3) repo docs/rules. Only genuinely unresolved items become questions; everything else is stated as a decision with its source.
- [ ] [ship-to-staging] Add to item 5: "Renaming a migration file after the first push orphans the old version in the per-PR Supabase preview branch → benign ❌ 'Remote migration versions not found'. It is NOT the authoritative deploy. Prefer finalising the migration timestamp BEFORE the first push; if you must rename post-push, expect and pre-explain this preview ❌."
