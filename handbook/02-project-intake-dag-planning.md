# Chapter 2: Project intake and overall planning

## 2.0 Project-Agnostic Intake Rule

Do not begin decomposition by copying another project's structure. The Lead must determine project type, authoritative inputs, toolchain, module boundaries, verification commands, and semantic resources from the repository being onboarded. `docs/`, `spec/`, `plan/`, and `workflow/` are input categories; actual files/directories may use different names.

If the project has no DB, omit migration allocation. If it has no frontend, do not create a frontend WP. For Mobile, CLI, or Distributed Systems, the ownership matrix must reflect actual screens/modules/commands/protocols/resources.

## 2.1 Purpose of Project Intake

Do not dispatch an agent until the Lead understands the repository well enough to identify the source of truth, build/test process, module boundaries, shared hotspots, and unknowns that could invalidate planning. Intake does not require reading every line of code; it creates an evidence-based **Project Execution Profile**.

## 2.2 Step 1: Analyze project context

Read in this order of priority:

1. Project instructions: `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, contribution docs.
2. `docs/`, `spec/`, `plans/`, ADRs, API schemas.
3. Repository tree and module manifests.
4. Toolchain: package/build files, task runners, lockfiles.
5. DB/migrations/env samples.
6. Tests and CI/CD.
7. Recent Git history when needed to understand conventions or in-flight migrations.

Sample evidence discovery:

```bash
pwd
git status --short
git log -10 --oneline
find . -maxdepth 2 -type f | sort
find . -maxdepth 4 -type f | sort
```

For a large project, inventory by module rather than dumping every file into context.

## 2.3 From facts to the Project Execution Profile

The profile must answer: primary architecture/modules; canonical commands; source authority by subject; shared integration hotspots; semantic resources; generated/read-only files; frozen contracts; baseline failures; unknowns that block planning.

Do not serialize secrets to “prove” that `.env` exists. Record only the path and policy.

## 2.4 Step 2: Decompose modules into Work Packages

A WP is the smallest unit that can be **assigned, audited, and accepted independently**. A good WP has a clear outcome, boundary, and acceptance contract.

```text
Bad:  WP: Implement the whole platform
Good: WP-A Domain A implementation + assigned resources + tests
      WP-B Client/consumer integration + tests (only if required)
      WP-C Freeze shared contract used by A/B
```

Do not split work so finely that every worker must edit the same central file. Decomposition optimizes **safe parallelism**, not agent count.

## 2.5 Ownership Matrix

Before a parallel wave, build the Code Ownership Matrix:

| WP | allowed_paths | readonly_paths | integration-only | semantic resources |
|---|---|---|---|---|
| WP-A | `<domain-a-path>/**`, allocated resource R1 | `<frozen-contract>` | `<integration-hotspot>` | `<domain-a-resource>` |
| WP-B | `<client-or-consumer-path>/**` | `<frozen-contract>` | `<global-registration-hotspot>` | contract consumer |
| WP-C | `<infra-or-platform-path>/**` | service/runtime manifests | `<shared-schema-or-domain-hotspot>` | assigned ports/resources |

Ownership covers more than paths. Two workers may edit different files while colliding on the same API route or env var.

## 2.6 Step 3: Build the Dependency DAG

Classify dependencies:

- Hard dependency: downstream work cannot start yet.
- Contract dependency: downstream work may start once the contract is frozen.
- Soft dependency: useful but non-blocking.
- Integration dependency: affects merge order/cross-WP gates.

```text
WP-0 Freeze shared contract(s)
  |------> WP-A Domain implementation
  |------> WP-B Consumer/client implementation
  |------> WP-C Platform/integration dependency

WP-A + WP-B + WP-C
  |
  v
WP-Z Cross-WP Integration/E2E
```

If a shared contract is not frozen and producer/consumer workers independently guess the interface, that is a deferred conflict, not parallelism.

## 2.7 Select parallel waves

A WP enters the same wave only when:

```text
all hard deps satisfied
AND no overlapping exclusive path ownership
AND no exclusive resource collision
AND required contracts frozen
AND workspace available
AND verification commands known enough
```

Project-neutral six-agent example:

```text
Wave 0: Contract/architecture freeze
Wave 1 parallel:
  A1 Domain A                Resource Slot R1
  A2 Domain B                Resource Slot R2
  A3 Domain C                Resource Slot R3
  A4 Client/Consumer         Resource Slot R4 or contract-consumer only
  A5 Infrastructure         isolated infra ownership
  A6 CI/CD                  workflow-only ownership
Wave 2: sequential integration by DAG + global QA
```

If A5 discovers it needs a shared resource outside its allocation, it must not create or claim it independently; it must submit a request. In a DB project that resource may be a migration slot, but the framework does not assume a DB.

## 2.8 Step 4: Allocate Git Worktree / Workspace

With Git:

```bash
git worktree add ../wt-wp210 -b feat/wp-210
git worktree add ../wt-wp220 -b feat/wp-220
git worktree list
```

Each coding worker gets its own working tree. Do not place multiple agents in one working tree and merely instruct them not to interfere. Isolation must be physical or enforced by the harness.

If worktrees are unavailable: use a separate clone, per-agent remote workspace, per-agent container/VM; docs-only work may use an isolated directory.

## 2.9 Baseline and branch identity

Before dispatch, record:

```text
wp_id
branch
worktree_path
base_sha
assignment_generation
allowed/readonly/forbidden
resource leases
verification commands
```

The base SHA is required for audit diffing and main drift detection.

## 2.10 Expected Files are part of planning

Do not wait until audit to decide which files must exist. WP planning must list expected outputs by layer:

```text
Project-specific persistence/schema: required artifact(s) if the project has them
Domain implementation: modules/services/types/tests
Client/consumer surface: UI, SDK, CLI registration, mobile screen, adapter or equivalent if required
Contracts: frozen API/proto/schema/interface as applicable
Docs: update only if WP requires
```

Expected files catch half-complete work that tests may miss.

## 2.11 Planning when documentation is incomplete

Adaptive procedure:

1. Inventory facts from code/manifests/CI.
2. Separate `FACT`, `INFERENCE`, and `UNKNOWN`.
3. Use only low-risk inference for planning; convert protected ambiguity into a DR.
4. Do not invent architecture because documentation is missing.
5. Narrow the WP if the boundary is not reliable enough.

## 2.12 Dispatch Gate

```text
[ ] Objective clear
[ ] Dependency state clear
[ ] allowed/readonly/forbidden clear
[ ] shared resources allocated
[ ] expected outputs clear
[ ] acceptance commands discovered
[ ] stop/DR conditions clear
[ ] isolated workspace exists
[ ] base SHA recorded
```

If a critical item is `UNKNOWN`, the WP is not `READY`.