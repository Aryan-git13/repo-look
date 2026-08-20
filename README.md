# Project Playbook

`project-playbook` is an agent skill that turns an unfamiliar repository into an evidence-based guide for studying, running, and extending it. It covers frontend, backend, data, operations, tests, and any verifiable AI-development provenance.

## Install

Copy the `project-playbook` directory into the skills directory used by your terminal agent, preserving its contents:

```text
project-playbook/
├── SKILL.md
└── templates/playbook_template.md
```

For Claude Code, a project-local installation is commonly `.claude/skills/project-playbook/`. For Antigravity or another agent, use that tool's configured skill directory. The skill is deliberately self-contained and does not require scripts, a network connection, or another agent.

## Use

Ask your agent something like:

```text
Use the project-playbook skill to deeply analyze this repository and save the result as docs/PLAYBOOK.md.
```

Modes can be requested in natural language:

- `quick`: orientation for a new codebase.
- `deep`: complete architecture and learning guide.
- `onboarding`: setup and first-contribution guide.
- `ai-log`: evidence-based AI tools, models, prompts, and rules inventory.

The playbook reports claims as Confirmed, Inferred, or Not found, and must not disclose secret values from environment files or credentials.
