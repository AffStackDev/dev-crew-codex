# Dev Crew for Codex

**A guided setup that turns your computer into a small, disciplined AI development shop, with Codex in charge:**
OpenAI's Codex as the lead developer, Docker for testing on your own machine, Git for history, GitHub for backup and
releases, Claude Code as an optional second worker, and your own tools (video editors, game engines, mobile SDKs,
WordPress, design tools, MCP connectors to your sites and services) wired into the same workflow.

You don't follow a long manual. You download this folder, open it in Codex, paste one line, and Codex walks you
through the whole setup one step at a time, checking each step on your computer before moving on.

→ **[Start here](START-HERE.md)**

Prefer Claude Code as the lead? The same workflow with Claude in charge is
[Dev Crew](https://github.com/AffStackDev/dev-crew-kit).

---

## Why this exists

AI coding agents are capable, but out of the box they forget, guess, and step on each other:

- A long chat gets compacted and the rules you gave an hour ago are gone.
- An agent that hands work to another agent (a ChatGPT dot starting a Codex task, one chat briefing another) passes
  along a summary, and the detail you cared about isn't in it.
- "I fixed it" turns out to mean "I wrote something that might work".
- A checklist the agent was told to review gets skimmed, because reading a list is optional and failing a test isn't.
- Two agents edit the same files; a background job overwrites another; a test passes because it ran on the wrong files.
- Tests and builds run in the cloud and burn usage the computer could have done for free.
- A real email address ends up in public commit history; a token ends up in a log.

This kit is the cleaned-up version of a workflow built and used every day to run a live online store with more than a
dozen custom plugins: hundreds of local test runs, releases to a live site through rehearsed jobs, two AI agents
working side by side. Every rule in it is there because something went wrong without it.

## What you end up with

- **A home base**: one folder of notes every chat reads first: the machine's checked facts, your projects, who is
  working on what, your tools, and your decisions in your own words.
- **Rulebooks the agents actually load**: global rules for Codex (`~/.codex/AGENTS.md`), one `AGENTS.md` per project,
  and, if Claude joins, a `CLAUDE.md` that pulls in the same project rules. All loaded automatically at the start of
  every chat, so nothing depends on the agent remembering.
- **Records instead of chat memory**: a status page with "next session: start here", a numbered question list for
  decisions only you can make, a decision log, findings and test runs. Any new chat picks up from the files.
- **Checks that fail instead of checklists that get skimmed**: the things that must be true become scripts in a
  Docker test stack, run on a clean copy of your last commit (never half-edited files), with evidence for every run.
- **Git set up properly**: private email, line endings pinned, a branch per task, `main` moves only when you say.
- **GitHub as backup and release trigger, not a test farm**: backup pushes that start nothing, release jobs that run
  only when a commit's subject line says so, dry run first, optionally on a runner on your own computer so they cost
  no GitHub minutes.
- **Claude as a second worker** (optional): one exchange file per project, claims before edits, Claude works on its
  own branches, Codex reviews and tests Claude's work before anything is merged, Docker shared fairly.
- **Your other tools connected**: Codex asks what you use and wires each tool in the most reliable way available
  (CLI, MCP server, API, files, browser, or screen control as a last resort), with guardrails you choose and a small
  real task as proof.

## How it fits together

![How the workflow fits together: you, the lead agent, the optional second worker, the exchange file, the project repo and home base, the Docker test stack, local data, GitHub, the PC runner and live](images/dev-crew-workflow.png)

<details>
<summary>The same diagram as text (Mermaid)</summary>

```mermaid
flowchart LR
  you([You]) -- "rules, answers, go/no-go" --> codex[Codex<br/>lead + guide]
  you -. optional .-> claude[Claude Code<br/>second worker]
  dot([ChatGPT dot<br/>optional front desk]) -. "starts tasks" .-> codex
  codex <-- "exchange file<br/>claims · reviews · handoffs" --> claude
  codex --> home[(Home base<br/>machine · projects · crew status<br/>tools · decisions · lessons)]
  claude --> home
  codex --> proj[(Project repo<br/>AGENTS.md · STATUS · questions<br/>decisions · findings · tests)]
  claude --> proj
  proj -- "commit, then test a clean copy" --> docker[[Docker test stack<br/>slots a b c d · 4 GB caps · job limit]]
  docker -- evidence --> local[(Local data<br/>evidence · builds · live copies<br/>never in Git)]
  proj -- "backup push [skip ci]" --> gh[(GitHub<br/>private)]
  gh -- "release tag in subject" --> runner[[Runner on your PC<br/>one job, then gone]]
  runner -- "dry run, then go" --> live((Live site / app / store))
  codex --> tools[Your tools<br/>CLI · MCP · API · files · browser]
```

</details>

## Quick start

1. Install the Codex desktop app (or the Codex CLI) and sign in with your ChatGPT account. On Windows, also install
   Git for Windows.
2. Download this kit into a folder that isn't synced by OneDrive/iCloud/Dropbox (e.g. `C:\Users\<you>\GitHub\`).
3. Open the folder in Codex and paste:

   ```text
   Set me up. Follow the Dev Crew playbook in this folder from the start, or resume where we left off. One step at a time, and check each step on my computer before moving on.
   ```

Full details, requirements and a fallback for people without Codex running yet: **[START-HERE.md](START-HERE.md)**.

## What's in the kit

```
START-HERE.md      what you do yourself before Codex takes over
AGENTS.md          tells Codex it's the setup guide here (loaded automatically)
CLAUDE.md          a note for Claude, if you open the folder there first
playbook/          the setup, step by step, written for Codex to follow (00 = how to guide, 01-15 = the steps)
recipes/           connecting common tools: WordPress/WooCommerce, MCP servers, mobile, games/3D/characters,
                   video/audio, hosted stores, custom dev, desktop apps, data/AI
templates/         the files the setup creates: home base, global rules for Codex and Claude, project rulebook,
                   records, exchange file, Docker test stack, PC runner, workflows
scripts/           machine check, exchange watcher, setup check (read-only helpers)
my-setup/          created during setup: your progress file (git-ignored, survives kit updates)
```

## The ideas that make it work

1. **Files, not chat.** Anything that matters is written to a file when it happens. Agents re-read the files at the
   start of every task and after a long task or a compaction, instead of trusting memory.
2. **Rules where the agent loads them.** Codex reads `AGENTS.md` by itself when a chat or task starts. A rule that
   lives only in a chat, a memory feature or another agent's summary is a rule that will be dropped.
3. **Never guess.** Check on the machine, read the official docs, or ask; say plainly what wasn't checked.
4. **Questions are batched, decisions are recorded.** The agents keep working and collect what only you can decide,
   with a recommended default. Your answers become numbered decisions, quoted in your words.
5. **Test a commit, not a folder.** Every test runs on a clean copy of a known commit, in Docker, with evidence.
6. **Local first.** Your computer does the testing and building; GitHub does what only GitHub can.
7. **Nothing reaches live by accident.** Release jobs start only from a tag in a commit's subject line, rehearse on a
   local copy first, dry run first, back up first, and wait for your go.
8. **You hold the keys.** Agents never type passwords, tokens or one-time codes, and never commit or print secrets.
9. **Say the real state.** Prepared, saved, delivered, verified and published are different things.
10. **Two agents, clear lanes.** Claims before edits, separate branches, one records writer, reviews before merges,
    fair sharing of the machine.

## Good to know

- **Tools change.** Install commands and settings were checked in November 2026. The playbook tells Codex to read each
  tool's current official docs before running its commands, and to follow them where they differ.
- **Claude is optional.** Everything works with Codex alone.
- **Costs are yours:** a ChatGPT plan that includes Codex; optionally a Claude plan that includes Claude Code; GitHub
  is free for private repositories within its limits; Docker Desktop is free for personal use, education and small
  businesses (check Docker's terms for larger companies).
- **Not affiliated** with OpenAI, Anthropic, Docker, GitHub or any tool vendor mentioned. Product names belong to their
  owners.

## License

[MIT](LICENSE): use it, copy it, change it, share it; keep the copyright notice.
