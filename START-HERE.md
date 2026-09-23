# START HERE: Run an orchestration session in 15 minutes

This document is the shortest path to starting **any software project**. The framework is Project-Agnostic: it does not require the project to be Web-based, use Docker, include a database, or use any specific toolchain.

## 1. Intake project

Collect project input: architecture documentation (`docs/` or equivalent sources), specifications (`spec/` or equivalent), plans (`plan/`), workflows (`workflow/`), repository tree, build/test manifests, CI config, schema/migrations when present, env examples, and project instructions. These directory names only illustrate the source categories to locate; the project does not need to follow this convention. Record facts, unknowns, and contradictions with the [Project Intake Template](templates/project-intake.md).

Do not guess commands. The Lead must discover:

```bash
find . -maxdepth 2 -type f | sort
git status --short
ls package.json pyproject.toml go.mod Cargo.toml Taskfile.yml Makefile 2>/dev/null
find . -maxdepth 3 -type f | sort
```

## 2. Compile Project Execution Profile

Fill in the [Project Execution Profile](templates/project-execution-profile.md): architecture, toolchain, authoritative docs, commands, shared resources, integration hotspots, unknowns. If two sources with equal authority conflict, the state is `UNRESOLVED`; do not merge them by assumption.

## 3. Decompose Work Packages

Each WP must be independently reviewable. Use the [Work Package Template](templates/work-package.md); dependency, path scope, expected outputs, resources, tests, stop conditions, and acceptance criteria are mandatory.

## 4. Build the DAG and Ownership Matrix

Example:

```text
WP-100 Contracts
   |
   +--> WP-210 Backend Auth ----+
   |                            +--> WP-500 Integration
   +--> WP-220 Frontend Auth ---+

WP-300 Infra can run parallel if it owns no shared DB/API hotspot.
```

Use the [Ownership Matrix](templates/ownership-matrix.md) and [Resource Allocation](templates/resource-allocation.md). Allocate migration slots/ports before dispatch.

## 5. Create isolated workspaces

Git worktree is a strong default when the project uses Git:

```bash
git worktree add ../wt-wp210 -b feat/wp-210
git worktree add ../wt-wp220 -b feat/wp-220
```

If the environment does not support worktrees, use a separate clone/container/remote workspace. The invariant is **no two active writers write to the same working tree**.

## 6. Compile the prompt and run the Prompt Quality Gate

Fill in [Prompt Compile Input](templates/prompt-compile-input.md), then use the [Prompt Compiler](prompts/prompt-compiler.md) to compile a prompt from Project Truth + WP + Ownership + Resources + Acceptance. Choose `Compact`, `Standard`, or `Long-Horizon` mode based on complexity.

Run the [Prompt Quality Gate](prompt-engineering/07-prompt-quality-gate.md). If the disposition is not `READY`, **do not dispatch**.

## 7. Dispatch worker

Send the compiled [Worker Task Assignment Prompt](prompts/worker-task-assignment.md) with the exact objective, success predicate, context references, boundaries, resources, non-counting outcomes, verification, and output contract. The Lead keeps a ledger of branch/worktree, generation, resources, and dependency status.

## 8. When the worker asks a question

- Implementation question within scope → use [Clarification Guidance](prompts/clarification-guidance.md).
- Spec/reality conflict, security decision, scope expansion → use [Architectural Arbitration](prompts/architectural-arbitration.md) + [Decision Request](templates/decision-request.md).
- Worker requests a file/resource owned by another WP → deny by default, then find an in-scope alternative or create a formal request.

## 9. Worker reports done ≠ done

The Worker must submit a [Completion Report](templates/completion-report.md). An independent Auditor/Lead then runs the [Seven-Step Forensic Audit](checklists/seven-step-forensic-audit.md).

```bash
git status --short
git log -1 --stat
git diff <base>...HEAD --name-status
# chạy project-specific unit / quality commands từ Project Execution Profile
```

If required frontend/migration/docs/tests are missing from the rubric, the disposition is `REJECT/REWORK` even when tests for the implemented portion are green.

## 10. Integrate sequentially

Only an `ACCEPTED` candidate enters the queue. Merge according to the DAG, not “who finished first.” When policy permits merge commits:

```bash
git merge --no-ff feat/wp-210
# run global QA
git merge --no-ff feat/wp-220
# run global QA again
```

If main changes after audit, rebase/recreate the candidate and re-audit whenever candidate identity changes.

## 11. Finish

Use the [Final Project Gate](checklists/final-project-gate.md). State `FINAL` only when every critical criterion has evidence; if cloud/deployment verification cannot run, report the current assurance level instead of upgrading status through wording.