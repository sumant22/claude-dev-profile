# Changelog

## 1.1.0 — 2026-10-04

- **Backup and undo:** on **go**, every file the plan changes is copied to `~/.claude/dev-profile-backups/<date-time>/` first. New `/dev-profile undo` restores the most recent backup
- **Conflict detection:** new report section listing rules that contradict each other across global and repo `CLAUDE.md`, `AGENTS.md`, memory and skills, each with a proposed resolution
- **Context cost:** the summary now shows roughly how much text loads into every session before and after the plan, and proposes moving long detail out of `CLAUDE.md`
- **Global or project test:** a personal rule goes to the global profile only when the same correction appears in two or more projects; otherwise it stays in that project's memory
- **Rule quality test:** vague rules ("write clean code") are rewritten into the concrete behaviour the evidence shows, or dropped
- `AGENTS.md` is now read alongside `CLAUDE.md`

## 1.0.1 — 2026-10-04

- Template: added an iOS/SwiftUI example beside Flutter, React and PHP
- README: install notes (use a terminal for `/plugin` commands, choose one install method)
- README: correct plugin update command and auto-update steps
- README: note to back up `~/.claude/CLAUDE.md`, memory and skills before saying **go**

## 1.0.0 — 2026-10-01

- First release of the `dev-profile` skill: `/dev-profile`, `/dev-profile apply`, `/dev-profile privacy`
- Global profile template at `templates/CLAUDE.md.example`, with Flutter, React and PHP examples
- One-command install through the Claude Code plugin marketplace
