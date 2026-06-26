# Changelog — _improver

## v2 — 2026-06-26
- Early-exit on clean reviews: a review with no actionable frictions ("none"/empty) yields no candidates, so the Improver no longer reads or edits any prompt for it. If the whole batch is clean, it quits without opening a PR (clean reviews get swept up by the next improvement PR). Avoids pointless prompt reads/edits on perfect runs. (Manual human edit, requested by James.)

## v1 — 2026-06-26
- Initial version. Advisory-only; never edits itself; deletes consumed reviews.
