# Changelog — task-execution

## v3 — 2026-06-30
- Added Notion (specs + precedent QA pages) as a named first-class source in the "don't ask what the system already answers" rule, alongside repo rules / AGENTS.md / issue / docs/context (review: 2026-06-30-rek-1689-idor-lockdown.md, friction: "Both prompts say 'lean on project docs' but don't make Notion a first-class source ... I had only read repo docs, not Notion"). (The same review's "exhaust the issue's own proposed fix/acceptance before asking" point was already addressed by v2's "Linear issue is the HIGHEST authority" rule, so only the Notion-as-source gap remained for this prompt.)

## v2 — 2026-06-30
- In "Plan & ask", establish the Linear issue as the HIGHEST authority on WHAT to build: resolve every question against the issue text/comments first and treat anything the issue specifies as already-decided (not a question); reserve STOP-and-ask for genuine danger or scope well beyond the issue (review: 2026-06-30-rek-1924-survey-deeplinks.md, friction: "[task-execution] Asked blocking questions the Linear issue already answered ... The prompt treats the issue as input but doesn't establish it as the *highest authority* to self-resolve questions against before asking").

## v1 — 2026-06-26
- Initial version (plan → confirm (hard gate) → implement → verify → draft PR; reuses the prior read-only investigation step; never merges without explicit authorization).
