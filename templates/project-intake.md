# Template: Project Intake

> **Project-Agnostic Intake:** Do not assume Web, Docker, database, frontend, or migrations. Fill this template from the actual project. `docs/`, `spec/`, `plan/`, and `workflow/` are reference input categories; if the repository uses ADR/RFC/tickets/manifests or equivalent sources, use the sources with actual authority.

## Project Identity

- Project type / topology: `<web | microservices | mobile | distributed | CLI | other>`
- Primary languages/toolchains: `<...>`
- Repository shape: `<monorepo | multi-repo | single service | workspace | other>`

```text
Project name:
Repository/root:
Requested milestone:
Primary human authority:
Date/baseline SHA:
```

## Evidence Inventory

| Area | Evidence path | Fact | Authority/confidence |
|---|---|---|---|
| Product | | | |
| Architecture | | | |
| API/contracts | | | |
| Database | | | |
| Build/test | | | |
| CI/CD | | | |
| Security | | | |

## Repository Shape

```text
Languages:
Major modules:
Entry points:
Generated areas:
Persistence/schema/migration location (if applicable):
Client/UI/mobile/consumer locations (if applicable):
Infra/deploy locations:
```

## Toolchain Discovery

```text
Setup command:
Build command:
Unit test command(s):
Lint/typecheck:
Global quality gate:
Contract/schema validation:
E2E/smoke:
CI provider/workflows:
```

## Shared Resources / Hotspots

```text
Allocated sequential identifiers (migration/schema/etc., if applicable):
Ports (if applicable):
DB/storage objects (if applicable):
Routes/events/queues/commands/protocols (as applicable):
Env var namespaces:
Integration-only files:
```

## Facts, Inferences, Unknowns

```text
FACT:
INFERENCE:
UNKNOWN:
CONTRADICTION:
```

## Intake Exit Gate

Intake is complete only when the Lead has enough evidence to create the Project Execution Profile or has explicitly recorded which protected unknowns block planning.

## Input Source Map

| Information category | Actual source in this project | Authority/status |
|---|---|---|
| Architecture | `<docs/..., ADRs, RFCs, diagrams, code>` | `<authoritative/advisory/unknown>` |
| Specifications | `<spec/..., API schema, tickets, acceptance docs>` | `<...>` |
| Plan / sequencing | `<plan/..., issue tracker, roadmap>` | `<...>` |
| Workflow / engineering process | `<workflow/..., CI, CONTRIBUTING, Makefile/Taskfile/scripts>` | `<...>` |
| Build/test commands | `<manifests/scripts/CI>` | `<...>` |
| Environment/runtime constraints | `<env docs/config/deploy>` | `<...>` |
