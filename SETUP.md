# Setup

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

The automation acts as me ONLY because it's Private scope on my own repo. Do NOT promote it to Team Owned — that switches it to the shared cursor service account.
