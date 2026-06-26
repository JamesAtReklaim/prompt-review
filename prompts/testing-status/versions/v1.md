---
id: testing-status
version: 1
updated: 2026-06-26
---

# [PROMPT: testing-status v1]
<!-- Identifying tag — keep this line. The capture/review step scans for
     "[PROMPT: <id> v<n>]" to know which prompt + version a task used.
     When this prompt is improved, bump BOTH the frontmatter `version` and the
     v<n> in the line above, together. -->

# Testing status & next-actions request (re-runnable at any stage)

We're mid-testing on the issue we've been working (committed on the current branch,
PR open; it may or may not be merged/deployed yet). Do NOT change product code.
First DETECT the current stage from context (is the PR open? merged? deployed to
staging? what have we already run this session?), then produce the report below.
This prompt is run repeatedly as we progress, so each time: re-detect the stage,
move anything newly verified into "Already run," and promote anything newly
unblocked into the right action bucket.

## Actors (tag every test with exactly one)

- **[AGENT-CLOUD]** — you can run it right now in this cloud agent (deterministic,
  no real browser, no human judgment; has repo, node, deno, read-only staging DB;
  edge functions are NOT deployed until merge; no logged-in browser session).
- **[YOU]** — I must do it: visual/cross-browser, real authenticated click-through,
  subjective judgment, anything needing eyes or a real account.
- **[AGENT-DESKTOP]** — needs a local stack / Docker / local Supabase / real browser
  automation but NOT human judgment; I'll run it in desktop Cursor from a single
  paste-able prompt you provide.

Rules: don't duplicate what CI gates already cover (name the gate instead); prefer
the cheapest actor that gives a trustworthy signal; be explicit about what's blocked
at this stage and when it unblocks; keep it concise and skimmable — I'm acting off
this, not reading prose.

## Output (exactly these sections, in order)

### A. Current stage

One line: where we are (e.g. "PR open, pre-merge, not deployed") and what that means
for what's testable right now.

### B. ✅ Already run — status

A compact table of every test executed so far this effort:
`test | where (Local/CI/Staging) | actor | result (PASS/FAIL/●blocked-now-clear)`.
Include CI gate status if you can see it. If unsure whether a cheap [AGENT-CLOUD]
check is still green, re-run it and record the fresh result here.

### C. ⏳ Still to run

One line per outstanding test, each tagged with actor and (if blocked) the blocker +
when it clears. This is the master list; the next sections make it actionable.

--- the part I act on ---

### D. ▶️ I can run these now (this stage) — [AGENT-CLOUD]

Run them now, then report results inline and reflect them up in section B. If none
are runnable at this stage, say so.

### E. 🧑 For you to run manually — [YOU]

Only the [YOU] tests that are actually doable at the CURRENT stage. For each: a
short numbered do-this / look-for-that with exact commands or clicks, the test
account/data, PASS vs FAIL, and setup gotchas (ports, env, cache, which env it
points at). Defer [YOU] tests that aren't unblocked yet to section G.

### F. 💻 Hand to desktop — [AGENT-DESKTOP]

A single self-contained, copy-paste prompt for desktop Cursor that runs every
currently-unblocked [AGENT-DESKTOP] test autonomously (branch checkout, setup incl.
docker/supabase/Doppler env, exact commands, expected results, "don't commit/push").
Omit this section if there are none at this stage.

### G. 🚧 Blocked / can't verify at this stage

What nothing can confirm yet, the actor it'll need, and the trigger that unblocks it
(e.g. "after merge → staging deploy"). These graduate into D/E/F on the next run.
