# Chapter 2: Project Intake, Work Packages, and DAG Planning

## 2.0 Project-Agnostic intake rule

Never begin work package decomposition by copying arbitrary conventions from another project. The Lead discovers the target project type, authoritative documents, build systems, module boundaries, verification commands, and shared semantic resources directly from the assigned repository. Labels like `docs/`, `spec/`, `plan/`, and `workflow/` indicate informational categories; actual files and directories may use different names.

If the project has no database, omit migration allocations entirely. If the project lacks a frontend, do not construct frontend packages. When working on mobile apps, CLI utilities, or distributed systems, the ownership matrix must reflect the actual screens, command registries, network endpoints, or protocol schemas present in that codebase.

## 2.1 Objectives of Project Intake

Never dispatch coding agents before understanding where authority resides, how the project builds and tests, where module boundaries lie, which files are shared integration hotspots, and what unknowns might derail planning. Intake does not require reading every line of source code; it produces an evidence-backed **Project Execution Profile**.

## 2.2 Step 1: Context discovery

Examine project inputs in order of priority:

1. Repository guidelines: `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, `CONTRIBUTING.md`.
2. Specifications: `docs/`, `spec/`, `plans/`, ADRs, and OpenAPI or Protobuf schemas.
3. Code structure: repository file tree and module manifests.
4. Toolchain definitions: package manifests, build scripts, task runners, lockfiles.
5. Data definitions: database migrations, schema definitions, and environment templates.
6. Verification suites: unit tests, integration tests, and CI/CD pipelines.
7. Recent Git history: recent commits to identify conventions and in-flight migrations.

Baseline discovery commands:

```bash
pwd
git status --short
git log -10 --oneline
find . -maxdepth 2 -type f | sort
find . -maxdepth 4 -type f | sort
```

For large monorepos, catalog components by module rather than loading every source file into the context window.

## 2.3 Compiling the Project Execution Profile

The execution profile documents:
- Primary architecture patterns and core modules
- Canonical build, lint, and test commands
- Authoritative documentation by subject matter
- Shared integration hotspots
- Semantic resources (ports, routes, database tables)
- Generated or read-only files
- Frozen interfaces and API contracts
- Pre-existing baseline failures
- Critical unknowns blocking dispatch

Never include raw credentials or secrets in project documentation. Record file paths and credential policies only.

## 2.4 Step 2: Work Package decomposition

A Work Package (WP) is the smallest deliverable unit that can be **dispatched, audited, and merged independently**. An effective package defines clear deliverables, strict boundaries, and explicit acceptance criteria.

```text
Bad:  WP: Implement user management platform
Good: WP-A User domain service implementation, allocated resources, and unit tests
      WP-B User interface views and integration tests (dependent on WP-A contract)
      WP-C Freeze shared user authentication contract consumed by WP-A and WP-B
```

Avoid decomposing tasks so granularly that multiple workers must edit the same central bootstrap file simultaneously. Decomposition optimizes for **safe parallelism**, not sheer agent quantity.

## 2.5 Ownership Matrix

Before dispatching parallel workers, compile the Code Ownership Matrix:

| WP | allowed_paths | readonly_paths | integration-only | semantic resources |
|---|---|---|---|---|
| WP-A | `<domain-a-path>/**`, allocated resource R1 | `<frozen-contract>` | `<integration-hotspot>` | `<domain-a-resource>` |
| WP-B | `<client-or-consumer-path>/**` | `<frozen-contract>` | `<global-registration-hotspot>` | contract consumer |
| WP-C | `<infra-or-platform-path>/**` | service manifests | `<shared-schema-or-domain-hotspot>` | assigned ports/resources |

Ownership encompasses more than directory paths. Two workers editing separate files can still collide on identical API routes, database tables, or environment variable keys.

## 2.6 Step 3: Constructing the Dependency DAG

Classify dependencies systematically:

- **Hard dependency**: Downstream work cannot commence until upstream implementation finishes.
- **Contract dependency**: Downstream work can proceed in parallel once interface contracts are frozen.
- **Soft dependency**: Helpful context that does not strictly block execution.
- **Integration dependency**: Dictates merge sequencing and cross-package testing order.

```text
WP-0 Freeze shared contracts
  |------> WP-A Domain service implementation
  |------> WP-B Consumer client implementation
  |------> WP-C Platform infrastructure configuration

WP-A + WP-B + WP-C
  |
  v
WP-Z Cross-Package Integration and End-to-End Verification
```

If producer and consumer workers implement complementary sides of an unverified interface without a frozen contract, they create deferred integration conflicts rather than genuine parallel progress.

## 2.7 Scheduling parallel execution waves

A Work Package enters an active parallel wave only when:

```text
all hard dependencies are satisfied
AND no overlapping write permissions exist with active workers
AND no shared resource collisions exist
AND required interface contracts are frozen
AND isolated workspaces are provisioned
```

If safety guarantees cannot be demonstrated, serialize the execution sequence.

## 2.8 Step 4: Workspace provisioning

Use Git worktrees to isolate parallel agents:

```bash
git worktree add ../wt-wp210 -b feat/wp-210
git worktree add ../wt-wp220 -b feat/wp-220
```

When worktrees are unsupported, provision isolated repository clones, containers, or dedicated remote workspaces. Never permit two active coding agents to write to the same working tree concurrently.