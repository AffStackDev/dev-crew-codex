# 05 · The home base, and Codex's global rules

**What and why:** one small folder of notes that every chat (and Claude, if it joins) reads first: the machine's
facts, the list of projects, who is working on what, the tools, and the user's decisions. Chats get long, get
compacted and get closed; these files don't. Anything that matters is written here or in a project's records when it
happens, never left only in a chat. Then Codex's own global rules file, which loads in every chat on this computer,
so the user's rules don't depend on Codex remembering them.

## Part 1: the folder

Default path: `C:\Users\<you>\GitHub\crew-home` (Mac/Linux `~/GitHub/crew-home`), next to the projects, outside any
synced folder. Use the folders chosen in step 2.

1. Create it and `git init` it. The `.git` matters: Codex treats the nearest folder with `.git` as the project root
   (Part 2). If the user chose a per-repository identity in step 4, set it here (`git config user.name` /
   `user.email` inside the folder).
2. Copy the templates from `templates/home/` into it, keeping the structure:

   ```
   crew-home/
     AGENTS.md                what this folder is and how chats use it (Codex loads it here by itself)
     CLAUDE.md                the same for Claude, if it joins (it imports AGENTS.md)
     MACHINE.md               checked facts about this computer, with dates
     WORKSPACES.md            every project: folder, local-data folder, Docker slots, ports, who works there
     CREW-STATUS.md           one row per active Codex chat or Claude session: what it's doing and owns right now
     TOOLS.md                 every tool and how the agents reach it (filled in step 12)
     OWNER-QUESTIONS.md       questions only the user can answer that belong to no single project
     records/decisions.csv    the user's decisions that apply everywhere (D-001, D-002, ...)
     exchange/                Codex <-> Claude exchange files (step 11)
     lessons/                 lessons learned, one file each (see Part 3)
     mirrors/                 copies of the global rule files, for their history (Part 2)
     docs/UNIVERSAL-PHASE-VERSIONING-AND-HANDOFF-RULE.md   (if adopted in step 2)
     scripts/                 watch-exchange.ps1 / .sh, check-setup.ps1 (copied from the kit's scripts/)
     .gitignore  .gitattributes
   ```

3. Fill them in from PROGRESS:
   - `MACHINE.md`: the step 1 report and everything installed in step 3, each with date and source. Add a line
     "Not checked:" for anything you didn't verify.
   - `records/decisions.csv`: one row per answer from step 2's section C and the other choices that apply everywhere
     (Docker limits, folders, GitHub yes/no, Claude yes/no, the versioning rule). `D-001` onward. Quote the user's
     words in `owner_words`; date and time from step 2's record.
   - `WORKSPACES.md`: the projects named in step 2 (filled properly in step 6).
   - `CREW-STATUS.md`: a row for this setup chat.
4. The local-data folder from step 2 (`C:\Users\<you>\Dev-local\`): create it. It holds test evidence, builds and
   any copy of a live system, per project. It is never inside a Git repository and never synced to the cloud. Add a
   `README.txt` there saying exactly that.
5. First commit: `git add -A` then `git commit -m "Home base"`. Show the user `git log --oneline` and explain that
   this folder's history is now kept.

The home base is local. It may get a private GitHub backup in step 8 like any repository (the user decides). It
never holds secrets or customer data.

## Part 2: Codex's global rules (`~/.codex/AGENTS.md`)

### How Codex loads its instructions

Checked against OpenAI's docs in November 2026; re-check the current docs ("AGENTS.md" and "config" pages) and note
any change in PROGRESS under "Kit differences":

- **Global file:** `AGENTS.md` in Codex's home folder (`CODEX_HOME`, default `~/.codex`, on Windows
  `C:\Users\<you>\.codex`). Loaded first, in every chat, in every folder. If a non-empty `AGENTS.override.md` exists
  there, it is used **instead**. Don't create one.
- **Project files:** then one `AGENTS.md` per folder from the project root down to the working folder, joined in that
  order; later (deeper) files win where they conflict.
- **Project root** = the nearest folder with `.git`. If there isn't one, Codex checks only the current folder. That
  means a folder **without** `.git` doesn't pass its `AGENTS.md` down to subfolders (found in practice: rules in a
  non-Git parent folder never reached chats started in its dated task subfolders). Put anything that must apply
  everywhere into the global file's routing table instead.
- **Size limit:** the joined files stop at `project_doc_max_bytes`, 32 KiB by default. Keep the global file under about
  20 KB so global + project still fit.
- The chain is read when a chat or run **starts**. The docs don't promise it's re-sent after the chat is compacted.
  So the global file tells Codex to re-read its status and decisions files after long tasks and compactions.
- Codex's built-in **memories** feature is optional and builds memories in the background from earlier chats. The
  docs say required rules belong in `AGENTS.md`, not memories. It doesn't replace the files in this setup. Turning it
  on is the user's choice.

### Write the global file

1. If `~/.codex/AGENTS.md` exists, read it and show the user; plan to merge, not replace. Show the exact change
   before saving.
2. Write it from `templates/codex/global-AGENTS.md`, filled in:
   - **Start here** routine (every chat, resume, compaction, long-task checkpoint): find the working folder's row in
     the routing table; read that project's `docs/STATUS.md`, decisions and questions; read `crew-home/CREW-STATUS.md`;
     read the lessons that fit; before anything external, re-read the decisions and check the step follows the
     established workflow.
   - **Rules in force**: a five-line summary at the top (so a fresh chat can state them), then the user's rules
     **word for word** from the home base's `records/decisions.csv`, each with its date. Where a rule names a file
     (`OWNER-QUESTIONS.md`), say which file that means in a project.
   - **Conflict check**: if a new request conflicts with a recorded decision, a hold or the established workflow,
     don't draft or act yet; quote the decision, say what the workflow would do, and ask whether the user is
     overriding it. (This was the one behaviour check that failed in the original setup until this rule was added: a
     fresh chat read a new request as permission and drafted ten posts that an earlier decision had ruled out.)
   - **Write as you go** table (Part 4).
   - **Routing table**: working folder → project rulebook → STATUS file → decisions/questions files → who else works
     there.
   - **Local first**, **secrets**, **times from the clock**, **saved vs delivered vs verified**, **ownership**.
   - **Lesson index**: one line per file in `crew-home/lessons/codex/` and `crew-home/lessons/shared/`.
   - The universal rule (if adopted).
   - **Last line:** `END OF GLOBAL AGENTS.md (version <date and time from the clock>)`. If that line shows up in
     Codex's loaded prompt (below), the whole file loaded. Update it on every change.
3. Mirror: copy it to `crew-home/mirrors/codex-global-AGENTS.md` and commit, every time it changes. The home base's
   Git history is then the history of the global rules.
4. Tell the user: this file now applies to **every** Codex chat on this computer, in every folder.

### Check that it loaded

If the CLI has the command (it did in codex-cli 0.165 and 0.167, November 2026; check `codex --help` and
`codex debug --help`):

```bash
codex debug prompt-input "loading check only"
```

It prints what Codex would send to the model without calling it, so it's free. Run it from the home base and from an
unrelated folder (e.g. Downloads); step 6 repeats it from the project. Search the output for the END line. Save the
outputs in `Dev-local\codex-checks\<stamp>\` and record the result.

On Windows the desktop app may keep its own copy of the CLI under `%LOCALAPPDATA%\OpenAI\Codex\bin\` (in a
subfolder); `where.exe codex` shows what's on PATH. If the command doesn't exist in their version, use the behaviour
checks in step 13 instead.

### Permissions and sandbox

- Codex asks to trust a folder the first time; trusted folders are listed in `~/.codex/config.toml`. Trust only the
  user's project folders and the home base.
- Codex's Windows sandbox runs commands as a restricted account that may not be able to read some folders. In the
  original setup, a sandboxed test run got "Access is denied" on a project folder while the desktop app could read
  it. If Codex can't read a project, check from the desktop app before changing anything, and record what you find.
- Docker from Codex: it needs to reach Docker Desktop. If its sandbox blocks that, the user decides whether to allow
  it in Codex's settings; don't change Codex's security settings yourself.

### Open chats

Chats that were already open keep their old instructions. Tell the user: start fresh Codex chats, or tell each open
one "re-read your global AGENTS.md and the files it points to". Record which.

## Part 3: memory and lessons

Two kinds of memory, both files:

- **Records** (status, questions, decisions, findings, test runs): what is true and what was decided. Per project in
  the project's `docs/`, cross-project in the home base. Step 6 covers them.
- **Lessons**: what an agent learned the hard way and must not relearn. "PowerShell 5.1 reads BOM-less scripts as
  ANSI." "The user wants screenshots of every UI change." One small file per lesson, with an index line.

Codex's lessons go in `crew-home/lessons/codex/<slug>.md` (template `templates/codex/lesson.md`), each with one index
line in the global `AGENTS.md` under "Lessons", so they're listed in every chat. Claude's lessons, if Claude joins,
are covered in step 10. A lesson that applies to both agents goes in `crew-home/lessons/shared/` and both global rule
files index it.

What deserves a lesson: a correction from the user, a mistake that cost real time, a fact about this machine or a tool
that isn't obvious and will come up again. What doesn't: what the code or Git history already says, one-off task
details, status (that's what STATUS is for).

## Part 4: the records habit (explain it to the user once)

| When | Write |
|---|---|
| The user answers a question or gives a rule | A decision row (next D-number) with their words and the time; mark the question answered; act on it |
| A decision only the user can make | A question row in OWNER-QUESTIONS (next Q-number, asked time, plain question, why it matters, what waits, recommended default), batched; then keep working on other things |
| A step finishes, before anything external, at each stopping point, before saying "done", before the context runs low | Rewrite the project's `docs/STATUS.md` ("Next session: start here" first), and your row in `crew-home/CREW-STATUS.md` |
| The user corrects you, or you learn something the next chat needs | A lesson and its index line, in the same turn |
| A long task ends, or the chat was compacted | Re-read STATUS and the decisions before the next action |

Silence is never consent: an unanswered question waits; its default is a recommendation, not an approval.

## Record

PROGRESS: the home base path, the files made, the commit id, the global `AGENTS.md` change (new or merged), the
loading check result, `crew-home/TOOLS.md` (a Codex card: version, app/CLI, plan not recorded beyond "user's plan").
Mark step 5 done.
