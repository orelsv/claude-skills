# Claude Skills

Мої кастомні **skills** і **slash-команди** для [Claude Code](https://claude.com/claude-code). Кожен skill — окрема папка, можна встановити тільки те, що потрібно.

## Що всередині

| Skill | Для чого |
|---|---|
| [app-architecture](skills/app-architecture/SKILL.md) | Дизайн архітектури додатків: tech stack, структура папок, діаграми, ADR |
| [iac-azure-security](skills/iac-azure-security/SKILL.md) | Secure Terraform для Azure: NSG, Key Vault, RBAC, managed identity + сканування (tfsec, Checkov, Trivy) |
| [lab-writeup](skills/lab-writeup/SKILL.md) | Документація завершеної лаби/проекту: Obsidian-нотатка, LinkedIn-пост, summary |
| [security-review](skills/security-review/SKILL.md) | Security-аудит коду і конфігів: OWASP, threat modeling, auth/secrets review |
| [ui-ux-animation](skills/ui-ux-animation/SKILL.md) | UI/UX з анімаціями: scroll effects, parallax, hover, micro-interactions |

| Команда | Для чого |
|---|---|
| [/save](commands/save.md) | Зберегти контекст сесії — snapshot у Obsidian vault, daily log, git-стан |
| [/start](commands/start.md) | Відновити контекст у новій сесії — завантажує останній snapshot |

## Встановлення skill-а (один рядок)

Скопіюй команду потрібного skill-а — встановиться глобально для всіх проектів (`~/.claude/skills/`):

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

Як це працює:

```bash
mkdir -p ~/.claude/skills \                      # створити папку skills, якщо нема
&& curl -sL https://github.com/orelsv/claude-skills/archive/main.tar.gz \  # скачати repo як tar.gz
| tar -xz --strip-components=2 \                 # розпакувати, відкинувши перші 2 рівні шляху
  -C ~/.claude/skills \                          # ціль — глобальна папка skills
  claude-skills-main/skills/<назва-skill>        # витягти тільки потрібний skill
```

> Тільки для одного проекту замість усіх: заміни `~/.claude/skills` на `.claude/skills` у корені проекту.

## Встановлення /save + /start (session workflow)

Цикл `/save` → `/clear` → `/start`: зберігає стан сесії у Obsidian vault і відновлює його у новій сесії (контекст Claude Code обмежений, а snapshot переживає `/clear`).

```bash
# створити папку команд, якщо нема
mkdir -p ~/.claude/commands
# скачати обидві команди
curl -sL https://raw.githubusercontent.com/orelsv/claude-skills/main/commands/save.md -o ~/.claude/commands/save.md
curl -sL https://raw.githubusercontent.com/orelsv/claude-skills/main/commands/start.md -o ~/.claude/commands/start.md
```

⚠️ **Після встановлення обовʼязково**: у обох файлах шлях до vault прописаний як `/Users/orel/vault_claude/` — заміни на свій:

```bash
# заміни ШЛЯХ_ДО_ТВОГО_VAULT на свій шлях (наприклад ~/my-vault)
sed -i '' 's|/Users/orel/vault_claude|ШЛЯХ_ДО_ТВОГО_VAULT|g' ~/.claude/commands/save.md ~/.claude/commands/start.md
```

Команди очікують у vault структуру `logs/`, `logs/sessions/`, `wiki/index.md` — створи її або адаптуй файли під свою.

## Встановити все одразу

```bash
# клонувати repo у тимчасову папку
git clone --depth 1 https://github.com/orelsv/claude-skills.git /tmp/claude-skills
# скопіювати всі skills глобально
mkdir -p ~/.claude/skills && cp -r /tmp/claude-skills/skills/* ~/.claude/skills/
# скопіювати команди /save і /start
mkdir -p ~/.claude/commands && cp /tmp/claude-skills/commands/*.md ~/.claude/commands/
# прибрати тимчасову папку
rm -rf /tmp/claude-skills
```

## Skill vs команда — у чому різниця

- **Skill** (`skills/<name>/SKILL.md`) — Claude підхоплює сам, коли запит збігається з `description`; вручну: `/<name>`.
- **Slash-команда** (`commands/<name>.md`) — запускається тільки вручну: `/save`, `/start`. Підтримує `$ARGUMENTS`.

Після встановлення перезапусти сесію Claude Code (або відкрий нову), щоб skill зʼявився у списку.
