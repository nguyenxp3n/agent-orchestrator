# AGENT-ORCHESTRATOR: Design Specification

**Date:** 2026-09-23  
**Status:** User-authorized automatic approval for all design/execution gates  
**Product type:** Markdown handbook + operational toolkit, not a software runtime  
**Audience:** AI Lead Architect / engineer coordinating 5â€“10+ coding agents on the same software project

## 1. Objective

Build a methodological framework usable with Claude, GPT, Gemini, Antigravity, Cursor, Windsurf, and other coding agents; applicable to Web Fullstack, Backend/Microservices, Mobile, Distributed Systems, and CLI projects. The framework must enable an AI Lead Architect to read a project, decompose Work Packages, build a DAG, assign ownership and resource locks, create isolated workspaces, orchestrate parallel work, handle Decision Requests, run independent audits, and perform sequential evidence-backed integration.

## 2. Positioning

AGENT-ORCHESTRATOR is not a Python orchestration engine, SDK, scheduler daemon, or executable control plane. The framework is a **field manual + operational toolkit** executed by an AI Lead Architect. Primitives such as DAG, state machine, locks, recovery, audit attestation, and integration queue remain protocols, templates, checklists, and playbooks; they are not implemented as a 400KB runtime.

## 3. Synthesis sources

1. V1 `agent-orchestrator-framework`: Zero Hallucination, Zero Trust, exhaustive checklist discipline, resource boundary enforcement, strict completion gate, Forensic Audit, and anonymized/generalized field incidents.
2. V2 `orchestrator-framework-v2.0.0`: Project Intelligence, Work Package protocol, DAG, Ownership Matrix, semantic resources, locks/leases, state machine, generation fencing, recovery bundle, 15 conformance scenarios, independent audit, dependency-aware integration, cross-WP gate.
3. `ai-project-finalization-workflow-v2.0.0`: subject-specific source of truth, state names as assurance levels, stop conditions, audit closure discipline.
4. Previous runtime: prompt roles for Orchestrator/Worker/Auditor/Integrator and the rule that `WORKER_COMPLETE` does not imply `ACCEPTED`.
5. Anonymized six-agent field experience: parallel branches, resource-slot allocation, scope rejection, missing required-layer detection, external CI verification, and sequential merge.

## 4. Four invariants

### 4.1 Zero Hallucination
Without evidence, the state is `UNKNOWN`. Do not invent files, command output, CI status, commits, branches, resource allocations, or completion.

### 4.2 Zero Trust
A Worker report is a claim. The Lead or Auditor must independently verify every mutable state before treating it as truth.

### 4.3 Mandatory Verification
Every important state transition requires verifiable evidence: filesystem/Git diff/exit code/test log/CI/API or another inspectable artifact.

### 4.4 Atomic Completion
A WP reaches `ACCEPTED` only when 100% of mandatory acceptance criteria pass. Backend passing while frontend is missing remains `INCOMPLETE`.

### 4.5 Project-Agnostic
The framework must not depend on a specific project/domain/case study. Project input (`docs/`, `spec/`, `plan/`, `workflow/` or equivalent sources) determines the project model, ownership, resources, commands, and verification. DB/migration/frontend/Docker concepts appear only when the actual project has them.

## 5. Adaptive Strictness

The framework is strict on outcomes/invariants and flexible on mechanisms. Isolation is mandatory, but may use a Git worktree, separate clone, container, or remote workspace. The quality gate is mandatory, but the command depends on the toolchain: `task qa`, `make test`, `pnpm test`, `cargo test`, `go test ./...`, `pytest`, `dotnet test`â€¦ Do not force Docker when the project does not use Docker.

## 6. Separation of Duties

- Lead Orchestrator: project intake, source-of-truth, decomposition, DAG, ownership, resource allocation, assignment, DR arbitration, recovery, acceptance, integration order.
- Worker: implementation only within the WP and permission envelope.
- Auditor/Reviewer: independently verifies; does not modify code in the same audit task.
- Integrator: integrates accepted candidates; modifies hotspots only through approved Integration Requests.

The Lead does not automatically write code on behalf of the Worker. If implementation must change, reassign the Worker, create a corrective WP, or replace the Worker. This preserves ownership, auditability, and separation of duties.

## 7. Canonical Lifecycle

`PROJECT INPUT â†’ INTAKE â†’ PROJECT MODEL â†’ WP DECOMPOSITION â†’ DAG â†’ OWNERSHIP/RESOURCE ALLOCATION â†’ ISOLATED WORKSPACES â†’ PARALLEL EXECUTION â†’ WORKER_DONE â†’ FORENSIC AUDIT â†’ ACCEPTED/REWORK â†’ INTEGRATION QUEUE â†’ SEQUENTIAL MERGE â†’ CROSS-WP/GLOBAL QA â†’ MAIN/RELEASE`.

The DAG determines integration order, not the time a worker reports completion.

## 8. State Model

Minimum WP states: `DRAFT`, `READY`, `ASSIGNED`, `RUNNING`, `BLOCKED`, `AWAITING_DECISION`, `READY_FOR_AUDIT`, `REWORK`, `ACCEPTED`, `INTEGRATING`, `INTEGRATED`, `FAILED`, `CANCELLED`.

Agent runtime state is a separate concept: `READY`, `RUNNING`, `SUSPECTED_STALLED`, `PAUSED`, `FAILED`, `REASSIGNED`, `ABANDONED`. A worker crash does not automatically fail the WP.

## 9. Resource Model

Manage both paths and semantic resources. Reference ownership modes: `EXCLUSIVE_WRITE`, `SHARED_READ`, `INTEGRATION_ONLY`, `ALLOCATED_WRITE`, `APPEND_ONLY`, `GENERATED`.

Semantic resources include: migration numbers, DB tables/columns, API routes, event names, queue names, env vars, ports, CLI commands, feature flags, schema versions, public interfaces.

The Lead allocates migration/port slots before dispatch; workers do not self-allocate.

## 10. Work Package Contract

Each WP includes at minimum: ID, objective, source requirements, dependencies, allowed/readonly/forbidden paths, owned resources, expected files/artifacts, frozen contracts, acceptance criteria, commands, stop conditions, DR procedure, completion report contract, integration requests.

A Worker does not self-expand scope. Any requirement outside the envelope becomes a Decision Request, Resource Request, Ownership Transfer, Integration Request, or new WP.

## 11. Source-of-Truth

Do not use one flat precedence ladder for every subject. Authority is subject-specific: product behavior, API wire shape, DB physical schema, security invariant, deployment, and implementation order may belong to different sources. If equal-authority sources conflict, preserve `UNRESOLVED`; do not merge them into an assumption.

## 12. Forensic Audit

Audit must cover at least seven layers: Git/identity, workspace hygiene, scope/diff, expected outputs, unit/target tests, repo-wide quality gate, acceptance rubric + security/contracts/regression. Missing evidence = `UNKNOWN` and blocks acceptance. Audit binds to the exact candidate SHA/snapshot; changing the candidate after audit invalidates the old audit.

## 13. Sequential Integration

Only an `ACCEPTED` candidate enters the integration queue. Merge by dependency DAG, with pre-merge checks and post-merge global checks at each step. Only the Integrator handles shared integration hotspots. Main drift or a stale audited SHA requires batch revalidation.

## 14. Recovery

When an agent crashes: freeze the workspace, capture status/diff/latest safe commit/leases/DRs/warnings, classify clean recoverable/dirty recoverable/corrupted/unsafe-unknown, create a Recovery Bundle, then increment assignment generation. Reject a zombie agent from an old generation.

## 15. Adaptive Playbook

Required playbooks: no Docker/low RAM, agent timeout/crash, spec conflict, scope expansion, migration collision, Git conflict, CI failure, main drift, stale audit, cross-WP failure, and Safe Mode.

## 16. 15 required scenarios

1. Safe parallel work
2. Path ownership collision
3. Resource/migration collision
4. Agent crash/recovery
5. Zombie agent after reassignment
6. Contract change during downstream work
7. Integration hotspot conflict
8. Main drift
9. Harness/tool outage
10. Incomplete/contradictory docs
11. Stale audit SHA
12. Forbidden-path write
13. Independently passing WPs fail together
14. Duplicate/stale message
15. Corrupt orchestration state â†’ Safe Mode

Each scenario must include Trigger, Risk, Evidence, Immediate Action, Forbidden Response, Recovery, Exit Criteria, and Example Lead Response.

## 17. Package Structure

```text
AGENT-ORCHESTRATOR/
  README.md
  START-HERE.md
  handbook/01..07
  prompts/6 role prompts
  templates/9 operational templates
  checklists/5 checklists
  playbooks/8+ incident playbooks
  scenarios/15 scenario files
  case-studies/project-neutral-six-agent-case.md
  examples/5 project examples
  quick-reference/4 references
  meta/BUILD-SPEC.md
  meta/IMPLEMENTATION-PLAN.md
  meta/FINAL-VERIFICATION-REPORT.md
  SHA256SUMS.txt
```

## 18. Quality Requirements

- High-quality technical Vietnamese, clear and direct.
- Each concept includes an example or concrete command when appropriate.
- Prompts must be copy-paste ready with clear placeholders.
- Templates must be usable manually or by AI.
- No prohibited placeholder markers or empty sections.
- Do not claim `production ready` or `100%` when verification is insufficient.
- README must provide a 15-minute path to start.
- Chapter 7 and scenarios must emphasize adaptation to the actual toolchain/project.

## 19. Non-Goals

- Do not build a Python/Node/Rust runtime.
- Do not create a database state store.
- Do not write an executable scheduler.
- Do not depend on a specific model/provider.
- Do not require Docker/Kubernetes.
- Do not promise zero defects; enforce evidence-before-acceptance.

## 20. Acceptance Criteria

The artifact meets requirements when:
1. All required structure and files in section 17 exist.
2. The 7 handbook chapters fully cover the user brief.
3. The 6 prompts include role, constraints, inputs, output contract, and stop/escalation rules.
4. Templates and checklists are usable and contain no stubs.
5. All 15 scenarios use the required format.
6. The project-neutral six-agent case study captures at least four field lessons: missing required output layer, allocated resource slots, controlled scope expansion/security, and external CI verification.
7. A six-agent project-neutral example and a ten-agent project example exist; the overall example set demonstrates applicability to Web, Microservices, Mobile, Distributed Systems, and CLI.
8. A runnable validator checks: file presence, forbidden placeholders, minimum sections, basic internal links, and checksum/archive integrity.
9. The Final Verification Report records exact commands, exit codes, and known limitations.
