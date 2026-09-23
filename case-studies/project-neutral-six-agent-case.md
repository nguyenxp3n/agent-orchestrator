# Case Study: Project-Neutral Six-Agent Parallel Delivery

## 1. Purpose

This case study **does not define a required project structure**. It is a neutral model showing how a Lead Orchestrator coordinates six agents on any software project with parallel work, shared resources, and integration hotspots.

The labels `Domain A`, `Domain B`, `Domain C`, `UI/Client`, `Infrastructure`, and `CI/CD` are illustrative roles. In actual use, the Lead must replace them with real domains discovered from project input.

Example mapping:

| Project type | Domain A/B/C may be | UI/Client may be | Common shared hotspot |
|---|---|---|---|
| Web Fullstack | auth, billing, search | web feature/module | router, DB migrations, OpenAPI |
| Microservices Backend | service A/B/C | consumer/client SDK | proto/schema, event contract, gateway |
| Mobile App | sync, profile, offline cache | screens/features | navigation, shared model, API contract |
| Distributed System | scheduler, worker, storage | admin/control client | protocol/schema, leader state, ports |
| CLI Tool | parser, command groups, config | CLI UX/help | root command registry, config schema |

## 2. Project input is the source of truth

The Lead does not start from the case study name. The Lead starts from actual project input:

```text
architecture docs / docs-equivalent
specifications / spec-equivalent
implementation plan / plan-equivalent
workflow / engineering process
repository tree
build + test manifests
CI configuration
DB/schema/migration model if applicable
environment constraints
```

The directory names `docs/`, `spec/`, `plan/`, and `workflow/` are common examples. If the project uses ADRs, RFCs, tickets, Makefile, a Cargo workspace, an Xcode project, Gradle, Bazel, Helm, Terraform, or another structure, the Lead must discover those sources and compile them into the Project Execution Profile.

## 3. Generalized six-agent model

```text
Lead Orchestrator
  |
  +-- A1 Domain A          [Resource Slot R1]
  +-- A2 Domain B          [Resource Slot R2]
  +-- A3 Domain C          [Resource Slot R3]
  +-- A4 UI / Client       [Resource Slot R4 or contract consumer]
  +-- A5 Infrastructure    [isolated infra ownership]
  +-- A6 CI/CD             [workflow ownership]

All workers -> Completion Claim -> Independent Audit
Accepted queue -> DAG-aware Sequential Integration -> Global QA -> Cloud/External CI
```

A `Resource Slot` may be a migration ID, port, queue name, schema namespace, event name, CLI command namespace, feature flag, or another semantic resource. If the project has no migrations, do not invent a migration concept.

## 4. Incident A: Worker reports done while a required layer is missing

Assume WP-D requires:

```text
backend/service implementation
client/UI integration
contract adaptation
unit tests
```

The Worker reports that backend and tests are green, but the client/UI artifact does not exist. The Auditor must conclude:

```text
Implemented subset: PASS
Tests for implemented subset: PASS
Required client/UI outputs: MISSING
WP: REJECT_REWORK / INCOMPLETE
```

For a CLI project, “client/UI” may map to root command registration/help output; for microservices, it may map to a consumer contract or integration adapter. The governing principle is **Atomic Completion**, not a specific file path.

## 5. Incident B: Worker requests access to a shared resource outside scope

Assume the Infrastructure Worker discovers that a shared schema/gateway/root bootstrap change is needed outside its ownership.

The Lead denies the direct write because:

- the resource already belongs to another WP or is `INTEGRATION_ONLY`;
- the shared hotspot has cross-WP blast radius;
- the worker does not own that domain contract.

Resolution: keep implementation within allowed scope, submit an `Integration Request` or `Resource Request`, or create a corrective WP if the dependency genuinely requires a change.

Lesson: **rejecting scope expansion early costs less than resolving a late collision**.

## 6. Incident C: Controlled scope extension

Some out-of-scope requests are valid. If the Worker proves that a shared build/task configuration must change to expose the deliverable, the Lead may grant **the smallest necessary file/resource**, with explicit invariants and quality gates.

Neutral example:

```text
Granted: <shared-build-or-task-config>
Conditions:
- preserve existing commands/tasks
- project quality gate remains green
- spec/contract validation remains green when applicable
```

The framework does not force `Taskfile.yml`; a project may use `Makefile`, `package.json`, `Cargo.toml`, `build.gradle`, `justfile`, Bazel, Xcode settings, or another tool.

## 7. Incident D: Verify External CI directly

A worker report that cloud CI succeeded is only a claim. The Lead/Auditor must verify a run tied to the exact candidate SHA through the relevant provider.

GitHub example:

```bash
gh run list --branch <branch> --limit 10
gh run view <run-id>
```

GitLab/Bitbucket/Jenkins/Buildkite or another system uses the corresponding API/CLI. If access is unavailable, the state is `UNVERIFIED_EXTERNAL_CI`; do not promote it to PASS.

## 8. Sequential Integration

After independent acceptance, candidates must not be merged in bulk. The Lead/Integrator selects order by DAG/resource dependencies, integrates one candidate at a time, and runs cross-WP/global verification after each step.

```text
ACCEPTED WP-A
   -> integrate
   -> global gate
ACCEPTED WP-B
   -> integrate
   -> global gate
...
```

## 9. Derived operating principles

1. Actual project input determines decomposition, not the case study.
2. The Lead must map expected outputs to the actual project type and WP.
3. Semantic resources must be allocated before concurrent writers exist.
4. Shared hotspots must not be assigned to multiple workers at the same time.
5. The Lead must block unapproved scope expansion.
6. A valid scope extension must be bounded, explicit, and subject to additional verification.
7. External CI/status requires direct evidence or must be recorded as `UNVERIFIED`.
8. `WORKER_DONE != ACCEPTED`.
9. Integration order follows dependencies, not worker speed.
10. The framework does not assume Web, DB, Docker, migrations, or any specific toolchain.
