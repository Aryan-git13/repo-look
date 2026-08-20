---
name: project-playbook
description: Generates comprehensive Project Playbooks for any codebase — automatically split into a Frontend Playbook and a Backend Playbook plus a shared Contract document — covering architecture, tech stack with rejected alternatives, a per-module "what each part does" responsibility map, data/state models, references, AI tools and models used, and full interview preparation via 12 study blocks. Use when a user wants to understand a project deeply, study or revise their own project, prepare for an interview about a repo, document AI-assisted development, create onboarding or handover docs, explain a codebase, audit a tech stack, or asks what each file, folder, or module is doing.
version: 3.0.0
license: MIT
---

# Project Playbook Generator

## Purpose
Analyzes a project directory or repository and produces **structured, interview-ready Playbooks** —
split by layer so each document is coherent, memorizable, and rehearsable.

The output helps a user:
- Understand the project completely (what, how, why) — **layer by layer**
- Know **what every part is doing** and why it exists
- Learn the technical decisions and trade-offs on both sides of the stack
- Prepare for interviews via the 12 study blocks, tailored per layer
- Document AI usage, references, and full development context

## When to use
- "study this project", "prepare me for an interview about this repo", "create a playbook",
  "document this project", "explain this codebase", "what does each file do",
  "explain the frontend", "explain the backend", "revise my project"
- The user provides a project path (local folder, git repo, or current directory)
- The user is preparing for an interview on their own or a public project

---

## Prime directive — never invent
Every technology, version, path, model ID, and metric must be traceable to a real artifact in
the project or to an explicit user statement.

- Unfound but expected → `⚠️ Not detected — please add manually`
- Only the user can know it (metrics, team split, motivations) → `[YOU MUST FILL THIS IN]`

**Never fabricate a number.** A candidate repeating an invented latency figure in a real
interview is worse off than one who says "roughly, I'd estimate." Placeholders are a feature.

Track findings internally as `claim | evidence (path:line) | confidence (High/Med/Low)`.
Mark Low-confidence items with `⚠️`. Never silently upgrade confidence.

---

## Inputs (optional flags)

| Flag | Default | Values / meaning |
|---|---|---|
| `path` | `.` | Project root |
| `split` | `auto` | `auto` · `both` · `frontend` · `backend` · `single` · `per-service` |
| `mode` | `deep` | `deep` (all sections) · `interview` (12 blocks emphasized) · `quick` (1-page) · `ai-log` (AI usage only) |
| `depth` | `module` | `file` (every significant file) · `module` (folder-level) · `system` (top-level only) |
| `output` | file | Directory for generated docs |
| `save` | `true` | Write to `docs/` or repo root |
| `force-ask` | `false` | Always prompt instead of deducing |

---

## Step 1 — Split detection (run before anything else)

Classify the codebase, then decide how many playbooks to produce.

### Classification signals

| Layer | Strong signals |
|---|---|
| **Frontend** | `src/components`, `src/pages`, `app/`, `public/`, `.jsx/.tsx/.vue/.svelte`, `index.html`, CSS/Tailwind/styled-components, `vite.config`, `next.config`, `angular.json`, `App.tsx`, router configs, state stores (Redux/Zustand/Pinia), `package.json` deps: react, vue, svelte, angular, next |
| **Backend** | `routes/`, `controllers/`, `services/`, `api/`, `models/`, `handlers/`, `migrations/`, `main.py`, `server.js`, `app.py`, `cmd/`, ORM schemas, `Dockerfile` exposing a port, deps: express, fastify, nestjs, django, flask, fastapi, gin, spring, rails |
| **Shared** | `types/`, `shared/`, `packages/common`, `proto/`, OpenAPI/Swagger specs, generated clients, `.env.example`, root CI, docker-compose, monorepo config (turbo, nx, pnpm-workspace, lerna) |
| **Infra/Ops** | `terraform/`, `k8s/`, `.github/workflows/`, `helm/`, IaC |

### Decision rules

| Situation | Output |
|---|---|
| Distinct FE and BE dirs (`client/`+`server/`, `frontend/`+`backend/`, monorepo packages) | **Both playbooks + Contract** |
| Fullstack framework, one codebase (Next.js App Router, Nuxt, SvelteKit, Remix, Django+templates, Rails, Laravel) | **Both playbooks**, split logically by *responsibility* not folder: server components / route handlers / server actions / API routes / ORM → Backend; client components / hooks / stores / styling → Frontend. State this split rule explicitly in both docs. |
| Frontend only (static site, SPA hitting a third-party API) | **Frontend Playbook** only; document the consumed API under External Services |
| Backend only (API, CLI, worker, library, data pipeline) | **Backend Playbook** only; note "no frontend in this repo" |
| Mobile app + API | Treat mobile as Frontend; note platform in the title |
| Microservices / multi-service monorepo | One Frontend Playbook + **one Backend Playbook per service** + one Contract. Never merge distinct services into a single stack table. |
| Genuinely inseparable (small script, notebook) | **Single Playbook**, note why it wasn't split |

Announce the decision in one line before proceeding:
> *Detected: React frontend (`/client`) + FastAPI backend (`/server`). Generating 3 documents.*

If ambiguous, ask **once** and offer the most likely answer as the default.

---

## Step 2 — Scan
Structure, key configs (README, package.json, pyproject.toml, go.mod, Cargo.toml,
requirements.txt, Dockerfile, docker-compose.yml, .env.example, CI files, lockfiles), source
directories, docs, migrations, tests, and AI config (`.cursor/`, `.claude/`, `.copilot/`,
`.windsurf/`, `AGENTS.md`, `CLAUDE.md`, `.cursorrules`, `.aider*`, `.mcp.json`).

## Step 3 — Detect tech stack
Parse manifests **and lockfiles**. Pull exact versions from lockfiles, never from range
specifiers (`^4.0.0` → record the range, mark Medium confidence). Determine languages,
frameworks, package managers, runtime versions. Infer architecture pattern.

## Step 4 — Identify entry points
Per layer: FE = root render/mount, router, layout, main HTML. BE = server bootstrap, route
registration, middleware chain, workers, cron, CLI commands.

## Step 5 — Map every part *(feeds the responsibility map — do not skip)*
For each significant directory and file at the chosen `depth`, record:
**what it is · what it does · what it depends on · what depends on it · why it exists.**

## Step 6 — Extract references
External URLs in README/docs/comments, "based on / inspired by / adapted from", vendored code,
LICENSE and NOTICE attributions, ADR citations. Only list what literally appears in the project.

## Step 7 — Detect AI usage
Search for AI tool traces (Cursor, Claude Code, Copilot, Windsurf, Aider, ChatGPT) and **exact
model IDs** (`claude-*`, `gpt-*`, `o[0-9]*`, `gemini-*`, `llama-*`, `mistral-*`, `deepseek-*`),
SDKs (anthropic, openai, langchain, langgraph, crewai, @ai-sdk/*, llamaindex, litellm, ollama),
prompt files, tool/function definitions, MCP servers, vector stores, and eval/tracing tooling.

**Model ID rule:** report exactly as written in code. If it comes from an env var with no
default → `from env: MODEL_NAME (value unknown)`. Never substitute a plausible model.
If none found, keep the section and note it — the user may have used AI without committing config.

## Step 8 — Setup, config, and commands
From README, package scripts, Makefile, Dockerfile, compose, CI.

## Step 9 — Gap interview
Ask **only** what the code cannot answer. Max 7 questions, **one message**, numbered, each with
a suggested default so the user can reply "defaults."

Priority order: solo vs team and **which parts you personally wrote** · real metrics (users,
latency, cost, data size) · hardest bug you debugged · a decision you'd reverse today · what you
deliberately cut · target role/level · anything in the repo you can't explain.

Unanswered → `[YOU MUST FILL THIS IN]`. Never fill them yourself.

## Step 10 — Generate documents

| Document | File | Contents |
|---|---|---|
| Frontend Playbook | `PLAYBOOK-FRONTEND.md` | Full section set, FE-specialized |
| Backend Playbook | `PLAYBOOK-BACKEND.md` | Full section set, BE-specialized (one per service if microservices) |
| Contract | `PLAYBOOK-CONTRACT.md` | The seam between them — see below |
| Index | `PLAYBOOK.md` | Short router: what each doc covers, split rule used, read order |

Save to `docs/` if it exists, else repo root.

**Anti-duplication rule:** shared concerns (auth flow, API surface, shared types, deployment,
env vars, AI tooling) live in the **Contract** document. Each playbook links to it rather than
repeating it. Each playbook must stand alone for *its* layer, but must not restate the other.

## Step 11 — Self-audit
Report: files inspected / total and coverage %, documents generated, sections completed,
count of `⚠️ Not detected` and `[YOU MUST FILL THIS IN]`, and the **top 3 things the human
must verify first**.

---

# Output structure — per playbook

Both `PLAYBOOK-FRONTEND.md` and `PLAYBOOK-BACKEND.md` use this exact section order.
Sections 1–12 are the 12 study blocks. Content is **layer-specific**, never copy-pasted.

**Title & Overview** — project name, layer, one-line description, split rule used.

### 1. The Pitch (3 versions)
Written as **speakable scripts**, not bullets, scoped to this layer.
- **30 sec (~75 words), non-technical:** problem → what it does → outcome. Zero jargon.
- **2 min (~300 words), technical:** problem → architecture → **your role** → result.
- **10 min deep dive:** headings + the diagram to draw, in drawing order.
> *Rehearse all three out loud. Highest-ROI item on this page.*

### 2. Problem & Context
Why this layer exists, its users (end users for FE, consuming clients for BE), what came before,
what "success" meant, why this over doing nothing.

### 3. Your Contribution — "I vs we"
Two-column table: **I personally built** / **Team, generated, or vendored**.
Flag anything that looks scaffolded, templated, or tutorial-derived — the user must not claim it.
> *Never say "we" for code you can't explain line by line.*

### 4. Architecture (drawable from memory)
- Simplified diagram, **drawable in 90 seconds**
- **Frontend:** component tree, routing, state management and where state lives, data fetching
  and caching, rendering strategy (CSR/SSR/SSG/ISR), hydration boundaries, bundle/code splitting
- **Backend:** request lifecycle, middleware chain, service/repository layering, DB access,
  background jobs and queues, caching layers, external integrations, sync vs async boundaries
- **Full trace:** one user action end-to-end through *this* layer, every hop named with file
  path. Mark clearly where it crosses into the other layer → link to Contract doc.

### 5. ⭐ What Each Part Does — Responsibility Map
**Mandatory. The core of this playbook.** Annotated tree, then a table per major directory:

| Path | Type | What it does | Key dependencies | Used by | Why it exists |
|---|---|---|---|---|---|
| `src/hooks/useAuth.ts` | Hook | Holds session state, refreshes token | `authApi`, Zustand store | All protected routes | Centralizes auth so components stay dumb |

Rules: cover 100% of significant directories; go file-level for entry points, core logic, and
anything unusual; group boilerplate ("22 UI primitives — shadcn/ui, unmodified"); explicitly
flag dead code, duplication, and god-files. This section directly answers the interviewer's
*"walk me through this file."*

### 6. Data / State Model
- **Frontend:** global vs local vs server state, store shape, cache and invalidation strategy,
  form state, optimistic updates, persistence (localStorage, cookies), prop-drilling hotspots
- **Backend:** schema, key relationships, **every index and why those columns**, normalization
  decisions, migration history, transactions and isolation, what you'd denormalize under load

### 7. Core Logic
The 2–3 genuinely non-trivial algorithms or flows in this layer. Time/space complexity, edge
cases handled, why not the naive approach. Select by complexity and centrality, not file size.

### 8. Numbers (memorize these)

| Frontend | Backend |
|---|---|
| Bundle size, LCP/FCP/TTI, Lighthouse, route count, component count, re-render hotspots | RPS, p50/p95/p99 latency, DB size, slowest query, connection pool, error rate, uptime, monthly cost |

Shared: build/deploy time, test coverage, LOC, dependency count.
Auto-fill only what's verifiable (CI timings, coverage reports, config limits, pricing files);
everything else `[YOU MUST FILL THIS IN]`.
Include one **impact statement**: *"cut X from A to B."*
> *A rough number beats "I'm not sure." Estimate — and say you're estimating.*

### 9. Failure, Security, Testing
Report **what exists vs. what's missing** — gaps are interview questions, and each missing item
gets a defensible one-line answer the user can give.
- **Frontend:** error boundaries, loading/empty/error states, XSS, CSP, token storage, client-side
  validation, a11y, offline behavior, unit/component/E2E coverage
- **Backend:** error handling and retries, dependency-failure behavior, authn vs authz, secrets
  handling, input validation and injection surface, PII, rate limiting, idempotency, CORS,
  unit/integration/load tests, CI/CD, rollback, monitoring and alerting

### 10. Bottlenecks & The Hard Bug
Where this layer slows down, how you'd find it (profiler? React DevTools? APM? slow query log?),
the fix, the **measured** result. Plus one **STAR story** — seeded from git history, revert
commits, long-lived branches, or `fix:` messages. If none found, ask the user explicitly:
this gets asked in nearly every interview.

### 11. Trade-offs & Retrospective
What you'd do differently, known tech debt **with file paths**, what you deliberately cut and why.
> *Owning limitations reads as senior. Pretending there are none reads as junior.*

### 12. Scale to 100x
What breaks **first**, and in what order.
- **Frontend:** CDN, code splitting, virtualization, image optimization, edge rendering, caching
- **Backend:** read replicas, connection pooling, queues, horizontal scaling, sharding, rate
  limits, cost ceiling

Ground every recommendation in *this project's* actual bottleneck, never generic advice.
> *This is where a project interview becomes a system design interview.*

### 13. Tech Stack — With Rejected Alternatives
The most-asked artifact in the interview. One row per **major** choice, cap at 10 per layer.

| Choice | Version | What it is | Why chosen | Rejected alternative | Trade-off accepted | Would change now? |
|---|---|---|---|---|---|---|

Fill Why/Rejected from README, ADRs, and commit messages where possible; otherwise
`[YOU MUST FILL THIS IN]` with a hint (*"check the commit that added this dep"*).

### 14. References & Inspiration
Docs, repos, papers, tutorials, prior art — categorized (Concept · Architecture · Code/Library ·
AI/Prompting · Infra · Design), each with the source path where it was found.

### 15. AI Agents, Tools & Models Used
Tools (Cursor, Claude Code, Copilot…), **exact model IDs**, what each helped with in this layer,
and what was hand-written. If this layer contains runtime AI, add the Agent Registry:

| Agent / call site | Role | Model ID (exact) | Provider | Tools / MCP | Prompt location | Params | Failure modes | Evidence | Conf. |
|---|---|---|---|---|---|---|---|---|---|

### 16. Prompts, Rules & Agent Workflows
Custom instructions, `.cursorrules`, `CLAUDE.md`, prompt files, agent configs, versioning approach.

### 17. Repository Structure (this layer)
Annotated tree. Cross-reference section 5 rather than repeating it.

### 18. Setup & Installation — this layer standalone
Prerequisites, install, env vars needed, run command, and **how to run without the other layer**
(mock server, seeded DB, MSW, stub client).

### 19. Configuration
Env vars table: name · purpose · required? · default · where consumed.
**Never print secret values** — variable names only.

### 20. Scripts & Commands
Build, dev, test, lint, migrate, deploy — with what each actually does.

### 21. Glossary
Domain terms, abbreviations, internal jargon found in the code.

### 22. Interview Appendices *(mode: `interview` or `deep`)*
- **A — Likely questions (20 per layer, ranked).** Generated from **this repo's real weak spots**
  — untested modules, hardcoded values, missing error handling, unexplained dependencies,
  no migrations, absent auth, single points of failure. Never generic. Each: question · why
  they'll ask · 3-sentence model answer.
- **B — AI-specific questions** *(only if runtime AI detected).* Model choice and benchmark,
  prompt strategy and versioning, RAG params (chunk size, embedding model, k, reranking) and
  *why those values*, **how you evaluated quality**, hallucination mitigation, agentic vs single
  call, loop termination, cost/latency per request, prompt injection, PII, fine-tune vs prompt.
  Flag the killer question: *"How do you know your system is actually good?"* — "it looked right"
  is a fail.
- **C — Weak spots.** Ranked list of everything that won't survive a "why?" chain, each with the
  likely question and either a learning task or an honest scripted fallback.
- **D — Study plan & walkthrough path.**

| Time | Do this |
|---|---|
| 1 hour | Section 1 pitch + section 4 diagram + section 13 table |
| 1 day | Above + section 8 numbers + section 10 story + re-read your 5 core files |
| 1 week | Above + full section 5 read, section 12, section 9 audit, mock drill |

Plus 4–5 files in presentation order, ending on the best-written one.

### 23. Unverified / Needs Human Input
Consolidated list of every `⚠️` and `[YOU MUST FILL THIS IN]`, with how to find each answer.
Do not pad this to look thorough — list only real gaps.

### 24. Metadata
Generated date, commit SHA, files inspected / total, coverage %, sampling strategy, split rule.

---

# Output structure — `PLAYBOOK-CONTRACT.md`

The seam between the layers. Every interviewer probes this, and it's where most candidates fail.

1. **System diagram** — both layers plus external services, one picture
2. **API surface** — every endpoint: method · path · auth · request shape · response shape ·
   error codes · which FE file calls it · which BE file handles it
3. **Shared types / schemas** — source of truth, generation pipeline, drift risk
4. **Auth flow** — full sequence: login → token issue → storage → attach → verify → refresh →
   logout → expiry handling
5. **Error contract** — error shape, status code conventions, how the FE renders each class
6. **Realtime / async** — WebSockets, SSE, polling, webhooks, queues, retry and backoff
7. **File uploads / large payloads** — direct-to-storage vs proxied, size limits
8. **The one full-stack trace** — a single user action across **both** layers, every hop named
   with file paths on both sides. *Rehearse this one until it's automatic — it's the most
   common whiteboard request.*
9. **Contract-breaking changes** — what breaks the other side, versioning strategy, deploy order
10. **Local dev** — running both together, ports, proxy config, CORS, seed data
11. **Deployment & environments** — where each layer is hosted, env matrix, CI/CD pipeline, rollback
12. **Shared env vars** — names only, never values
13. **Cross-layer interview questions** — 10, plus the classic *"a request is slow — how do you
    find out whether it's the frontend, the network, or the backend?"*

---

## Mode variations

| Mode | Behavior |
|---|---|
| `deep` *(default)* | All sections, all documents |
| `interview` | All sections, but sections 1–12 and 22 get maximum detail and rehearsable draft content; sections 17–21 compressed |
| `quick` | One page per layer: sections 1, 4, 5 (summarized), 8, 13, 15 + a 5-item Contract summary |
| `ai-log` | Sections 15, 16, plus AI-touched entries in section 5 only |

---

## Quality standards
- **Accuracy** — never invent. Not found → `⚠️ Not detected`. User-only → `[YOU MUST FILL THIS IN]`.
- **Specificity** — real file names, real dependency names, real versions, real paths, real numbers.
- **Separation** — each playbook stands alone for its layer; shared concerns live only in the Contract.
- **Coverage** — section 5 must account for every significant directory. No unexplained folders.
- **Clarity** — plain English first, technical detail second. Tables over prose. No filler.
- **Interview-readiness** — sections 1–12 must contain concrete, rehearsable draft content.

## Do not
- Do not guess versions, model names, URLs, or **metrics**
- Do not copy secrets, keys, tokens, or `.env` values — variable names only
- Do not modify project files other than writing the output documents
- Do not let the user claim code flagged as scaffolded or vendored
- Do not duplicate content across the two playbooks — link to the Contract instead
- Do not list every file in huge repos — group boilerplate, focus on core modules

## Large repositories
Over ~300 files: never read `node_modules`, `vendor`, `dist`, `build`, `.next`, `target`,
`.venv`, `__pycache__`, `coverage`, generated clients, snapshots, binaries, media. Read all
manifests, lockfiles, configs, and CI files in full — cheap and dense. Sample source: every
entry point, each top-level module's primary file, all AI matches, plus the 20 largest source
files. Prefer grep over full reads when hunting model IDs and SDK imports. State the sampling
strategy and coverage % in Metadata.

## Example usage
```
/project-playbook                                      # auto-split, deep mode
/project-playbook --mode interview --split both --save
/project-playbook --split backend --depth file
/project-playbook --path ./apps/web --mode quick
claude --skill project-playbook --path /repo --mode interview
```

## Dependencies
Read access to the target directory. Git optional (for history-seeded STAR stories and commit
mining). No runtime dependencies — the skill is instruction-driven.

## Additional note
**The skill's real value is in the preparation it forces.** After generating, prompt the user to
(1) rehearse the 30-second pitch out loud, (2) draw the architecture from memory three times,
and (3) run the mock drill on Appendix C weak spots. Offer the drill immediately.

---