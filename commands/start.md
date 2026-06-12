---
description: Start a session — load the latest snapshot, vault index, and git state
---

# /start — restore session context

Load the saved context (created by the `/save` command) and explain to the user where we left off.

Argument (optional — project name, part of a snapshot filename, or a date): $ARGUMENTS

## 1. Vault index

Read `/Users/orel/vault_claude/wiki/index.md` — the overall knowledge and project context.

## 2. Find the right snapshot

List snapshots (newest first):
```bash
ls -t /Users/orel/vault_claude/logs/sessions/
```

Selection — strictly bound to the current folder:
- if an argument is given — find the snapshot matching it (by project slug, date, or time)
- otherwise — the newest snapshot for EXACTLY the current project (basename of cwd in the filename)
- if there are NO snapshots for the current folder — do NOT auto-load another project; show the list of available snapshots and ask the user which one to take (or start fresh)

If the folder has several snapshots (parallel sessions) — take the newest, but mention that older ones exist and a specific one can be chosen via `/start <date_time>`.

## 3. Read the context

- the found snapshot in full
- the latest daily log (`logs/YYYY-MM-DD.md`, newest)
- if the snapshot mentions specific project files or wiki notes — do NOT read them all upfront; read them only when the work actually touches them (preserve context)

## 4. Git state

If cwd is a git repository: `git status` + `git log --oneline -5`. Compare with what the snapshot recorded (branch/commit) — if the state has diverged, note it.

## 5. Summary for the user

Brief and structured:
- **Where we left off** — 2-3 sentences from the snapshot
- **Next steps** — the `- [ ]` list from the snapshot (plus git divergences, if any)
- **Open questions** — if there were any

Finish by proposing which step to start with. Do NOT start executing steps without the user's confirmation.
