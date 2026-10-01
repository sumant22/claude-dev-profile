---
name: dev-profile
description: Analyse how this developer works with Claude and propose a categorised setup — a global developer working profile (personal rules), project memory (facts, patterns, temporary context), team skills (repeatable workflows) and optional hooks — with every recommendation labelled personal or team and nothing written until the developer says go. Use when a developer wants to set up, audit or clean their Claude memory, CLAUDE.md, skills or working preferences. Works for any stack.
---

# Developer Working Profile

## Purpose

Turn accumulated chat history, memory and corrections into a small set of categorised, durable rules and
artefacts, placed where each is actually used, with private data kept out. Report first, change nothing
until approved.

Arguments: `/dev-profile` runs the full analysis. `/dev-profile apply` applies the plan approved earlier in
this same session; if no report exists in the current conversation, say so and offer to run the analysis
first instead of guessing a plan. `/dev-profile privacy` runs only Step 4.

## Step 1 — Gather evidence

None of these sources is required. Most developers have no CLAUDE.md and no curated memory; their patterns
live in the transcripts and the git history. Use whatever exists, in this order:

1. **Session transcripts** — `~/.claude/projects/<project-slug>/*.jsonl`, the primary source. Read the
   user-role messages of the most recent sessions (start with 10–20). Look for: corrections ("why did you…",
   "no need…", "we have X right?"), reverts, the same instruction given more than once, the same context
   re-explained, prompts typed in near-identical form, multi-step tasks run more than once, how bugs are
   reported, how approval is given.
2. **Prompt history** — `~/.claude/history.jsonl`, one line per prompt across all projects. Cheap way to
   find repeated prompts and which projects the developer works in.
3. **Git history** — the developer's own commits in this repo: commit message style, size of changes,
   files touched together, reverts, branch naming.
4. **The codebase** — patterns the developer already follows, so the profile mirrors reality rather than
   inventing a style.
5. The memory directory and its index file, if present.
6. CLAUDE.md files: global (`~/.claude/CLAUDE.md`), repo root, sub-folders, if present.
7. Skills under the repo's `.claude/skills` and the global `~/.claude/skills`, if present.
8. Hooks and permissions in `settings.json` / `settings.local.json`, if present.
9. The current conversation.

Transcripts contain whatever the developer pasted, including secrets. Read them for patterns only; never
copy a credential, token, email or customer record into the report or any file.

**Cold start.** If there are fewer than about five transcripts and no memory, do not invent a profile. Run a
short interview instead: one question per bucket 1–4 and 6, each with two or three concrete options and a
"skip" answer, then build the profile from the answers and mark every rule as *stated, not yet observed*.
The next run of `/dev-profile` upgrades rules to *confirmed* once transcripts show them in practice.

Steps 1–5 are read-only. Nothing is created, moved or deleted anywhere until the developer says go, so
existing memory, skills and hooks keep working unchanged.

Code first, notes second. Never assert a preference without evidence. State the checked-out branch.

## Step 2 — Classify every finding

One bucket each:

| # | Bucket | Contains |
|---|--------|----------|
| 1 | Coding principles | scope, refactoring, comments, naming, style |
| 2 | Communication style | confirming understanding, tone, reporting, questions |
| 3 | Debugging workflow | order of reproduce → root cause → minimal fix → edge cases → verify |
| 4 | Architecture preferences | layers to respect, state management to reuse, never-introduce list |
| 5 | Code patterns | approved reusable implementations, with the file that is the good example |
| 6 | Change-management rules | what may not be removed, renamed, committed or pushed without an explicit go |
| 7 | Skills and workflows | repeatable multi-step tasks: scaffolding, audits, wiring checks, PR review, release |
| 8 | Project-specific knowledge | architecture, utilities, naming, tooling facts for this project only |
| 9 | Temporary context | current bugs, branches, open PRs; must carry an "as of" date and expire |
| 10 | Memory hygiene | how all of the above stays clean |
| 11 | Working-habit observations | where the developer loses time or safety: context re-explained each session, credentials pasted, fixes verified late, repeated manual steps, the same rule given twice |
| 12 | Repeated prompts | prompts typed more than once in similar form → propose a reusable prompt template or a skill |

For each finding record: rule in one line · evidence (quote or action, dated if known) · confidence
(confirmed / inferred / single occurrence) · **scope: personal or team**.

Scope test: would a teammate on the same repo want the same thing? Yes → team. It is about how this one
developer prefers to be worked with → personal.

## Step 3 — Place by type, not by content

| Finding type | Goes to | Visibility |
|---|---|---|
| Standing rule on how to work with me (1–4, 6, 10) | global `~/.claude/CLAUDE.md`, written stack-neutral, current stack only as example | personal |
| Team convention everyone must follow | repo `CLAUDE.md` or the skills' shared conventions file | team |
| Approved code pattern (5) | project memory, one file per pattern, linked from the index and from any skill that generates it | personal, or team if promoted into a skill |
| Repeatable multi-step workflow (7) | a skill under the repo's `.claude/skills` with a clear trigger sentence | team |
| Hard constraint that must never be skipped | a hook in `settings.json`; rules guide but cannot block. Propose only if asked | personal or team |
| Project fact (8) | project memory, indexed by module or topic. Do not shrink | personal |
| Temporary note (9) | project memory, dated, with status. Flag finished ones for deletion | personal |
| Rule needing long detail | its own memory file; the profile links to it in one line | as above |

A skill, not a memory, whenever the task has steps, checks and an output format. Skills are shared, so they
contain no personal preferences and no private data.

## Step 4 — Privacy filter

List anything stored that must not be: credentials, tokens, pasted logins, emails, phone numbers, customer or
production data, internal IDs, colleague personal details. Propose removal or anonymisation. Check skills and
repo CLAUDE.md with extra care because the team reads them.

## Step 5 — Keep thinking open

The profile's first principle: **analyse freely, act narrowly.** Rules limit what is changed unasked, never
what is noticed or suggested. Suggestions go in a short separate section after the delivered work, one line
each with the benefit. Open design questions ("how should we build this?") are exempt from the minimal-change
rules and get the full range of options with a recommendation.

## Output — one report, easy to scan (no files changed yet)

1. **Summary** — five lines max on how this developer works, plain words, including what already works well.
2. **Action table** — Create / Update / Delete / Keep, one row per file, rule, pattern or skill, with columns:
   type (profile / memory / skill / hook / repo-CLAUDE.md) · scope (personal / team) · reason.
3. **Proposed global profile** — complete, under 80 lines, sections 0–6 (0 = analyse freely, act narrowly).
4. **Proposed memory index** — grouped by the buckets above.
5. **Skills** — existing ones with a verdict (keep / update / merge / retire) and proposed new ones with their
   trigger sentence and whether they are team or personal.
6. **Working-habit observations** — what costs time or risk today and the one change that fixes each.
   Specific and plain, no blame.
7. **Repeated prompts** — each repeated prompt with a proposed reusable template or skill trigger.
8. **Privacy findings.**
9. **Open questions** — where evidence conflicts or is thin.

Then stop. Write, move or delete nothing until the developer says go. On go, apply the plan exactly and show
the final file list. Temporary notes never become permanent rules without explicit confirmation.

## Sharing this skill

Copy the `dev-profile` folder to `~/.claude/skills/` for personal use in every project, or into a repo's
`.claude/skills/` so the whole team gets `/dev-profile`. The skill itself holds no personal data.
