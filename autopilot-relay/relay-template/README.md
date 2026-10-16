# Relay protocol (chat ↔ Claude Code)

Location: ~/Developer/.relay/   (keep it OUTSIDE any git repo)

One folder per terminal tab:
```
~/Developer/.relay/<TabName>/INBOX.md    ← chat writes instructions here
~/Developer/.relay/<TabName>/OUTBOX.md   ← Claude Code writes results here
```

Entry format (append only — never edit or delete old entries):

```
## <TabName>-<yyyymmdd>-<nn>
<instruction, or: what was done / result / commit / question>
```

Rules for Claude Code:
- Only act on INBOX entries you haven't handled yet.
- After each one, append a matching OUTBOX entry with the SAME ID.
- Never push, deploy, merge to main or write to a production DB unless the
  INBOX entry contains the literal text "GO confirmed by user".
- If something is unclear, write the question in OUTBOX and stop.

## One-time prompt to paste into the Claude Code tab

(Replace <TabName>. Uses Claude Code's /loop command — check `/help` in your
version that /loop exists and how it takes an interval.)

```
/loop 10m Check ~/Developer/.relay/<TabName>/INBOX.md for entries you haven't handled yet. For each new entry, in order: do the work it asks, then append a matching entry with the same ID to ~/Developer/.relay/<TabName>/OUTBOX.md saying what you did, the result, any commit hash, and any question. Never push, deploy, merge to main or write to production unless the entry contains "GO confirmed by user". If there's nothing new, do nothing.
```
