# 10 · Claude Code as a second worker (optional)

**What and why:** Claude Code is Anthropic's coding agent. In this setup it's a second worker: it takes its own tasks
on its own branches, and Codex reviews its work before anything is merged or released (step 11). A second agent is
useful for a second opinion, for parallel work, and for things one agent does better than the other. The hard part
isn't installing it. It's making Claude follow the same rules as Codex and **not forget them**, which is what happens
when rules live only in a chat. The cure is the same as for Codex: everything that matters is in files that load
automatically or that it re-reads at fixed moments.

Skip this step if the user said no to Claude in step 2.

## 1. Install and sign in

Read Anthropic's current Claude Code docs first (rule 2 in 00); start from code.claude.com/docs. Options in
November 2026:

- the **Claude desktop app** (Windows and macOS, from claude.ai/download): its **Code** tab is Claude Code; or
- the **Claude Code CLI** (`claude`), installed the way the current docs say.

The user signs in with their Claude account; you don't handle the sign-in. Model and plan are the user's choices.

Verify: the app's Code tab opens on a folder, or the CLI answers `claude --version`.

## 2. How Claude loads its instructions

Checked against Anthropic's docs in November 2026; re-check the current "memory" / `CLAUDE.md` page and note any
change in PROGRESS:

- Claude Code reads **`CLAUDE.md`** files, not `AGENTS.md`: the user-level one at `~/.claude/CLAUDE.md`
  (`C:\Users\<you>\.claude\CLAUDE.md`) in every session in every folder, then the project's own `CLAUDE.md`.
- A `CLAUDE.md` can pull another file in with an import line: `@AGENTS.md` on a line of its own. Relative paths are
  read from the file that contains the import. An import inside a code block is ignored. An import of a file outside
  the folder Claude was opened in asks the user to allow it the first time.
- That's how this setup keeps one rulebook: each project's real rules are in its `AGENTS.md` (Codex loads it by
  itself), and the project's `CLAUDE.md` is a short adapter that imports `AGENTS.md` and adds Claude's lane. One
  set of rules, both agents, no copies to drift apart.
- Check the current docs for whether Claude Code reads `AGENTS.md` on its own in their version. Keep the import
  either way: it's explicit, and it works.

## 3. Claude's global rules (`~/.claude/CLAUDE.md`)

1. If the file exists, read it and show the user; plan to **add** a section, not replace it. Show the exact change
   before saving.
2. Write it from `templates/claude/global-CLAUDE.md`, filled in with:
   - the path of the home base;
   - the user's rules in force, **word for word** from the home base's `records/decisions.csv`, each with its date
     and D-number (the same rules Codex's global file carries);
   - Claude's lane: its own worktrees on `claude/<task>` branches, Codex is the records writer and the merger, Codex
     reviews its work;
   - the "start here" routine every session follows;
   - the last line `END OF GLOBAL CLAUDE.md (version <date and time from the clock>)`, updated on every change.
3. Keep it short: under about 8 KB. Long material goes into lessons, with one index line.
4. Mirror: copy it to `crew-home/mirrors/claude-global-CLAUDE.md` and commit, every time it changes.
5. Tell the user: this file now applies to **every** Claude Code session on this computer. Sessions already open keep
   their old instructions until restarted.

## 4. The project adapter (`<project>/CLAUDE.md`)

Step 6 put one in each project from `templates/project/CLAUDE.md`. Check it:

- starts with the import line `@AGENTS.md`, so the project's shared rules load with it;
- says Claude works in its **own worktree** on `claude/<task>` branches, never in Codex's checkouts or on the work
  branch directly;
- says Codex writes the shared records on the work branch: Claude proposes question and decision rows in the exchange
  file and Codex assigns the numbers, so two agents never take the same number;
- gives the exchange file's absolute path (step 11) and Claude's Docker slots (step 7).

## 5. Claude's records and lessons

- `crew-home/CREW-STATUS.md`: Claude sessions get rows there like Codex chats. A session updates only its own row.
- Lessons: if the user's Claude Code has a built-in memory folder (its own instructions mention a memory directory it
  writes to, with an index file), Claude's lessons go there; tell the user where it is. If not, they go in
  `crew-home/lessons/claude/<slug>.md` (template `templates/home/lessons/LESSON-TEMPLATE.md`), each with an index line
  in `~/.claude/CLAUDE.md`. Shared lessons stay in `crew-home/lessons/shared/`.

## 6. Check that it loaded

Open a **fresh** Claude Code session in the project folder and ask, with no other briefing: "Quote the last line of
your global CLAUDE.md, and name the first item under 'Next session: start here' in this project's STATUS." It passes
when it quotes the END line and the STATUS item, and states the user's rules from `AGENTS.md` (which proves the import
loaded). In the CLI, `/memory` also lists the files it loaded. Record the result.

## 7. Permissions

- Claude Code asks before running commands and editing files, depending on the permission mode the user picks. The
  user can allow common read-only commands for the project; never allow destructive commands blanket-wide.
- Claude Code can run long commands in the background and is woken when they finish. Step 11 uses that for the
  exchange watcher.

## Record

PROGRESS and `crew-home/WORKSPACES.md` (who works where), `crew-home/TOOLS.md` (a Claude Code card: version, app/CLI,
plan not recorded beyond "user's plan"), the loading check result. Commit the home base. Mark step 10 done.
