# Start here

You'll do four small things yourself. After that, Codex walks you through the rest, one step at a time, and checks
each step on your computer before moving on.

## 1. What you need before you begin

| You need | Why | Notes |
|---|---|---|
| A Windows 10/11, macOS or Linux computer | Everything runs on your machine | Codex checks each step on your machine as it goes. 16 GB of RAM works, 32 GB is comfortable. Keep 40 GB of disk free. |
| A ChatGPT plan that includes Codex | Codex does the setup and the daily work | See OpenAI's pricing page for which plans include Codex today. |
| Codex: the desktop app or the CLI | Where Codex works on your files | The **Codex desktop app** (Windows, macOS), or the **Codex CLI** (`npm install -g @openai/codex`, needs Node.js; the CLI is the way on Linux). Start from developers.openai.com/codex. Sign in with your ChatGPT account. |
| **Windows only:** Git for Windows | Codex runs Git commands, and the setup builds on Git | **git-scm.com/download/win**. The installer's default choices are fine. |
| *Optional:* a Claude plan that includes Claude Code | Only if you want Claude as a second worker | Codex sets it up in step 10. Everything else works with Codex alone. |
| *Optional:* a GitHub account | Off-site backup, review, phone access, releases | Free at github.com. Codex helps you connect it in step 8. You can make the account then. |

## 2. Put this kit on your computer

Put it in a plain folder that **is not synced by OneDrive, iCloud Drive, Dropbox or Google Drive**. Sync tools fight
with Git and Docker over the same files. On Windows, `C:\Users\<you>\Documents` is often inside OneDrive without you
knowing, so use `C:\Users\<you>\GitHub\` instead.

Either:

- **With Git** (in a terminal):

  ```bash
  git clone https://github.com/AffStackDev/dev-crew-codex.git
  ```

- **Or as a ZIP:** on the kit's GitHub page, click **Code**, then **Download ZIP**, and unzip it into
  `C:\Users\<you>\GitHub\` (Mac/Linux: `~/GitHub/`).

## 3. Open it in Codex

Open the Codex desktop app and choose the kit's folder as the folder to work in. (Using the CLI instead? Open a
terminal in the kit's folder and run `codex`.) Codex asks whether to trust the folder the first time: say yes for this
folder.

## 4. Paste this

```text
Set me up. Follow the Dev Crew playbook in this folder from the start, or resume where we left off. One step at a time, and check each step on my computer before moving on.
```

Codex reads the kit's `AGENTS.md` by itself and takes it from there. You can stop whenever you like: progress is
saved in `my-setup/PROGRESS.md`, and next time you paste the same line (or just say "continue") and Codex picks up
where you left off.

Codex runs commands in a sandbox. Some setup steps (installs, Docker, writing outside this folder) may need your
approval, or Codex may hand you a command to run yourself. That's expected: you stay in charge of your machine.

## What the setup covers, in order

1. Checks your computer (read only) and tells you what's there and what's missing.
2. Asks you one batch of questions: what you work on, how you want the AI to work, what to set up first.
3. Installs the core tools, each with your OK: Git, GitHub CLI, Docker Desktop (and WSL 2 on Windows).
4. Sets up Git properly, and teaches you the five Git ideas you'll actually use (skip it if you know Git).
5. Makes your **home base** and Codex's **global rules**: notes and rules every chat reads first.
6. Sets up your first project with its own rulebook, status page, question list and decision log.
7. Builds a **Docker test stack** so tests and builds run on a clean copy of your latest commit, not on your files.
8. Connects **GitHub**: private backup, safe pushes that start nothing by accident, releases only on purpose.
9. *(Optional)* A **GitHub runner on your own computer**, so release jobs cost no GitHub minutes.
10. *(Optional)* Installs and configures **Claude Code** as a second worker that follows the same rules.
11. *(Optional)* The **Codex ↔ Claude exchange**: how the two hand work back and forth, review each other, and share
    Docker.
12. **Your other tools:** video editing, game and 3D, mobile SDKs, WordPress, design tools, MCP servers for your
    websites and services, whatever you use. Codex asks, then wires each one into the workflow and tests it.
13. **Proves it all works** with a real change, end to end.
14. Shows you how a normal day goes, how to use it from your phone, and how to use it with a ChatGPT dot.
15. Troubleshooting, for later.

## How long it takes

The core (steps 1–8) is usually one to two hours, most of it waiting on installs and one restart on Windows. Claude
adds about half an hour. Each extra tool depends on the tool. Nothing has to be done in one sitting.

## No Codex running yet? Start in a regular ChatGPT chat

If you're stuck before step 3, paste this into a normal ChatGPT chat. It walks you up to the point where you can paste
the line from step 4.

```text
I want to set up the "Dev Crew for Codex" local development workflow (Codex + Docker + Git + GitHub, optionally Claude Code). The kit is at https://github.com/AffStackDev/dev-crew-codex. I don't have Codex running yet. First ask me which operating system I use. Then walk me through, one step at a time and waiting for me to confirm each one: installing the Codex desktop app (or the Codex CLI) from OpenAI's official Codex page and signing in with my ChatGPT account; on Windows, installing Git for Windows from git-scm.com; downloading the kit into a folder that is not synced by OneDrive, iCloud, Dropbox or Google Drive (on Windows, C:\Users\<me>\GitHub\); and opening that folder in Codex. When Codex is open on the kit folder, tell me to paste the line from step 4 of START-HERE.md there. Don't go beyond that point.
```
