## Installation

### Quick install

Install `project-playbook` globally for both Claude Code and Antigravity:

```bash
npx skills add Aryan-git13/project-playbook \
  --skill project-playbook \
  -g \
  -a claude-code \
  -a antigravity
```

### Install for Claude Code only

```bash
npx skills add Aryan-git13/project-playbook \
  --skill project-playbook \
  -g \
  -a claude-code
```

### Install for Antigravity only

```bash
npx skills add Aryan-git13/project-playbook \
  --skill project-playbook \
  -g \
  -a antigravity
```

### Install into the current project

Remove `-g` to install the skill for the current project only:

```bash
npx skills add Aryan-git13/project-playbook \
  --skill project-playbook \
  -a claude-code
```

### CLI flags

| Flag             | Meaning                 |
| ---------------- | ----------------------- |
| `-g`             | Install globally        |
| `-a claude-code` | Target Claude Code      |
| `-a antigravity` | Target Antigravity      |
| `--skill <name>` | Select a specific skill |

---

## Verify Installation

Restart your agent after installation if required.

Then check the available skills:

```text
/skills
```

You should see:

```text
project-playbook
```

You can also test the skill directly:

```text
Create a playbook for this project
```

---

## Usage

Use natural language:

```text
Create a playbook for this project
```

```text
Prepare me for an interview about this repo
```

```text
Explain what each part of this codebase does
```

```text
Generate frontend and backend playbooks
```

Or invoke it explicitly:

```text
/project-playbook --mode interview --split both --save
/project-playbook --split backend --depth file
/project-playbook --path ./apps/web --mode quick
```

---

## Output

Project Playbook can generate:

| File                   | Contents                                                                              |
| ---------------------- | ------------------------------------------------------------------------------------- |
| `PLAYBOOK.md`          | Master index and recommended reading order                                            |
| `PLAYBOOK-FRONTEND.md` | Frontend architecture, stack, responsibility map, and study blocks                    |
| `PLAYBOOK-BACKEND.md`  | Backend architecture, data model, responsibility map, and study blocks                |
| `PLAYBOOK-CONTRACT.md` | Frontend ↔ backend API surface, authentication flow, data flow, and full-stack traces |

Example:

```text
project/
├── PLAYBOOK.md
├── PLAYBOOK-FRONTEND.md
├── PLAYBOOK-BACKEND.md
└── PLAYBOOK-CONTRACT.md
```

---

## Requirements

For the installation command to work:

1. The GitHub repository must be public.
2. `SKILL.md` must be discoverable in the repository.
3. The `SKILL.md` frontmatter must contain:

```yaml
---
name: project-playbook
description: Turn any codebase into an interview-ready frontend, backend, and full-stack playbook.
---
```

The `name` must match:

```text
--skill project-playbook
```

The repository is:

```text
https://github.com/Aryan-git13/project-playbook
```

---

## Update

```bash
npx skills update project-playbook
```

## Remove

```bash
npx skills remove project-playbook
```

---

## Manual Installation

### Claude Code

```bash
git clone https://github.com/Aryan-git13/project-playbook.git
mkdir -p ~/.claude/skills
cp -r project-playbook ~/.claude/skills/project-playbook
```

### Antigravity

```bash
git clone https://github.com/Aryan-git13/project-playbook.git
cp -r project-playbook ~/.antigravity/skills/project-playbook
```
