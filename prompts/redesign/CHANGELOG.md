# Changelog — redesign

## v2 — 2026-06-29
- Phase 4 live verification: added a clause for backend-/condition-gated UI states — it's acceptable to inject/stub the triggering response to render+screenshot the state, labelled "state injected; real path verified separately," and confirm the real path by other means (review: 2026-06-29-rek-1930-login-rate-limit.md, friction: "Phase 4's 'load the ACTUAL changed screen … VISUAL DIFF' assumes the target state is directly reachable ... gated behind a backend/transient condition ... no guidance for verifying states you can't reach by normal interaction").
- Clarified the MISSING-BACKEND rule's boundary: plumbing an already-tracked/already-computed value through an existing mechanism (additive, non-breaking) is allowed when it's the only truthful way to meet an explicit requirement (e.g. exposing a rate-limit `retryAfter`) and is NOT a forbidden fabricated stand-in — still flag for sign-off (review: 2026-06-29-rek-1930-login-rate-limit.md, friction: "The MISSING-BACKEND rule nearly conflicted with the core requirement ... the boundary between 'forbidden fabrication/new mechanism' and 'allowed plumbing of an existing value' had to be reasoned out from scratch").

## v1 — 2026-06-26
- Initial version (phased redesign workflow: branch setup → investigation w/ resource-access gate → planning + design decomposition → confirmation gate → implement & live visual-diff verification → report → review loop; never merges to main; mockup is the layout authority; copy-truth and missing-backend guardrails).
