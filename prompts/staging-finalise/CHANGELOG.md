# Changelog — staging-finalise

## v2 — 2026-06-29
- Make visual evidence mandatory whenever ANY acceptance criterion is visual — even on an otherwise backend/RLS change — and add a pre-post checklist line ("does any acceptance item involve what something looks like? if yes, the comment must contain matching images"); capture the deployed-staging images yourself or ask before posting, never silently drop a visual criterion (review: 2026-06-29-ship-to-staging-rek-1790-blog-images.md, friction: "The Linear proof comment omitted images even though part of the acceptance was a visual check ... the visual proof was silently skipped"). Friction was filed against ship-to-staging v1, but the proof step now lives here after the split.

## v1 — 2026-06-29
- Initial version. Split out of ship-to-staging v1: the post-manual-testing finalisation (proof comment on Linear with correctly-embedded images, reconcile every failure/error notice, set the issue to Ready to Deploy). Runs only after my staging sign-off; never production/Done/release.
