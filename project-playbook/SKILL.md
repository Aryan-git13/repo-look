---
name: project-playbook
description: Generates a comprehensive "Project Playbook" for any codebase, covering architecture, tech stack, references, AI usage, and learning path. Use when a user wants a detailed map of a project, onboarding guide, or AI-transparency documentation.
version: 1.0.0
---

# Project Playbook Generator

## Purpose
When invoked, this skill analyzes a project (a repository or directory) and produces a **structured Markdown playbook** that covers everything a developer, learner, or contributor needs to understand the project— including how it works, what technologies are used, where inspiration/references came from, and which AI agents/models were involved in its creation.

## When to use
- User asks to "understand this repo", "create a playbook", "document this project", "explain how this project is built".
- The user provides a project path (local folder, git repo, or from current directory).
- The user wants to document an AI‑assisted project (e.g., built with Cursor, Claude, Copilot).

## Inputs (optional flags)
The skill accepts the following parameters (passed as environment variables or as part of the prompt):
- `path` – (default: current directory) – path to the project root.
- `mode` – `quick` (1‑page summary), `deep` (full playbook with learning path), `ai-log` (focus on AI usage), `onboarding` (focus on setup/mental model).
- `output` – (default: stdout) – file path to save the playbook.
- `save` – boolean – if true, write playbook to `PLAYBOOK.md` or `docs/PLAYBOOK.md`.
- `force-ask` – if true, always prompt for missing info instead of guessing.

## Process
Follow these steps exactly:

1. **Scan the project** – using filesystem access, identify the structure, key configuration files (README, package.json, pyproject.toml, go.mod, Cargo.toml, requirements.txt, Dockerfile, docker-compose.yml, .env.example), source code directories, documentation files, and any AI-related configuration (`.cursor/`, `.claude/`, `.copilot/`, `.windsurf/`, `AGENTS.md`, `.cursorrules`, etc.).

2. **Detect tech stack** – parse configuration files to determine languages, frameworks, libraries, package managers, and runtime versions. Infer the overall architecture (monolith, microservices, client‑server, etc.) from file structure and dependencies.

3. **Identify entry points** – find `main` files, `index` files, or build scripts that start the application. Document them clearly.

4. **Extract references** – look for external links (URLs) in README, docs, comments, or configuration (e.g., documentation sites, blog posts, research papers) that might have inspired the project. Also look for mentions of "based on", "inspired by", "adapted from".

5. **Detect AI usage** – search for traces of AI tools and models:
   - Tools: Cursor, Claude Code, GitHub Copilot, ChatGPT, Gemini, Aider, Windsurf, Continue.dev, etc.
   - Models: GPT‑4, Claude 3/4, Gemini, Llama, etc.
   - Look for files like `.cursorrules`, `.claude/settings.json`, `.copilot-instructions.md`, `.aider.models`, or anywhere in README that says "built with", "powered by", "using".
   - If nothing is found, ask the user for this information; do not guess.

6. **Analyze code structure** – list main directories and important files; explain the role of each major module (e.g., `src/`, `lib/`, `tests/`, `scripts/`).

7. **Gather setup/run instructions** – extract from README or configuration (dependencies to install, environment variables, Docker commands, scripts). If missing, note that they are not documented.

8. **Compile all information** into a structured Markdown playbook using the template provided in `templates/playbook_template.md`. Fill every section with accurate, specific details. For any unknown items, write `⚠️ Not detected – please add manually`.

9. **Output** the playbook to stdout or save to the requested location (if `save` is true or `output` is given).

## Output Structure
The generated playbook must contain the following sections (in this order):

1. **Title** – project name and short tagline.
2. **Overview** – what the project does, its purpose.
3. **Problem it solves** – the need it addresses.
4. **How it works** – high‑level architecture, data flow, major components.
5. **Tech Stack** – languages, frameworks, libraries, databases, services.
6. **Technologies & Integrations** – external APIs, CI/CD, hosting, monitoring.
7. **Repository Structure** – annotated directory tree.
8. **Core Workflows / User Journeys** – how users interact with the system.
9. **Setup & Installation** – step‑by‑step local setup.
10. **Configuration** – environment variables, config files.
11. **Scripts & Commands** – common CLI commands (build, test, run).
12. **References & Inspiration** – links, papers, prior works.
13. **AI Agents, Tools & Models Used** – what AI was used, which models, and for what tasks.
14. **Prompts, Rules & Agent Workflows** – any custom instructions, `.cursorrules`, or prompt files.
15. **Key Decisions & Tradeoffs** – why certain choices were made.
16. **Limitations & Known Issues** – what doesn’t work well.
17. **Learning Guide** – a suggested reading/learning path for newcomers.
18. **Contribution / Extension Guide** – how to add features or fix bugs.
19. **Glossary** – domain‑specific terms.
20. **Changelog / Milestones** (optional if git history is available).

## Quality Standards
- **Accuracy**: never invent details. If something is not found, state it clearly.
- **Specificity**: use actual file names, dependency names, and code snippets when helpful.
- **Clarity**: write in plain, approachable language; assume the reader is new to the project.
- **Completeness**: ensure every section is addressed, even if only a placeholder note is added.

## Example Usage
- **Claude Code**: