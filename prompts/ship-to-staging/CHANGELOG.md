# Changelog — ship-to-staging

## v2 — 2026-06-29
- "How to test on staging" deliverable now splits guidance by change type: UI changes → steps for the human; backend/security/RLS/data changes → the agent runs the proof itself (e.g. the no-UI "anon write" negative test) and presents evidence (review: 2026-06-29-ship-to-staging-rek-1790-blog-images.md, friction: "The 'How to test on staging' deliverable produced a verification step the human director could not action and had not seen before ... an attacker-style **anon write** ... has no UI").
- Added a CI-pitfall bullet clarifying that the Supabase preview-branch bot's "out-of-order migrations" warning is a benign preview artifact, not the authoritative `Deploy to Supabase Staging` job (review: 2026-06-29-ship-to-staging-rek-1790-blog-images.md, friction: "The CI-pitfalls list does not mention the **Supabase preview-branch bot**, which posted a scary `⚠️ Applied out-of-order migrations` warning that looks like a migration failure but is a benign preview artifact").
- Reworded item 3 so the risky-surface pause carve-outs (security/RLS/migrations/etc.) are framed up front as classes that ALWAYS pause, rather than "auto-merge by default … except" (review: 2026-06-29-ship-to-staging-rek-1790-blog-images.md, friction: "item 3 reads 'auto-merge by default,' but any security/RLS/migration change *always* trips the 3c pause carve-out, so the stated default never actually applies to that whole class").

## v1 — 2026-06-26
- Initial version (ship the current branch to staging via the company-conventional PR/CI process; terminal state is the change verified on staging with the Linear issue at Ready to Deploy — never production).
