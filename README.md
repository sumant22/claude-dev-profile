# claude-dev-profile

A Claude Code skill that turns your chat history into a small, categorised set of working rules.

Run `/dev-profile` inside any project. Claude reads your past sessions, memory, skills and git history,
then shows you one report: how you work, what you repeat, what slows you down, which rules should become
a personal profile, which should become team skills, and what private data should not be stored.
Nothing on disk changes until you say **go**.

Works for any language or framework. The skill itself contains no personal data.

## Why

Long chat histories do not make Claude better at working with you. Rules do. But rules scattered across
hundreds of memory notes are recalled unreliably, mixed with stale task state, and easy to bloat with
private details. This skill separates them:

| Category | Examples | Lands in |
|---|---|---|
| Coding principles | minimal change, no refactor unless asked, no rationale comments | personal profile |
| Communication style | restate a complex bug before fixing, no trailing questions | personal profile |
| Debugging workflow | reproduce → root cause → minimal fix → edge cases → verify | personal profile |
| Architecture preferences | reuse the module's state layer, respect layer boundaries | personal profile |
| Code patterns | approved implementations with the file that shows them | project memory |
| Change-management rules | never delete unwired code, wait for go before commit | personal profile |
| Skills and workflows | scaffolding, audits, PR review, release steps | team skills |
| Project knowledge | architecture, utilities, naming, tooling facts | project memory |
| Temporary context | current bugs, branches, open PRs, dated and expiring | project memory |
| Working-habit observations | context re-explained each session, credentials pasted, repeated manual steps | report only |
| Repeated prompts | anything typed twice in similar form → template or skill | report only |

Every recommendation is labelled **personal** or **team**, so you know what to keep for yourself and what
to propose to your teammates.

## Install

### Option 1 — plugin (one command)

Inside Claude Code:

```
/plugin marketplace add sumant22/claude-dev-profile
/plugin install dev-profile@claude-dev-profile
```

Plugin skills are namespaced, so the command is `/dev-profile:dev-profile`. Updates arrive with
`/plugin marketplace update claude-dev-profile`.

### Option 2 — copy the skill (plain `/dev-profile` command)

Personal, every project on this machine:

```bash
git clone https://github.com/sumant22/claude-dev-profile.git
cp -r claude-dev-profile/skills/dev-profile ~/.claude/skills/
```

Windows PowerShell:

```powershell
git clone https://github.com/sumant22/claude-dev-profile.git
Copy-Item -Recurse claude-dev-profile\skills\dev-profile "$env:USERPROFILE\.claude\skills\"
```

Team, one repository:

```bash
cp -r claude-dev-profile/skills/dev-profile <your-repo>/.claude/skills/
```

Start a new Claude Code session. `/dev-profile` appears in the skill list.

## Use

| Command | What happens |
|---|---|
| `/dev-profile` | Full analysis. Read-only. Produces the nine-part report below. |
| `/dev-profile apply` | Applies the plan you approved in the report. Run it in the same session as the report. |
| `/dev-profile privacy` | Only the privacy scan: credentials, emails, customer data stored where they should not be. |

### The report

1. Summary of how you work, five lines, including what already works well
2. Action table: Create / Update / Delete / Keep, one row per file, rule or skill, each labelled personal or team
3. Proposed global profile for `~/.claude/CLAUDE.md`, under 80 lines
4. Proposed memory index, grouped by category
5. Skills: existing ones with keep / update / merge / retire, plus proposed new ones with trigger sentences
6. Working-habit observations: what costs you time or risk today, and the one change that fixes each
7. Repeated prompts, each with a proposed reusable template or skill
8. Privacy findings
9. Open questions where the evidence is thin or conflicts

Review it. Edit anything. Then say **go**. Claude applies exactly the approved rows and lists the final files.

### Where the evidence comes from

You do not need a CLAUDE.md or curated memory. Claude Code already saves every session on your machine.
The skill reads, in order of usefulness:

1. Session transcripts under `~/.claude/projects/<project>/`
2. Prompt history at `~/.claude/history.jsonl`
3. Your own git commits in the repo
4. The codebase itself
5. Memory, CLAUDE.md, skills, hooks, if they exist
6. The current conversation

Transcripts may contain things you pasted, including secrets. The skill reads them for patterns only and
never copies a credential, email or customer record into the report or any file.

The skill runs inside your own Claude Code session. Transcripts are read the same way Claude reads any
other file you open with it. The skill sends nothing anywhere else, and writes nothing until you say **go**.

**New user with almost no history?** The skill runs a short interview instead of inventing a profile. Rules
from the interview are marked *stated, not yet observed* and upgrade to *confirmed* on a later run once the
transcripts show them in practice.

## Does it restrict Claude?

No. The profile's first principle is **analyse freely, act narrowly**. Rules limit what Claude changes
without asking, never what it notices or suggests. Better approaches, hidden risks and newer APIs still
arrive, in a short separate section after the requested work, so your fix stays exactly scoped and the ideas
are yours to take or leave. Open design questions are exempt from the minimal-change rules entirely.

## Does it break anything?

No. Analysis is read-only. Existing memory, skills and hooks keep working unchanged. After **go**, only the
rows you approved are touched. Temporary notes never become permanent rules without your explicit
confirmation.

## What is in this repo

```
.claude-plugin/plugin.json        plugin manifest
.claude-plugin/marketplace.json   marketplace listing, enables /plugin install
skills/dev-profile/SKILL.md       the skill. Copy this folder for a manual install.
templates/CLAUDE.md.example       a starting global profile you can edit by hand
CHANGELOG.md                      release notes
```

## Contributing

Issues and pull requests welcome. Keep the skill stack-neutral and free of personal data.

## License

MIT
