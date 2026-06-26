# Setup

## Create the capture credential (one-time)

capture-prompt.md pushes reviews to this repo from whatever repo a task ran in (a cross-repo write), so it needs its own token — NOT the shared team GitHub MCP (that runs as a different account and is read-only here).

- On github.com as JamesAtReklaim: Settings -> Developer settings -> Personal access tokens -> Fine-grained tokens. Scope it to ONLY the prompt-review repo, permissions Contents: read/write and Metadata: read.
- Store it as a USER-scoped Cloud Agent secret named PROMPT_REVIEW_PAT (cursor.com -> Cloud Agents -> Secrets). User scope keeps it out of the shared team config and limits blast radius to this one repo.
- capture-prompt.md reads PROMPT_REVIEW_PAT from the environment to push the review.

## Create the Improver automation (cursor.com/automations -> New Automation)

- Trigger: GitHub -> "Push to branch" -> branch main -> repo JamesAtReklaim/prompt-review
- Repository: JamesAtReklaim/prompt-review @ main
- Tools: GitHub MCP + Open Pull Request (+ optional Memories)
- Permission scope: Private (runs as my GitHub account, billed to me, private to me)
- Prompt: paste the contents of prompts/_improver/prompt.md
- Save -> "Run now" once with a sample review in reviews/ to test.

## Use it

- Put real prompts into prompts/<id>/prompt.md (keep frontmatter + [PROMPT:] tag).
- At the end of a task, run capture-prompt.md.
- Review + merge the Improver's PRs.
- To change the Improver, edit prompts/_improver/prompt.md by hand and bump its version, then re-paste it into the automation.

## Identity note

There are two separate GitHub identities in play, and that's fine:

- The Improver automation runs at Private scope as my GitHub account (billed to me, private to me). It opens its advisory PR via the native Open Pull Request tool — it never pushes to main, and it only reads via the GitHub MCP (read access is enough).
- Reviews are pushed by capture-prompt.md using the repo-scoped PROMPT_REVIEW_PAT, so they land as me regardless of what the shared team GitHub MCP is authenticated as.

Do NOT promote the automation to Team Owned — that switches it to the shared cursor service account. And do NOT rely on the shared team GitHub MCP for writes to this repo; use PROMPT_REVIEW_PAT.
