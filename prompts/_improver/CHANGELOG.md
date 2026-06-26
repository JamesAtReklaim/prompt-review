# Changelog — _improver

## v3 — 2026-06-26
- Process only the review(s) added by the triggering push (via git), not the whole reviews/ folder. Stop deleting consumed reviews — reviews/ is now an append-only log, pruned by hand occasionally (~100+ files). This also makes the merge of an improvement PR a clean no-op (it adds no new review, so the run exits immediately), removing the awkward delete-to-avoid-reprocessing dance. (Manual human edit, requested by James.)

## v2 — 2026-06-26
- Early-exit on clean reviews: a review with no actionable frictions ("none"/empty) yields no candidates, so the Improver no longer reads or edits any prompt for it. If the whole batch is clean, it quits without opening a PR (clean reviews get swept up by the next improvement PR). Avoids pointless prompt reads/edits on perfect runs. (Manual human edit, requested by James.)

## v1 — 2026-06-26
- Initial version. Advisory-only; never edits itself; deletes consumed reviews.
