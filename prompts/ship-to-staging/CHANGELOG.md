# Changelog — ship-to-staging

## v2 — 2026-06-29
- Split into two prompts. This one now stops at "verified on staging (deploy + staging E2E green) + reported, awaiting manual testing" and no longer sets the Linear status. The post-manual-testing finalisation (proof comment, reconciling failure notices, set Ready to Deploy) moved to the new `staging-finalise` prompt. Updated SCOPE, Final deliverable, and Hard stop accordingly. (Manual human edit, requested by James.)

## v1 — 2026-06-26
- Initial version (ship the current branch to staging via the company-conventional PR/CI process; terminal state is the change verified on staging with the Linear issue at Ready to Deploy — never production).
