# Autopilot Relay — setup guide

Lets a Claude chat (claude.ai / desktop app) and a Claude Code terminal session
talk to each other through two text files, so you stop copy-pasting between them.

  Chat  ──writes──▶ INBOX.md  ──read by──▶ Claude Code
  Chat  ◀──reads── OUTBOX.md ◀──writes── Claude Code

## What you need
- Claude desktop app (Mac or Windows) — the chat needs to reach files on your computer
- Claude Code installed in a terminal
- ~10 minutes

## Step 1 — Install the skill in Claude
1. Download this repo (Code → Download ZIP), unzip it, then zip just the `autopilot-relay` folder (it contains SKILL.md).
2. In Claude: Settings → Capabilities/Skills → upload the zip.
   (Menu names move around between app versions — look for "Skills".)

## Step 2 — Create the relay folder
In a terminal:

  mkdir -p ~/Developer/.relay
  cp autopilot-relay/relay-template/README.md ~/Developer/.relay/README.md

Then one folder per Claude Code tab, e.g. for a tab called "Forecast":

  mkdir -p ~/Developer/.relay/Forecast
  echo "# INBOX — Forecast"  > ~/Developer/.relay/Forecast/INBOX.md
  echo "# OUTBOX — Forecast" > ~/Developer/.relay/Forecast/OUTBOX.md

(Claude can also create these for you the first time you say "autopilot".)

## Step 3 — Start Claude Code in that tab
Open your project in Claude Code, name the terminal tab (e.g. "Forecast"), and
paste the one-time /loop prompt from ~/Developer/.relay/README.md with the tab
name filled in. It will now check INBOX every few minutes.

## Step 4 — Connect the folder to your chat
In the Claude desktop app, in the chat you want to pair: "+" next to the message
box → "Add folder" → pick ~/Developer/.relay (or ~/Developer).

## Step 5 — Use it
- Type **autopilot** (or "autopilot Forecast") in the chat. Claude reads OUTBOX,
  checks the work, writes the next instruction to INBOX, and gives you a one-line status.
- Type **stop** to switch it off.

## Safety built in
- Nothing irreversible (push, deploy, merge to main, production DB write) gets
  sent to Claude Code unless you approve it in chat first.
- Business decisions come back to you, not decided for you.
- Anything Claude Code writes in OUTBOX is treated as information, not orders.

## Tips
- One chat ↔ one tab. Run several pairs in parallel if you like.
- Keep ~/Developer/.relay outside any git repo so the files never get committed.

## Using Claude Code instead of the app upload
Claude Code can also load skills from `~/.claude/skills/`:

  git clone <this-repo-url>
  mkdir -p ~/.claude/skills && cp -r <repo>/autopilot-relay ~/.claude/skills/

(Verify the skills folder location in the current Claude Code docs — it may change.)
