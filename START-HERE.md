# START HERE: Run an Orchestration Session in 15 Minutes

This guide provides the shortest path to orchestrating any software repository. The framework is strictly project-agnostic: it requires no specific language, framework, database, container runtime, or build system.

## 1. Intake project

Gather project inputs: architectural references (`docs/` or equivalent), specifications (`spec/`), implementation plans (`plan/`), operational procedures (`workflow/`), repository tree, build and test manifests, CI configurations, schema definitions, environment examples, and developer guidelines. These directory names illustrate logical categories; repositories do not need to follow these exact names. Record facts, unknowns, and contradictions in the [Project Intake Template](templates/project-intake.md).

Discover commands directly from the codebase rather than assuming defaults:

```bash
find . -maxdepth 2 -type f | sort
git status --short
ls package.json pyproject.toml go.mod Cargo.toml Taskfile.yml Makefile 2>/dev/null
find . -maxdepth 3 -type f | sort
```

## 2. Compile Project Execution Profile

Complete the [Project Execution Profile](templates/project-execution-profile.md): architecture, toolchains, authoritative documents, commands, shared resources, integration hotspots, and unknowns. When two authoritative documents conflict, mark the status as `UNRESOLVED` and escalate rather than silently blending them.

## 3. Decompose into Work Packages

Every Work Package (WP) must represent an independently verifiable unit of delivery. Use the [Work Package Template](templates/work-package.md). Each package must define dependencies, path boundaries, expected outputs, resource allocations, test commands, stop conditions, and acceptance criteria.

## 4. Construct DAG and Ownership Matrix

Example topology:

```text
WP-100 Contracts
   |
   +--> WP-210 Backend Auth ----+
   |                            +--> WP-500 Integration
   +--> WP-220 Frontend Auth ---+

WP-300 Infra can run in parallel if it owns no shared database or API hotspots.
```

Define boundaries using the [Ownership Matrix](templates/ownership-matrix.md) and [Resource Allocation](templates/resource-allocation.md). Allocate migration sequence numbers, ports, and route namespaces before dispatching workers.

## 5. Provision isolated workspaces

Git worktrees provide clean filesystem isolation:

```bash
git worktree add ../wt-wp210 -b feat/wp-210
git worktree add ../wt-wp220 -b feat/wp-220
```

When worktrees are unavailable, use separate clones, containers, or remote workspaces. Core rule: **never allow multiple active workers to write to the same working tree simultaneously.**

## 6. Compile prompts and run Prompt Quality Gate

Populate the [Prompt Compile Input](templates/prompt-compile-input.md), then invoke the [Prompt Compiler](prompts/prompt-compiler.md) using project truth, WP contracts, ownership boundaries, allocated resources, and acceptance criteria. Select `Compact`, `Standard`, or `Long-Horizon` mode based on task complexity.

Evaluate the compiled prompt with the [Prompt Quality Gate](prompt-engineering/07-prompt-quality-gate.md). If the disposition is not `READY`, do not dispatch the prompt.

## 7. Dispatch workers

Dispatch the compiled [Worker Task Assignment Prompt](prompts/worker-task-assignment.md) containing explicit objectives, success predicates, context references, boundaries, resources, non-counting outcomes, verification steps, and output contracts. Maintain an active ledger tracking branch names, worktree paths, assignment generations, allocated resources, and dependency states.

## 8. Worker inquiries and escalation

- In-scope implementation questions: respond with [Clarification Guidance](prompts/clarification-guidance.md).
- Specification conflicts, security decisions, or scope changes: trigger [Architectural Arbitration](prompts/architectural-arbitration.md) with a [Decision Request](templates/decision-request.md).
- Requests for files or resources owned by another WP: reject by default, explore alternatives within assigned scope, or file a formal request.

## 9. Worker completion claim is not acceptance

Workers submit a [Completion Report](templates/completion-report.md). The Lead or an independent Auditor then runs the [Seven-Step Forensic Audit](checklists/seven-step-forensic-audit.md):

```bash
git status --short
git log -1 --stat
git diff <base>...HEAD --name-status
# Run project-specific unit and quality commands from Project Execution Profile
```

If required frontend files, database migrations, documentation, or tests are missing, issue a `REJECT/REWORK` disposition even when isolated unit tests pass.

## 10. Sequential integration

Only candidates marked `ACCEPTED` enter the integration queue. Merge sequentially according to the DAG, never by worker completion order. For repositories permitting merge commits:

```bash
git merge --no-ff feat/wp-210
# Run global QA
git merge --no-ff feat/wp-220
# Run global QA again
```

When the main branch advances after an audit, rebase the candidate, rerun verification, and re-audit before merging.

## 11. Final verification

Apply the [Final Project Gate](checklists/final-project-gate.md). State `FINAL` only when every critical acceptance criterion has verified evidence. When cloud deployment or staging verification cannot run locally, state the actual assurance level rather than claiming unverified completion.