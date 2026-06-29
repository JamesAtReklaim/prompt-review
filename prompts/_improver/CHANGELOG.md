# Changelog — _improver

## v5 — 2026-06-29
- Fix the duplicate-PR guard: detect an existing open improvement PR by its TITLE ("prompt improvements — …"), not by an `improve/*` branch glob. The automation platform forces a `cursor/...` branch name, so the old branch-pattern check never matched — the Improver couldn't see its own open PR and opened a colliding second one (which then clashed with a concurrent human edit). Dropped the unachievable `improve/<date>-<slug>` branch instruction (title is the durable identifier). Added guidance to bump from each prompt's CURRENT version on main (not the review's, which may be stale) and re-read before writing, to avoid version-number collisions when edits race. (Manual human edit, requested by James.)

## v4 — 2026-06-26
- Revert v3's "process only the newly-pushed review" approach: it could silently drop a review captured while a prior improvement PR was still open (the open-PR guard made that run exit, and v3 never revisited it). Back to the robust original (Option A): each run reads ALL reviews in reviews/ (the unprocessed queue), reconciles idempotently against current prompt versions, and DELETES consumed reviews in the improvement PR — so the queue self-cleans and nothing is lost (deferred reviews are picked up after the open PR merges). No-improvement runs (e.g. a friction already fixed) open no PR and leave those reviews to be swept by the next improvement PR. (Manual human edit, requested by James.)

## v3 — 2026-06-26
- Process only the review(s) added by the triggering push (via git), not the whole reviews/ folder. Stop deleting consumed reviews — reviews/ is now an append-only log, pruned by hand occasionally (~100+ files). This also makes the merge of an improvement PR a clean no-op (it adds no new review, so the run exits immediately), removing the awkward delete-to-avoid-reprocessing dance. (Manual human edit, requested by James.)

## v2 — 2026-06-26
- Early-exit on clean reviews: a review with no actionable frictions ("none"/empty) yields no candidates, so the Improver no longer reads or edits any prompt for it. If the whole batch is clean, it quits without opening a PR (clean reviews get swept up by the next improvement PR). Avoids pointless prompt reads/edits on perfect runs. (Manual human edit, requested by James.)

## v1 — 2026-06-26
- Initial version. Advisory-only; never edits itself; deletes consumed reviews.
