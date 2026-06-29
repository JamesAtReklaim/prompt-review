# Setup

## Create the capture credential (one-time)

capture-prompt.md pushes reviews to this repo from whatever repo a task ran in (a cross-repo write), so it needs its own token — NOT the shared team GitHub MCP (that runs as a different account and is read-only here).

- On github.com as JamesAtReklaim: Settings -> Developer settings -> Personal access tokens -> Fine-grained tokens. Scope it to ONLY the prompt-review repo, permissions Contents: read/write and Metadata: read.
- Store it as a USER-scoped Cloud Agent secret named PROMPT_REVIEW_PAT (cursor.com -> Cloud Agents -> Secrets). User scope keeps it out of the shared team config and limits blast radius to this one repo.
- BOTH capture-prompt.md AND the Improver automation read PROMPT_REVIEW_PAT from the environment to push to main — so it must be injected into the automation's runs too (see the Improver setup + Identity note below).

## Create the Improver automation (cursor.com/automations -> New Automation)

- Trigger: GitHub -> "Push to branch" -> branch main -> repo JamesAtReklaim/prompt-review
- Repository: JamesAtReklaim/prompt-review @ main
- Permission scope: Private (runs as my GitHub account) — REQUIRED so the user-scoped PROMPT_REVIEW_PAT secret is injected into the run. The Improver commits directly to main with that token; it does NOT open a PR.
- Tools: none special. It edits files in its own checkout and pushes to main via PROMPT_REVIEW_PAT. (Optionally keep Memories. The GitHub MCP is not needed — and is read-only here anyway.)
- Prompt: paste the contents of prompts/_improver/prompt.md
- Save -> "Run now" once with a sample review in reviews/ to test, then confirm: (a) PROMPT_REVIEW_PAT is present in the run's env, and (b) the improvement commit lands on main directly (no PR).

## Use it

- Put real prompts into prompts/<id>/prompt.md (keep frontmatter + [PROMPT:] tag).
- At the end of a task, run capture-prompt.md.
- The Improver applies improvements to main automatically — nothing to merge. Skim the changelog / git history if you want to see what changed; `git revert` any edit you don't like (every version is snapshotted under versions/).
- To change the Improver, edit prompts/_improver/prompt.md by hand and bump its version, then re-paste it into the automation.

## Identity note

- The Improver automation must run at **Private scope** as my GitHub account so the **user-scoped PROMPT_REVIEW_PAT secret is injected**. It commits straight to main using that token. (If it were Team Owned, it would run as the shared cursor service account and might not get my user secret — do NOT promote it to Team Owned.)
- Both capture-prompt.md and the Improver write to main via the same repo-scoped PROMPT_REVIEW_PAT, independent of the shared team GitHub MCP (which is a different account and read-only here). Do NOT rely on the team MCP for writes to this repo.
