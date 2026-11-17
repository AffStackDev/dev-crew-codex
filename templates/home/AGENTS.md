# Home base: instructions for the agents

This folder is the home base of <name>'s development setup: the facts and records every chat and agent reads first.
It holds no project code, no secrets and no customer data. Made <date> with the Dev Crew for Codex kit. Codex loads
this file by itself when a chat starts here; Claude loads it through `CLAUDE.md`.

## What's here

| File | What | Who writes it |
|---|---|---|
| `MACHINE.md` | Checked facts about this computer, each with its date and how it was checked | Any chat, when it checks something new |
| `WORKSPACES.md` | Every project: folder, local-data folder, branches, Docker slots, ports, who works there | The chat that sets a project up or changes these |
| `CREW-STATUS.md` | One row per active Codex chat or Claude session | Each agent updates only its own row |
| `TOOLS.md` | Every tool: how it's reached, guardrails, who drives it | The chat that connects or changes a tool |
| `OWNER-QUESTIONS.md` | Questions for the owner that belong to no single project | Codex |
| `records/decisions.csv` | The owner's decisions that apply everywhere | Codex |
| `exchange/` | Codex <-> Claude exchange files, one per project or topic | Both, append only, newest entry first |
| `lessons/` | Lessons, one file each: `codex/`, `claude/`, `shared/` | The agent that learned it |
| `mirrors/` | Copies of `~/.codex/AGENTS.md` and `~/.claude/CLAUDE.md`, for their history | Whoever changes a global file |
| `docs/` | The universal rule file (if adopted), the setup log | — |
| `scripts/` | `watch-exchange`, `check-setup` | — |

## Rules for this folder

- Re-read a file immediately before writing it; other agents may have changed it. Preserve their entries.
- Commit at stopping points (`git add -A`, a short message). If Git reports a lock (`index.lock`), another agent is
  committing: wait a few seconds and try again; never delete the lock while a Git process runs.
- Decision numbers here are `D-001`… and apply everywhere. Projects keep their own sequences.
- Decisions and questions here are written by Codex. Claude puts a ready row in an exchange entry and Codex records it.
- Nothing here is pushed anywhere unless the owner chose a private GitHub backup for it (see `WORKSPACES.md`).
- The owner's global rules are in `~/.codex/AGENTS.md` (and `~/.claude/CLAUDE.md` for Claude); this file only
  explains the folder.
