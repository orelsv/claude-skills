---
description: Save session context — snapshot to vault, daily log, wiki, git check
---

# /save — save the session before closing

Save the current session state so the context can be restored with `/start` after `/clear` (or in a new session). Complete ALL steps in order.

Internal links — wikilinks `[[...]]`, NOT markdown links. Vault conventions (if the file exists): `/Users/orel/vault_claude/CLAUDE.md`.

Extra note from the user (may be empty): $ARGUMENTS

## 1. Session snapshot → vault

Create the file `/Users/orel/vault_claude/logs/sessions/YYYY-MM-DD_HHMM_<project-slug>.md`:
- date/time — current (`date +%Y-%m-%d_%H%M`)
- `<project-slug>` — current project folder name (basename of cwd)

Template:

```markdown
---
tags: [session-snapshot]
date: YYYY-MM-DD
project: <project-slug>
cwd: <full path to the project>
---

# Session snapshot — <project> — YYYY-MM-DD HH:MM

## Context
- What we worked on (2-3 sentences, clear to someone who remembers nothing)
- Git: branch + latest commit (if this is a git repository)

## What we did
- (concrete results of this session, not the process)

## Files (created / modified)
- `path/to/file` — what exactly and why

## Decisions and why
- (important decisions with reasoning — so they don't get re-litigated)

## Pitfalls / Lessons
- (what went wrong and how it was solved; if nothing — drop the section)

## Open questions
- (what remains unclear / unresolved)

## Next steps
- [ ] (concrete, actionable items — the next session starts from these)

## How to restore the environment
```bash
# commands to start the project/environment, if needed
```

## Related topics
- [[../../wiki/...|...]]
```

Fill it in honestly based on the facts of the session. Delete empty sections, don't leave placeholders.

## 2. Daily log

Update `/Users/orel/vault_claude/logs/YYYY-MM-DD.md` (if missing — create from `logs/TEMPLATE.md` if it exists): add what was learned, commands worth remembering, questions, "Next time" items. Don't duplicate the whole snapshot — only the learning digest.

## 3. Wiki (only if there was significant work)

If the session included a new concept, a new project, or substantial progress on an existing one:
- create/update a note in the matching `wiki/<category>/` or `wiki/projects/`
- add backlinks `[[...]]`
- update `wiki/index.md` (including the "Last updated" date)

If it was a minor/technical session — skip this step and say why.

## 4. Git

If cwd is a git repository: run `git status` and report uncommitted changes. Do NOT commit without the user's explicit permission. Remind about `.gitignore` if any changed files look like secrets (.env, *.key, credentials).

## 5. Security check

Before writing, verify that NO credentials, API keys, passwords, or tokens end up in the snapshot or logs. If anything like that is in context — replace it with `<REDACTED>`.

## 6. Final message

Show the user:
- paths to all created/updated files
- uncommitted git changes (if any)
- a reminder: **to free the context — run `/clear` (or close the session), then `/start` in the new session**
