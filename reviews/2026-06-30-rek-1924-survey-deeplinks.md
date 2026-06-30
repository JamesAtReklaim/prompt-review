---
task: REK-1924 Survey Deeplinks — investigate → implement → ship to staging → finalise
date: 2026-06-30
prompts_used:
  - id: opening-investigation
    version: 1
  - id: task-execution
    version: 1
  - id: testing-status
    version: 3
  - id: ship-to-staging
    version: 5
  - id: staging-finalise
    version: 3
---

## What went well

- [ship-to-staging] The **risky-surface pause (item 3c)** worked exactly as intended: the change touched auth + a migration, so the agent got everything merge-ready then stopped for explicit authorization instead of auto-merging. Clear, predictable.
- [ship-to-staging] **Sync-main-first (item 1)** and the **no-op `tsc --noEmit` caveat (item 5)** both paid off — main had advanced 8 commits, and running `tsc -p tsconfig.app.json` surfaced (pre-existing) errors the root command hides, so the agent could correctly attribute them as not-mine rather than chase a phantom.
- [staging-finalise] Forcing **failure-notice reconciliation** + **explicit Ready-to-Deploy-only hard stop** kept the close-out honest and bounded (no production creep).
- [opening-investigation] Produced a grounded, file:line writeup that made the later implementation fast.

## Frictions / issues

- [ ] **[task-execution] Asked blocking questions the Linear issue already answered.** In "Plan & ask" the agent surfaced Q2/Q3/Q4 (login/register round-trip, "completed → notice + next", "all surveys") as decisions to confirm, when the issue spec stated all of them explicitly. The user had to redirect: "we have to follow the linear issue spec, does that answer any of the questions." The prompt treats the issue as input but doesn't establish it as the *highest authority* to self-resolve questions against before asking.
- [ ] **[testing-status] The desktop (section F) hand-off is one-directional.** The desktop-Cursor prompt told the desktop agent what to run but not to **emit a return prompt back to the cloud agent** at the end. The human ended up manually translating desktop results back into chat, instead of just ferrying a prompt.
- [ ] **[testing-status] The desktop agent chased unavailable secrets instead of skipping.** It tried to fetch Doppler/AgentMail/service-role keys (which it couldn't access) and got blocked, rather than recognising those tests are either CI-covered or must be done manually by the human. Relatedly, the cloud agent did **not pre-declare up front which tests are [YOU]-manual** — the split only emerged after the desktop agent hit the blocker.
- [ ] **[testing-status] No required risk-communication format.** The user had to iteratively extract the model they wanted: a one-sentence headline + glanceable emoji table with **Risk (impact if it fails) × Failure-chance (likelihood, justified by what was built + already-passed tests)**, and a **severity scale anchored on irreversibility/production-impact** (critical = touches prod data), NOT blast radius. The prompt doesn't specify how to express per-test risk, so it took several turns to converge.
- [ ] **[staging-finalise] Image-sourcing gap for AUTHENTICATED staging proofs.** The prompt's embed flow assumes the agent either already has the image bytes or can capture a *public* surface. The real case here: the visual acceptance criteria were behind login, and the human's proof screenshot lived **in the chat (not a file the agent can read or upload)**. The agent first over-invested in a fragile headless OTP login (cookie-banner click interception, login email not arriving) before falling back to asking the human — and then needed a bespoke "paste into Linear → re-host under my own asset → embed" dance.
- [ ] **[staging-finalise] No guidance to clean up the human's hand-off image comment.** After the human pasted the screenshot into a Linear comment so the agent could source it, that comment became redundant clutter. The agent didn't remove it proactively — the user had to ask "did you delete the comment… it's useless now."

## Suggested improvements

- [ ] **[task-execution]** In "Plan & ask", add a hard line: *"The Linear issue is the highest authority on WHAT to build. Before listing any question, resolve it against the issue text/comments first and treat anything the issue specifies as already-decided (not a question). Reserve STOP-and-ask for genuine danger (irreversible/destructive, security/compliance) or scope well beyond the issue — not for choices the issue already makes."* Mirror the existing "don't ask what repo rules/AGENTS already answer" clause, with the issue as the top source.
- [ ] **[testing-status]** Section F must instruct the desktop agent to **end by producing a single paste-able return prompt addressed to the cloud agent** (results table per step, env/setup notes, what it couldn't do + why) so the human only ferries prompts both ways.
- [ ] **[testing-status]** Section F must tell the desktop agent to **NOT request or block on secrets/keys** — run only what works without them, and **skip + hand back** anything needing keys or human eyes. Correspondingly, add a requirement that the cloud agent **names the [YOU]-manual tests explicitly and up front** (before the desktop hand-off), not as a post-hoc discovery.
- [ ] **[testing-status]** Codify the report shape: a **one-sentence headline**, then a glanceable table with **Risk | Failure-chance (cite the evidence/already-passed tests) | Overall**, using a severity legend anchored on **irreversibility/production-impact** (🔴 critical = prod data/irreversible; 🟠 high = security boundary or wide-but-reversible; 🟡 medium = degraded UX; 🟢 low = cosmetic). "Overall = Risk × Failure-chance" so high-impact-but-near-zero-likelihood reads green.
- [ ] **[staging-finalise]** Add an explicit branch to the image step: *"If the visual proof is an AUTHENTICATED surface you can't reach, do NOT burn rounds on headless login — ask the human to paste the screenshot into a Linear comment (or attach the file). Then **download those bytes, re-upload under your OWN issue asset, embed that**, and verify it renders."* Make "ask the human" the first-line option for authenticated visuals, not a last resort.
- [ ] **[staging-finalise]** Add a clean-up step: *"If you sourced an image via a human hand-off comment, fold it into the consolidated proof comment and then DELETE the redundant hand-off comment — but re-host the image under your own asset FIRST so the deletion can't break the embed."* Make this proactive, not user-prompted.
