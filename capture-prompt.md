# Task Review Capture — run at the end of a task

Identify which named prompts were used: scan this task's conversation for tags like [PROMPT: <id> v<n>]. List each unique (id, version); use version unknown if a tag wasn't pasted.

Write the review in this exact shape:

---
task:
date:
prompts_used:
  - id:
    version:
---

## What went well

## Frictions / issues
- [ ]

## Suggested improvements
- [ ]

Attribute every friction to a prompt id in [brackets]. A clean run is still useful — record it with "none" under frictions.

Push it via the GitHub MCP: owner JamesAtReklaim, repo prompt-review, branch main, path reviews/<YYYY-MM-DD>-<slug>.md, message review: <task>. That push fires the Improver automatically.
