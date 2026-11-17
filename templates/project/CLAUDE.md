@AGENTS.md

# <Project name>: Claude adapter

The line above imports `AGENTS.md`, this project's rulebook: its rules apply to you as well (both agents follow the
same project rules). Then read `docs/STATUS.md`, `docs/records/decisions.csv`, `docs/OWNER-QUESTIONS.md` and the
exchange file. Your global rules are in `~/.claude/CLAUDE.md`.

For you in this project:

- **Where you work:** your own worktree on `claude/<task>` branches made from the work branch `<work>`. Never edit
  Codex's checkouts, and never commit to `<work>` or `main`.
- **Claims and handoffs:** in the exchange file `<home base>/exchange/<project>.md` (absolute path; newest entry
  first). Claim files before editing them; say "ready for review" with commits, evidence folders, checks and what's
  not checked.
- **Records:** Codex writes the records on the work branch. Put question and decision rows, ready to paste, in your
  exchange entry; Codex assigns the numbers. Don't pick Q, D or F numbers yourself.
- **Testing:** `scripts\local.cmd` with `SLOT=c` or `SLOT=d` and `CREW_AGENT=claude` (your slots). Commit before
  testing; check `docker ps` before each start; `CREW_MAX_JOBS` is shared with Codex.
- **Shared services** (<local copy of the live site, demo environment>): claim in the exchange before use.
- **Review:** Codex reviews your branch before it is merged or released; you make the changes the review asks for on
  your own branch, then say it's ready again.
- **Never:** release, deploy, publish, push to GitHub or merge, unless the owner tells you directly for that step.
