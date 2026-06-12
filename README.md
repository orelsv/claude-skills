# Claude Skills

Custom **skills** and **slash commands** for [Claude Code](https://claude.com/claude-code). Each skill lives in its own folder — install only what you need.

## What's inside

| Skill | What it does |
|---|---|
| [app-architecture](skills/app-architecture/SKILL.md) | Application architecture design: tech stack, folder structure, diagrams, ADRs |
| [iac-azure-security](skills/iac-azure-security/SKILL.md) | Secure Terraform for Azure: NSG, Key Vault, RBAC, managed identity + scanning (tfsec, Checkov, Trivy) |
| [lab-writeup](skills/lab-writeup/SKILL.md) | Document a finished lab/project: Obsidian note, LinkedIn post, summary |
| [security-review](skills/security-review/SKILL.md) | Security audit of code and configs: OWASP, threat modeling, auth/secrets review |
| [ui-ux-animation](skills/ui-ux-animation/SKILL.md) | UI/UX with animations: scroll effects, parallax, hover, micro-interactions |

| Command | What it does |
|---|---|
| [/save](commands/save.md) | Save session context — snapshot to an Obsidian vault, daily log, git state |
| [/start](commands/start.md) | Restore context in a new session — loads the latest snapshot |

## Install a skill (one line)

Copy the command for the skill you want — it installs globally for all projects (`~/.claude/skills/`):

```bash
# app-architecture
mkdir -p ~/.claude/skills && curl -sL https://github.com/orelsv/claude-skills/archive/main.tar.gz | tar -xz --strip-components=2 -C ~/.claude/skills claude-skills-main/skills/app-architecture
```

```bash
# iac-azure-security
mkdir -p ~/.claude/skills && curl -sL https://github.com/orelsv/claude-skills/archive/main.tar.gz | tar -xz --strip-components=2 -C ~/.claude/skills claude-skills-main/skills/iac-azure-security
```

```bash
# lab-writeup
mkdir -p ~/.claude/skills && curl -sL https://github.com/orelsv/claude-skills/archive/main.tar.gz | tar -xz --strip-components=2 -C ~/.claude/skills claude-skills-main/skills/lab-writeup
```

```bash
# security-review
mkdir -p ~/.claude/skills && curl -sL https://github.com/orelsv/claude-skills/archive/main.tar.gz | tar -xz --strip-components=2 -C ~/.claude/skills claude-skills-main/skills/security-review
```

```bash
# ui-ux-animation
mkdir -p ~/.claude/skills && curl -sL https://github.com/orelsv/claude-skills/archive/main.tar.gz | tar -xz --strip-components=2 -C ~/.claude/skills claude-skills-main/skills/ui-ux-animation
```

How it works:

```bash
mkdir -p ~/.claude/skills \                      # create the skills folder if missing
&& curl -sL https://github.com/orelsv/claude-skills/archive/main.tar.gz \  # download the repo as tar.gz
| tar -xz --strip-components=2 \                 # extract, dropping the first 2 path levels
  -C ~/.claude/skills \                          # target — the global skills folder
  claude-skills-main/skills/<skill-name>         # extract only the skill you need
```

> To install for a single project instead of globally: replace `~/.claude/skills` with `.claude/skills` in the project root.

## Install /save + /start (session workflow)

The `/save` → `/clear` → `/start` cycle: saves the session state into an Obsidian vault and restores it in a new session (the Claude Code context is limited, but a snapshot survives `/clear`).

```bash
# create the commands folder if missing
mkdir -p ~/.claude/commands
# download both commands
curl -sL https://raw.githubusercontent.com/orelsv/claude-skills/main/commands/save.md -o ~/.claude/commands/save.md
curl -sL https://raw.githubusercontent.com/orelsv/claude-skills/main/commands/start.md -o ~/.claude/commands/start.md
```

⚠️ **Required after install**: both files reference the vault path `/Users/orel/vault_claude/` — replace it with yours:

```bash
# replace YOUR_VAULT_PATH with your own path (e.g. ~/my-vault)
sed -i '' 's|/Users/orel/vault_claude|YOUR_VAULT_PATH|g' ~/.claude/commands/save.md ~/.claude/commands/start.md
```

The commands expect the vault to contain `logs/`, `logs/sessions/`, and `wiki/index.md` — create that structure or adapt the files to your own.

## Install everything at once

```bash
# clone the repo into a temp folder
git clone --depth 1 https://github.com/orelsv/claude-skills.git /tmp/claude-skills
# copy all skills globally
mkdir -p ~/.claude/skills && cp -r /tmp/claude-skills/skills/* ~/.claude/skills/
# copy the /save and /start commands
mkdir -p ~/.claude/commands && cp /tmp/claude-skills/commands/*.md ~/.claude/commands/
# remove the temp folder
rm -rf /tmp/claude-skills
```

## Skill vs command — what's the difference

- **Skill** (`skills/<name>/SKILL.md`) — Claude picks it up automatically when the request matches its `description`; manually: `/<name>`.
- **Slash command** (`commands/<name>.md`) — runs only manually: `/save`, `/start`. Supports `$ARGUMENTS`.

After installing, restart your Claude Code session (or open a new one) so the skill shows up in the list.
