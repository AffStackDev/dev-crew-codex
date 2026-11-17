# Dev Crew for Codex: note for Claude

This folder is a setup kit meant to be run by **Codex**, which walks the user through the whole setup (see
`AGENTS.md` and `playbook/`). In this setup Claude Code is an optional second worker. The Claude part of the setup is
`playbook/10-claude.md` and `playbook/11-codex-claude-exchange.md`; Codex does it with the user, and the files it
installs for you are `templates/claude/global-CLAUDE.md` (your global `CLAUDE.md`) and `templates/project/CLAUDE.md`
(per project).

If the user opened this folder in Claude Code first:

1. Tell them the guided setup is written for Codex (the Codex desktop app or the `codex` CLI) and point them to
   `START-HERE.md`. If they'd rather have Claude lead, the original kit with Claude as the lead is at
   https://github.com/AffStackDev/dev-crew-kit.
2. If they want you to do the setup instead, you may follow `playbook/` yourself. Read `playbook/00-guide-rules.md`
   first and apply it as written; where a step says "Codex", it means whichever agent is guiding. Keep progress in
   `my-setup/PROGRESS.md` exactly as the playbook describes, so Codex can take over later.
3. Don't edit the kit's own files unless the user asks you to change the kit.

Rules that apply either way: don't guess (check or ask); the user types every password, token, key and one-time code;
ask before each install or download; change nothing outside the folders this setup creates unless the user asks.
