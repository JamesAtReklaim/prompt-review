# prompt-review (private)

A private, self-improving library of my reusable task prompts plus a background agent that turns end-of-task reviews into small, evidence-backed prompt improvements. Private to me; not connected to any company repo.

## The loop

- Named, versioned prompts live in prompts/<id>/.
- At the end of a task I run capture-prompt.md, which writes a review to reviews/ and pushes it (via a repo-scoped token, PROMPT_REVIEW_PAT — not the shared team MCP).
- That push triggers the Improver automation (prompts/_improver/prompt.md), which reads ONLY the newly-pushed review(s) -> checks each friction against the CURRENT prompt version (skips anything already fixed) -> makes the smallest improving edit, bumps the version, snapshots it, updates the changelog -> opens ONE advisory PR for me to merge. Reviews are kept as an append-only log (not deleted).
- I review/merge the PR. The agent never merges.

## Structure

- prompts/<id>/prompt.md — CURRENT version (copy this to use). Carries the [PROMPT: <id> v<n>] tag.
- prompts/<id>/CHANGELOG.md — one entry per version.
- prompts/<id>/versions/vN.md — frozen, append-only snapshots.
- prompts/_improver/ — the agent's OWN prompt; I edit it by hand, no agent touches it.
- reviews/ — append-only log of task reviews; the Improver reads only the newly-pushed one(s) and never deletes them. Prune by hand occasionally (e.g. when it grows past ~100 files).
- capture-prompt.md — end-of-task prompt that writes + pushes a review.

## Safety rules

- Advisory only (agent opens PRs; I merge). Append-only versions + changelog.
- The Improver never edits itself — to change it, I edit prompts/_improver/ by hand.
- Every edit cites the review + friction that motivated it. Reviews are an append-only log, pruned by hand occasionally.
- Reviews are pushed with a repo-scoped personal token (PROMPT_REVIEW_PAT), not the shared team GitHub MCP — so they land as me and the token can only touch this repo.
