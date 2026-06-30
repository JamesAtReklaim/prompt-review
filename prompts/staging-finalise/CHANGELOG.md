# Changelog — staging-finalise

## v4 — 2026-06-30
- Added an explicit authenticated-visual branch to the proof-image step: if the visual proof is behind login and you don't have the bytes, make "ask me to paste it into a Linear comment (or attach the file)" the FIRST-line option (don't burn rounds on fragile headless OTP login), then download those bytes, re-host under your OWN issue asset, embed that, and verify it renders (review: 2026-06-30-rek-1924-survey-deeplinks.md, friction: "Image-sourcing gap for AUTHENTICATED staging proofs ... The agent first over-invested in a fragile headless OTP login ... before falling back to asking the human").
- Added a proactive clean-up step: once the own-asset embed renders, fold the image into the consolidated proof comment and DELETE the redundant hand-off comment — re-host under your own asset FIRST so the deletion can't break the embed (review: 2026-06-30-rek-1924-survey-deeplinks.md, friction: "No guidance to clean up the human's hand-off image comment ... the user had to ask 'did you delete the comment… it's useless now'").

## v3 — 2026-06-29
- Added the QA Notion Sign-Off as an explicit step (new step 3) gating the Ready to Deploy transition, per qa-checklist.mdc, and listed that rule in Relevant docs — so the QA artifact isn't left stale and the agent doesn't rely on a human nudge (review: 2026-06-29-ship-to-staging-finalisation-gaps.md, friction: "The Finalisation section omits the QA Notion Sign-Off step, even though qa-checklist.mdc makes a completed Sign-Off the precondition for moving Linear to Ready to Deploy ... I only caught the gap because the human nudged"). Friction was filed against ship-to-staging v1, but finalisation now lives here after the split.

## v2 — 2026-06-29
- Make visual evidence mandatory whenever ANY acceptance criterion is visual — even on an otherwise backend/RLS change — and add a pre-post checklist line ("does any acceptance item involve what something looks like? if yes, the comment must contain matching images"); capture the deployed-staging images yourself or ask before posting, never silently drop a visual criterion (review: 2026-06-29-ship-to-staging-rek-1790-blog-images.md, friction: "The Linear proof comment omitted images even though part of the acceptance was a visual check ... the visual proof was silently skipped"). Friction was filed against ship-to-staging v1, but the proof step now lives here after the split.

## v1 — 2026-06-29
- Initial version. Split out of ship-to-staging v1: the post-manual-testing finalisation (proof comment on Linear with correctly-embedded images, reconcile every failure/error notice, set the issue to Ready to Deploy). Runs only after my staging sign-off; never production/Done/release.
