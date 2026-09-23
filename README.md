# AGENT-ORCHESTRATOR

**Model-Agnostic Â· Project-Agnostic Â· Adaptive Â· Evidence-Driven**

AGENT-ORCHESTRATOR is an operations handbook for an AI Lead Architect coordinating 5â€“10+ coding agents on the same software project. The package focuses on methodology, prompts, templates, checklists, playbooks, and case studies; it does not implement an orchestration engine, SDK, or scheduler runtime. A Lead can apply this documentation with Claude, GPT/Codex, Gemini/Antigravity, Cursor, Windsurf, or an equivalent harness.

> Central invariant: `WORKER_DONE != ACCEPTED`. A Worker reports a claim; acceptance exists only after independent verification backed by evidence.

## Project-Agnostic: applicable to any software project

AGENT-ORCHESTRATOR applies to multiple software project types: Web Fullstack, Microservices Backend, Mobile App, Distributed Systems, CLI/Developer Tools, and other structures. The Lead needs sufficient project input to build the Project Model and Project Execution Profile before orchestration begins.

Project input commonly includes `docs/`, `spec/`, `plan/`, `workflow/`, the repository tree, build/test manifests, CI config, DB/schema/env information, and project instructions. These are **logical information categories, not mandatory directory names**: if the project uses ADR/RFC/tickets, Makefile, Gradle, Cargo, Xcode, Bazel, Terraform, or equivalent sources, the Lead must discover and use those actual sources.

Principle: **project truth determines framework adaptation; the framework does not force a project to match a sample case study.**

## What problems does the framework address?

When multiple AI agents modify one repository, the largest risks come from coordination across agents rather than isolated coding ability: two agents edit the same migration; one worker crosses into another worker's files; an agent reports completion while omitting the frontend; a branch passes independently but fails after integration; cloud CI differs from local execution; a worker crashes and later returns with stale results; main drifts after audit. AGENT-ORCHESTRATOR converts these risks into a controlled process.

## Four invariants

1. Zero Hallucination: when evidence is absent, record `UNKNOWN`.
2. Zero Trust: an agent report is a claim, not truth.
3. Mandatory Verification: every important transition requires verifiable evidence.
4. Atomic Completion: if one mandatory acceptance criterion is missing, the WP is incomplete.

The framework applies **Strict on outcomes, adaptive on mechanisms**. Isolation is mandatory; the mechanism may be a Git worktree, a separate clone, a container, or a remote workspace. A quality gate is also mandatory, but the Lead must discover commands from the project instead of forcing `task qa` or Docker onto every repository.

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

## Read by purpose

- Start in 15 minutes: [START-HERE.md](START-HERE.md).
- Understand philosophy and role boundaries: [handbook/01-core-philosophy.md](handbook/01-core-philosophy.md).
- Intake, WP, DAG, worktrees: [handbook/02-project-intake-dag-planning.md](handbook/02-project-intake-dag-planning.md).
- Paths, locks, migration slots: [handbook/03-resource-locking-boundaries.md](handbook/03-resource-locking-boundaries.md).
- Prompt Engineering + prompt compiler + role prompts: [handbook/04-ready-to-use-prompts.md](handbook/04-ready-to-use-prompts.md), the `prompt-engineering/` directory, and `prompts/`.
- Independent audit: [handbook/05-zero-trust-forensic-audit.md](handbook/05-zero-trust-forensic-audit.md).
- Sequential integration + CI/CD: [handbook/06-sequential-integration-cicd.md](handbook/06-sequential-integration-cicd.md).
- Edge cases and adaptive handling: [handbook/07-adaptive-playbook.md](handbook/07-adaptive-playbook.md).
- Project-type examples (non-prescriptive): [examples/README.md](examples/README.md).


## Prompt Compiler: convert project truth into a dispatch-ready prompt

AGENT-ORCHESTRATOR does not require the Lead to manually â€œwrite a good promptâ€ for every agent. The Lead compiles prompts from the Work Package, source authority, ownership, resource allocation, expected outputs, and verification contract. Start with `prompt-engineering/01-prompt-anatomy.md`, use `prompts/prompt-compiler.md`, then run the Prompt Quality Gate before dispatch.

Three modes: **Compact** for small tasks, **Standard** for a normal WP, **Long-Horizon** for long/high-risk tasks that require a success predicate, non-counting outcomes, adversarial verification, and an audit-gated return.

## Operating rules

The Lead Orchestrator owns decomposition, architecture constraints, ownership, resources, decision arbitration, audit disposition, and integration order. The Worker implements within the WP scope; the Auditor verifies independently; the Integrator handles approved merges. Separation of duties prevents the Lead from silently modifying code and then accepting the same change without independent review.

## Assurance limits

The framework does not promise zero defects or absolute elimination of hallucination. The enforceable goals are: **do not accept an unverified completion claim, do not intentionally permit writes outside scope, and do not merge a candidate whose identity no longer matches the audit evidence.**
