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

Attribute every friction to a prompt id in [brackets]. Only capture a review when the task actually surfaced a friction worth improving — if the run was clean with nothing to improve, don't write a review (we don't record clean runs).

Push it to prompt-review/main using the repo-scoped PAT — NOT the GitHub MCP. (The shared team GitHub MCP authenticates as a different account and is read-only on this repo, so MCP pushes fail.) The token is provided as the cloud-agent secret PROMPT_REVIEW_PAT (a fine-grained token scoped to this repo with Contents: read/write), injected as an env var.

Save the review to a file, then push it on branch main at path reviews/<YYYY-MM-DD>-<slug>.md, for example:

```bash
DATE=$(date +%F)
SLUG=<short-slug>
git clone --depth 1 "https://x-access-token:${PROMPT_REVIEW_PAT}@github.com/JamesAtReklaim/prompt-review.git" /tmp/prompt-review
cp review.md "/tmp/prompt-review/reviews/${DATE}-${SLUG}.md"
cd /tmp/prompt-review
git add "reviews/${DATE}-${SLUG}.md"
git -c user.name="prompt-review capture" -c user.email="team@reklaimyou.com" commit -m "review: <task>"
git push origin main
```

That push to main fires the Improver automatically.
