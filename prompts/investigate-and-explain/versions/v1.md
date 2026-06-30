---
id: bug-investigation
version: 1
updated: 2026-06-26
---

# [PROMPT: bug-investigation v1]
<!-- Identifying tag — keep this line. The capture/review step scans for
     "[PROMPT: <id> v<n>]" to know which prompt + version a task used.
     When this prompt is improved, bump BOTH the frontmatter `version` and the
     v<n> in the line above, together. -->

# Investigation-only task: <LINEAR_ID e.g. REK-1759>

You are doing a **read-only investigation** of the Linear issue **<LINEAR_ID>**. This is a
bug investigation. The goal is a correct, deep understanding of the problem — NOT a fix.

This task has **two clearly separate aspects**, with two different audiences:

- **Phase A — Investigate & understand (for YOU, the agent).** Go to full technical depth.
  Read the real code, trace the real data flow, confirm the real root cause. Be as
  technical and precise as you need to be here — this part is about *you* genuinely
  understanding the issue and the system around it.
- **Phase B — Explain it to me (for a non-technical reader).** Take everything you learned
  in Phase A and translate it into a high-level, plain-language explanation and a set of
  proposed solutions. Assume I have **no deep knowledge of the company, the technologies,
  or backend/database concepts.** I care about how the pieces fit together and what they
  do, not dense technical detail.

The deliverable you hand me is **Phase B**. Phase A is the work that makes Phase B correct.
Do not skip the depth of A, and do not hand me the raw technical depth of A as the answer —
distil it.

## Hard rules

- **Do NOT edit, create, or delete any code, config, migrations, or docs.** No commits, no
  branches, no PRs. This is purely investigative. If you want to "just fix it," stop and
  write it up as a proposed solution instead.
- Read-only tools only: read files, search the codebase, read the Linear issue and its
  comments, read the relevant feature specs/context docs, and (read-only) inspect the
  database schema / logs if it helps confirm a hypothesis. Do not mutate anything.
- Do not trust the issue title alone. Confirm what's actually happening against the code and
  the project's documented behavior before drawing conclusions.
- **Lean heavily on the project's docs, specs, and rules throughout.** This codebase has
  extensive written context — feature specs, a data model, architecture docs, and many
  company/engineering rules and conventions. These frequently answer questions the
  investigation raises (e.g. "is this the intended behavior?", "why is it built this way?",
  "are we even allowed to change X?") and they constrain which solutions are acceptable.
  Treat the docs as a primary source, not an afterthought.

---

## PHASE A — Investigate & understand (full technical depth, internal work)

This phase is for you to build a rock-solid understanding. Be rigorous and technical.

1. **Understand the task.** Pull the full Linear issue **<LINEAR_ID>**: title, description,
   ALL comments (fetch comments explicitly — they may not come back by default),
   attachments, labels, linked issues/PRs, repro steps. Restate the reported problem
   precisely: symptom, who's affected, when it happens, expected vs. actual behavior. Note
   anything ambiguous or missing.
2. **Orient using the project's own context — and keep returning to it.** Start from
   `docs/context/INDEX.md` and read the feature spec(s) under `docs/context/FEATURE_SPECS/`
   for the area this touches. Read the data-model / architecture docs if the bug touches the
   database or backend flows. Also consult the company/engineering rules and conventions
   (e.g. `.cursor/rules/*`, `AGENTS.md`/`CLAUDE.md`, and any migration/feature SSoT docs the
   specs point to). Identify exactly which feature, screens, backend functions, and database
   tables are involved. **Whenever the investigation raises a question** — intended behavior,
   an invariant, why something is designed a certain way, what's allowed or off-limits —
   **check the docs/specs/rules for the answer before guessing**, and note what you found
   (or that the docs are silent on it).
3. **Trace the real code path end to end** (UI → data fetching/hooks → backend/edge
   functions → database, as applicable). Cite specific files and line ranges as evidence.
4. **Find the root cause, not just the symptom.** Form a clear hypothesis (or competing
   ones). Distinguish "where it shows up" from "where it actually breaks." Confirm with
   read-only evidence (specs, schema, tests, logs, related past issues/PRs) wherever
   possible. Cross-check your understanding of "correct" behavior against what the specs say
   it *should* be — the bug may be code-vs-spec drift in either direction.
5. **Map the blast radius.** What else depends on this code path? What else could the same
   root cause — or any future fix — affect? Note any documented invariants or rules a fix
   must not violate.

Keep your Phase A evidence (file paths, line refs, doc/spec/rule sections, issue comments)
so the findings in Phase B are verifiable, but Phase A itself is your working analysis, not
the final answer.

---

## PHASE B — Explain it to me (high-level, non-technical — THIS is the deliverable)

Translate Phase A into plain language. Define any unavoidable technical term in one short
phrase. Prefer a "user does X → system does Y → here's where it goes wrong" narrative over
jargon. Structure it like this:

1. **TL;DR** (2–3 sentences): what the bug is and your leading theory of the cause.
2. **What this issue is** — the bug, in plain language.
3. **How it fits into the larger system** — which feature it lives in, how that feature
   works conceptually, and where in that flow the bug sits.
4. **The effects it has** — who/what is impacted, how badly, and any knock-on effects.
5. **What the docs/specs/rules say about it** — in plain language, note anything the
   project's written context established that's relevant: intended behavior, constraints,
   or rules that shaped (or ruled out) the options below. If the docs are silent or unclear
   on a key point, say so — that likely becomes one of the open questions in section 7.
6. **Proposed solutions** — give **2–4 distinct options**. For each: a short plain-language
   description, then **Pros** and **Cons** (effort, risk, how much it touches, whether it
   fixes the true root cause or just the symptom, long-term maintainability, **and whether
   it's consistent with the documented rules/intended design**). If you have a
   recommendation, state it and why — but still show the alternatives.
7. **Gaps / open questions for me** — the things you could NOT determine (including from the
   docs) that are blocking a better decision, phrased as direct questions I can answer. For
   each, say *why* it matters and how my answer would change your recommendation (e.g.
   missing repro steps, unclear intended behavior, a product/design call, data you couldn't
   access, a spec that's silent or contradictory).

Optionally, you may include a short **"Evidence / technical appendix"** at the very end with
the concrete file paths, line references, and doc/spec/rule sections behind your conclusions
— clearly separated, so the main explanation stays high-level for me but I (or another
agent) can verify it.

---

Reminder: produce **analysis and a written explanation only**. No code changes of any kind.
The thing I'm graded on is Phase B; the thing that makes Phase B trustworthy is Phase A.
