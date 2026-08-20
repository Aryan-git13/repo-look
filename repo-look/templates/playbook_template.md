# [Project name] — Project Playbook

> **Purpose:** [One-sentence description.]  
> **Analysis scope:** [Repository path, branch/commit if known, and mode.]  
> **Confidence:** [What was confirmed, inferred, or unavailable.]

## 1. Overview

- **What it does:** [Confirmed description.]
- **Problem it solves:** [User or business need.]
- **Primary users:** [Who uses it.]
- **System shape:** [Monolith, client/server, library, service, etc.; evidence.]

## 2. Architecture and Data Flow

[Describe the major components and their responsibilities. Include a compact diagram only when it makes the relationships clearer.]

### Representative flow

1. [User action, request, event, or CLI command.]
2. [Frontend/client or entry point and file path.]
3. [API/handler/service/domain logic and file paths.]
4. [Database, queue, storage, or external service.]
5. [Result returned or emitted.]

## 3. Technology Stack

| Area | Technology | Purpose | Evidence |
| --- | --- | --- | --- |
| Runtime | [ ] | [ ] | [file] |
| Frontend | [ ] | [ ] | [file] |
| Backend | [ ] | [ ] | [file] |
| Data | [ ] | [ ] | [file] |
| Operations | [ ] | [ ] | [file] |

## 4. Repository Map

```text
[annotated, depth-limited tree]
```

| Path | Role | Start here when… |
| --- | --- | --- |
| [ ] | [ ] | [ ] |

## 5. Frontend

[If applicable: routes, components, state/data fetching, styling, forms, and client-side auth. Otherwise state why this section does not apply.]

## 6. Backend and APIs

[If applicable: entry points, endpoint/RPC groups, middleware, validation, services, domain logic, error handling, auth, and external integrations.]

## 7. Data and Async Processing

[Database/ORM, schemas, migrations, cache, storage, queues, workers, scheduled jobs, ownership and lifecycle.]

## 8. Configuration and Operations

- **Required configuration:** [Documented variable names and config files only—never values.]
- **Local services:** [Databases, containers, emulators, etc.]
- **CI/CD and deployment:** [Workflows and manifests.]
- **Observability:** [Logs, metrics, error reporting; or Not found.]

## 9. Setup, Run, and Verify

### Prerequisites

[Versions and tools, with evidence.]

### Commands

```sh
# Documented commands; label as unverified unless executed during this analysis.
[command]
```

### Verification

[How to tell the app, tests, and checks are working.]

## 10. Quality and Testing

[Test locations/types, lint/format/typecheck commands, fixtures, and gaps.]

## 11. Design Decisions, Limitations, and Risks

| Item | Status | Evidence / impact |
| --- | --- | --- |
| [Decision, tradeoff, limitation, or risk] | Confirmed / Inferred / Not found | [ ] |

## 12. References and Provenance

- **References/inspiration:** [Links or `Not found in repository evidence`.]
- **AI agents, tools, and models:** [Only verified evidence; otherwise `Not found in repository evidence`.]
- **Rules and prompts:** [Relevant instruction files and their role.]

## 13. Learning Path

### First hour

1. [Read/run/trace steps.]

### First day

1. [Deeper architecture and testing steps.]

### First contribution

1. [Small, safe change and verification path.]

## 14. Extension Guide

[Where to make common changes, conventions to preserve, and tests to add/run.]

## 15. Glossary

| Term | Meaning in this project |
| --- | --- |
| [ ] | [ ] |

## Appendix: Evidence and Unknowns

- **Confirmed:** [Key claims and source files.]
- **Inferred:** [Claim + rationale.]
- **Not found / needs owner input:** [Specific missing information.]
