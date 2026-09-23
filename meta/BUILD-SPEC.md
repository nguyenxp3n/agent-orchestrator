# AGENT-ORCHESTRATOR: Design Specification

**Date:** 2026-09-23  
**Status:** User-authorized autonomous execution across design and governance gates  
**Product Type:** Modular Markdown field manual and operational toolkit, not a software runtime  
**Target Audience:** AI Lead Architects and engineering leads coordinating 5 to 10+ coding agents on shared codebases  

## 1. Objective

Build a model-agnostic, project-agnostic orchestration framework compatible with Claude, GPT, Gemini, Antigravity, Cursor, Windsurf, and other agentic coding harnesses. The framework applies across Web Fullstack, Backend/Microservices, Mobile, Distributed Systems, and CLI projects. It gives an AI Lead Architect the operating model to ingest project requirements, extract decoupled Work Packages, construct execution DAGs, allocate ownership and semantic resource locks, provision isolated workspaces, drive parallel agent execution, arbitrate Decision Requests, conduct independent forensic audits, and execute evidence-backed sequential integration.

## 2. Positioning

Agent Orchestrator is not an executable Python engine, an SDK, a scheduler daemon, or a control-plane service. Agent Orchestrator is an operational field manual and methodology executed directly by the AI Lead Architect. Primitives such as DAGs, state machines, resource locks, recovery procedures, forensic audit attestations, and integration queues are delivered as protocols, contracts, templates, checklists, and incident playbooks rather than a monolithic runtime executable.

## 3. Source Synthesis

1. V1 `agent-orchestrator-framework`: Zero Hallucination, Zero Trust, exhaustive checklist review, strict resource boundaries, atomic completion gates, multi-layer forensic audits, and anonymized field incident logs.
2. V2 `orchestrator-framework-v2.0.0`: Project Intelligence, formal Work Package contracts, dependency DAGs, Ownership Matrices, semantic resource locking and leases, explicit state machines, assignment generation fencing, recovery bundles, 15 conformance scenarios, independent audits, dependency-aware integration queues, and cross-WP acceptance gates.
3. `ai-project-finalization-workflow-v2.0.0`: Subject-based source-of-truth hierarchy, state labels reflecting true assurance levels, explicit stop conditions, and strict audit closure discipline.
4. Legacy runtime prototypes: Role definitions for Orchestrator, Worker, Auditor, and Integrator, combined with the core operational rule that `WORKER_DONE != ACCEPTED`.
5. Field experience from anonymized multi-agent projects: Parallel branch isolation, semantic resource-slot reservation, out-of-scope boundary enforcement, shared-configuration extension control, security-domain boundaries, missing required-layer detection, independent CI verification, and sequential merge pipelines.

## 4. The Five Core Invariants

### 4.1 Zero Hallucination
Unverified claims default to `UNKNOWN`. Never fabricate file paths, terminal command output, CI pipeline statuses, git commits, branches, resource allocations, or completion milestones.

### 4.2 Zero Trust
A worker completion report is an unverified claim. The Lead Architect or an Independent Auditor must independently inspect all mutable artifacts before treating claims as facts.

### 4.3 Mandatory Verification
Every major lifecycle state transition requires verifiable evidence: filesystem artifacts, git diffs, exit codes, test execution logs, CI outputs, API responses, or inspectable logs.

### 4.4 Atomic Completion
A Work Package reaches `ACCEPTED` only when 100% of its mandatory acceptance criteria are satisfied. A passing backend test suite with an incomplete frontend integration remains `INCOMPLETE`.

### 4.5 Project-Agnostic Design
The framework remains decoupled from specific business domains, platforms, or case studies. Project source materials (`docs/`, `spec/`, `plan/`, `workflow/`, or repository truth) determine the project model, ownership boundaries, semantic resources, build commands, and verification criteria. Databases, migrations, frontends, or Docker containers appear only when the target project requires them.

## 5. Adaptive Strictness

The framework enforces outcomes and invariants strictly while keeping operational mechanisms adaptable. Workspace isolation is non-negotiable, but teams can implement it using git worktrees, dedicated directory clones, Docker containers, or isolated cloud workspaces. Quality gates are non-negotiable, but commands conform to the project toolchain: `task qa`, `make test`, `pnpm test`, `cargo test`, `go test ./...`, `pytest`, `dotnet test`, or custom scripts. Never mandate Docker when a project does not use it.

## 6. Separation of Duties

- Lead Orchestrator: Project intake, source-of-truth arbitration, task decomposition, DAG construction, ownership boundaries, resource allocation, task dispatch, Decision Request arbitration, failure recovery, final acceptance, and integration sequencing.
- Worker: Implementation strictly within assigned Work Package boundaries and resource permissions.
- Auditor/Reviewer: Independent verification against raw evidence; forbidden from modifying code during an audit task.
- Integrator: Sequential integration of accepted candidate branches; authorized to resolve shared hotspots solely through approved Integration Requests.

The Lead Architect does not write implementation code directly. When code changes are required, the Lead reassigns the task to a Worker, creates a targeted corrective Work Package, or replaces the Worker. This rule protects clear ownership, auditability, and separation of duties.

## 7. Canonical Lifecycle

`PROJECT INPUT -> INTAKE -> PROJECT MODEL -> WP DECOMPOSITION -> DAG -> OWNERSHIP/RESOURCE ALLOCATION -> ISOLATED WORKSPACES -> PARALLEL EXECUTION -> WORKER_DONE -> FORENSIC AUDIT -> ACCEPTED/REWORK -> INTEGRATION QUEUE -> SEQUENTIAL MERGE -> CROSS-WP/GLOBAL QA -> MAIN/RELEASE`.

The dependency DAG governs the merge order, not the chronological completion time of individual workers.

## 8. State Model

Work Package states: `DRAFT`, `READY`, `ASSIGNED`, `RUNNING`, `BLOCKED`, `AWAITING_DECISION`, `READY_FOR_AUDIT`, `REWORK`, `ACCEPTED`, `INTEGRATING`, `INTEGRATED`, `FAILED`, `CANCELLED`.

Agent runtime states operate independently: `READY`, `RUNNING`, `SUSPECTED_STALLED`, `PAUSED`, `FAILED`, `REASSIGNED`, `ABANDONED`. A worker failure or crash does not mark the Work Package as failed.

## 9. Resource Model

The framework governs both filesystem paths and semantic resources. Ownership modes include: `EXCLUSIVE_WRITE`, `SHARED_READ`, `INTEGRATION_ONLY`, `ALLOCATED_WRITE`, `APPEND_ONLY`, and `GENERATED`.

Semantic resources include: migration sequence numbers, database tables and columns, API endpoints, message event names, message queue names, environment variables, network ports, CLI commands, feature flags, schema versions, and shared public interfaces.

The Lead allocates migration and port slots prior to dispatch. Workers never allocate shared semantic slots autonomously.

## 10. Work Package Contract

Every Work Package specification contains: unique ID, explicit objective, source requirements, upstream dependencies, allowed/readonly/forbidden path boundaries, assigned semantic resources, expected deliverables and artifacts, frozen interfaces, acceptance criteria, verification commands, stop conditions, Decision Request escalation procedures, completion report contracts, and integration requirements.

Workers cannot expand their own scope. Requirements outside the assigned envelope require a formal Decision Request, Resource Request, Ownership Transfer, Integration Request, or a separate follow-on Work Package.

## 11. Source of Truth

Never rely on a single flat precedence hierarchy across all project domains. Authority derives from subject domains: product behavior, wire protocol shapes, physical database schemas, security invariants, deployment topology, and implementation sequence each have designated authoritative sources. When equivalent peer sources conflict, mark the discrepancy `UNRESOLVED` and escalate rather than guessing intent.

## 12. Forensic Audit

Every forensic audit executes at least seven distinct verification layers: git identity, workspace hygiene, scope and diff conformance, expected output files, targeted unit/integration test results, full repository quality gates, and acceptance criteria coverage alongside security and regression checks. Missing evidence defaults to `UNKNOWN` and blocks acceptance. An audit binds immutably to a specific candidate commit SHA. Modifying the branch invalidates previous audit attestations immediately.

## 13. Sequential Integration

Only `ACCEPTED` candidate branches enter the integration queue. Merges proceed in topological dependency order. Each step executes pre-merge compatibility checks followed by post-merge global test runs. Integrators resolve shared integration hotspots directly. If the target branch drifts or the audited SHA expires, the batch requires re-validation.

## 14. Recovery

When an agent stalls or crashes: freeze the workspace, collect runtime status, examine git diffs, identify the latest clean commit, verify held leases, log pending Decision Requests, classify the failure (clean recoverable, dirty recoverable, corrupted, or unsafe-unknown), build a Recovery Bundle, and increment the assignment generation counter. Late messages or commits from previous assignment generations are rejected automatically.

## 15. Adaptive Playbooks

The operational toolkit includes targeted playbooks for: environments without Docker or low RAM, agent timeout or process crashes, specification conflicts, scope expansion attempts, migration sequence collisions, git merge conflicts, CI pipeline failures, main branch drift, stale audit attestations, cross-WP integration failures, and safe mode recovery.

## 16. The 15 Mandatory Conformance Scenarios

1. Safe parallel execution
2. Path ownership collision
3. Resource and migration sequence collision
4. Agent crash and recovery
5. Zombie agent rejection after reassignment
6. Contract modification during downstream execution
7. Integration hotspot conflict
8. Target branch drift
9. Toolchain and test harness outages
10. Incomplete or contradictory specifications
11. Stale audit commit SHA
12. Unauthorized write to forbidden paths
13. Individually passing Work Packages failing when combined
14. Duplicate or stale agent messages
15. Corrupted orchestration state triggering Safe Mode

Every scenario document specifies: Trigger, Risk, Observable Evidence, Immediate Action, Prohibited Responses, Recovery Procedure, Exit Criteria, and Example Lead Responses.

## 17. Repository Structure

```text
AGENT-ORCHESTRATOR/
  README.md
  START-HERE.md
  CHANGELOG.md
  LICENSE.md
  handbook/01..07
  prompts/7 canonical prompts
  prompt-engineering/01..07
  templates/11 operational templates
  checklists/5 checklists
  playbooks/10 incident playbooks
  scenarios/15 scenario files
  case-studies/project-neutral-six-agent-case.md
  examples/6 project examples
  quick-reference/5 reference documents
  meta/BUILD-SPEC.md
  meta/IMPLEMENTATION-PLAN.md
  meta/FINAL-VERIFICATION-REPORT.md
  meta/SOURCE-SYNTHESIS-MAP.md
  SHA256SUMS.txt
```

## 18. Quality Requirements

- Professional, direct, human-written technical English.
- Concrete examples and shell commands accompanying abstract concepts.
- Copy-paste ready prompts with clear, explicit placeholder markers.
- Functional operational templates ready for manual completion or LLM-driven generation.
- Zero placeholder markers or empty sections in production documents.
- No unsupported claims of production readiness without complete verification evidence.
- A concise 15-minute quickstart workflow inside `README.md` and `START-HERE.md`.
- Explicit guidance in Handbook Chapter 7 and scenario playbooks for adapting to project-specific toolchains.

## 19. Non-Goals

- No standalone Python, Node.js, or Rust runtime engine.
- No database-backed state store daemon.
- No executable scheduling background service.
- No dependency on vendor-locked model APIs.
- No mandatory Docker or Kubernetes runtime requirements.
- No guarantee of zero defects without enforcing evidence-before-acceptance.

## 20. Acceptance Criteria

The framework artifact meets acceptance criteria when:
1. All directory structures and required files in Section 17 exist on disk.
2. The 7 handbook chapters fully cover all core orchestration domains.
3. Canonical prompts define role, context, constraints, inputs, output contracts, and escalation rules.
4. Templates and checklists are functional and complete without stubs.
5. All 15 operational scenarios follow the standard specification format.
6. The project-neutral six-agent case study documents key operational lessons: missing deliverable layers, allocated resource slots, controlled scope expansion, and independent CI verification.
7. Architectural examples cover 6-agent and 10-agent configurations across Web, Microservices, Mobile, Distributed Systems, and CLI domains.
8. Deterministic validation verifies file presence, heading consistency, prohibited placeholders, relative links, and checksum integrity.
9. The final verification report documents exact commands, execution outputs, and residual operational risks.