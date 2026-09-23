# AGENT-ORCHESTRATOR

**Model-Agnostic · Project-Agnostic · Adaptive · Evidence-Driven**

Agent Orchestrator is an operational field manual for an AI Lead Architect coordinating 5 to 10+ coding agents on a single software repository. The package provides methodologies, prompts, templates, checklists, playbooks, and case studies. It does not ship a standalone orchestration engine, SDK, or scheduler daemon. A Lead can apply these operating standards directly on Claude, OpenAI Codex, Gemini, Cursor, Windsurf, or equivalent agent harnesses.

> Core Invariant: `WORKER_DONE != ACCEPTED`. A worker report represents a completion claim. Acceptance occurs only after independent verification backed by concrete evidence.

## Project-Agnostic: built for any software repository

Agent Orchestrator applies across diverse architectures: Web Fullstack, Microservices Backend, Mobile Applications, Distributed Systems, CLI Developer Tools, and hybrid stacks. The Lead requires sufficient project input to construct a Project Model and a Project Execution Profile before scheduling work.

Project inputs include architecture documents (`docs/`), specifications (`spec/`), implementation plans (`plan/`), operational workflows (`workflow/`), repository trees, build and test manifests, CI configurations, schema definitions, environment specifications, and developer instructions. These represent logical categories rather than mandatory folder names. When a repository uses Architecture Decision Records (ADRs), RFCs, issues, Makefiles, Gradle, Cargo, Xcode, Bazel, or Terraform, the Lead discovers and works with those existing sources.

Principle: **Repository truth drives framework adaptation. The framework never forces a project to fit an arbitrary template.**

## Problem scope

When multiple AI agents modify a shared repository, the primary failure mode is coordination breakdown rather than syntax generation:
- Multiple agents generate conflicting database migrations.
- A worker silently edits files assigned to another worker.
- An agent reports completion while omitting required frontend layers or tests.
- A branch passes tests in isolation but breaks upon merge.
- Cloud CI environments diverge from local test environments.
- An agent crashes, restarts, and resubmits stale state.
- Main branch drift invalidates earlier audit results.

Agent Orchestrator establishes an enforceable workflow to isolate, audit, and serialize these operations.

## Four core invariants

1. Zero Hallucination: record `UNKNOWN` whenever evidence is absent. Never fabricate repository state or test outcomes.
2. Zero Trust: treat every agent report as an unverified claim until independently validated.
3. Mandatory Verification: require verified test runs, clean diffs, and artifact checks before state transitions.
4. Atomic Completion: mark a work package incomplete if any mandatory acceptance criterion is missing or failing.

The framework enforces a rule of **strict outcomes, adaptive mechanisms**. Workspace isolation is non-negotiable, whether implemented through Git worktrees, separate clones, containers, or remote workspaces. Quality gates are equally non-negotiable, but the Lead discovers the actual verification commands from the project rather than imposing rigid tool assumptions.

## Standard lifecycle

```text
PROJECT INPUT
  -> INTAKE
  -> PROJECT MODEL
  -> WORK PACKAGE DECOMPOSITION
  -> DEPENDENCY DAG
  -> OWNERSHIP + RESOURCE ALLOCATION
  -> ISOLATED WORKSPACES
  -> SAFE PARALLEL EXECUTION
  -> WORKER_DONE
  -> ZERO-TRUST FORENSIC AUDIT
  -> ACCEPTED / REWORK
  -> INTEGRATION QUEUE
  -> SEQUENTIAL MERGE
  -> CROSS-WP + GLOBAL QA
  -> MAIN / RELEASE
```

## Reading guide

- Fast-track setup (15 minutes): [START-HERE.md](START-HERE.md).
- Operating philosophy and role boundaries: [handbook/01-core-philosophy.md](handbook/01-core-philosophy.md).
- Intake, work packages, DAG scheduling, and worktrees: [handbook/02-project-intake-dag-planning.md](handbook/02-project-intake-dag-planning.md).
- Path ownership, file locks, and migration allocation: [handbook/03-resource-locking-boundaries.md](handbook/03-resource-locking-boundaries.md).
- Prompt Engineering, prompt compiler, and role prompts: [handbook/04-ready-to-use-prompts.md](handbook/04-ready-to-use-prompts.md), `prompt-engineering/`, and `prompts/`.
- Independent forensic audit: [handbook/05-zero-trust-forensic-audit.md](handbook/05-zero-trust-forensic-audit.md).
- Sequential integration and CI/CD gates: [handbook/06-sequential-integration-cicd.md](handbook/06-sequential-integration-cicd.md).
- Edge cases, crash recovery, and adaptive playbooks: [handbook/07-adaptive-playbook.md](handbook/07-adaptive-playbook.md).
- Architecture examples (non-prescriptive): [examples/README.md](examples/README.md).

## Prompt Compiler: compiling project truth into dispatch prompts

Agent Orchestrator eliminates manual, ad-hoc prompt writing. The Lead compiles targeted execution prompts directly from Work Package definitions, source authority, ownership matrices, allocated resources, expected artifacts, and verification commands. Reference `prompt-engineering/01-prompt-anatomy.md`, use `prompts/prompt-compiler.md`, and validate outputs against the Prompt Quality Gate before dispatching workers.

Three operating modes:
- **Compact**: For isolated bug fixes and small tasks with constrained token budgets.
- **Standard**: For routine feature work packages requiring standard boundary enforcement.
- **Long-Horizon**: For complex, multi-turn tasks requiring explicit success predicates, non-counting outcome checks, adversarial red-teaming, and audit-gated returns.

## Operating discipline

The Lead Orchestrator is not an all-purpose worker. The Lead owns task decomposition, architectural constraints, resource governance, decision arbitration, audit disposition, and integration sequencing. Workers own code implementation strictly within their assigned boundaries. Auditors own objective verification. Integrators own approved branch merges. Strict separation of duties prevents the Lead from casually modifying source code and rubber-stamping its own unverified changes.

## Warranty boundaries

The framework does not guarantee defect-free software or total elimination of model errors. Its enforceable objective is operational: **reject unverified completion claims, prevent writes outside assigned scopes, and block merges when candidates drift from audited state.**