# Changelog — bug-investigation

## v2 — 2026-06-30
- Made Notion a first-class source alongside the in-repo docs in the "lean heavily on docs" hard rule (the repo's own docs-navigation rule points to Notion hubs: feature specs, product decisions, precedent QA pages) (review: 2026-06-30-rek-1689-idor-lockdown.md, friction: "Both prompts say 'lean on project docs' but don't make Notion a first-class source ... I had only read repo docs, not Notion").
- Added a gate to Phase B §7 (Gaps / open questions): before listing ANY open question, confirm it is not already answered by (1) the issue's own proposed fix / acceptance criteria, (2) Notion specs + precedent QA pages, (3) repo docs/specs/rules — anything those settle is stated as a sourced decision, not raised as a question (review: 2026-06-30-rek-1689-idor-lockdown.md, friction: "encouraged surfacing questions that were actually answerable from (a) the issue's own prescribed fix and (b) Notion ... listed decisions as 'open' that the issue + documented precedent already settled").

## v1 — 2026-06-26
- Initial version (read-only bug investigation: Phase A full technical root-cause analysis for the agent, Phase B plain-language explanation + 2–4 solution options for a non-technical reader; no code changes).
