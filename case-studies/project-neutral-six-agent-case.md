# Case Study: Project-Neutral Six-Agent Parallel Delivery

## 1. Purpose

This case study **does not represent a mandatory project structure**. It provides a neutral reference model demonstrating how an AI Lead Orchestrator coordinates six concurrent agents across any software repository characterized by parallel workstreams, shared resources, and integration hotspots.

Labels such as `Domain A`, `Domain B`, `Domain C`, `UI/Client`, `Infrastructure`, and `CI/CD` are illustrative roles. In actual practice, the Lead substitutes real domains discovered from repository inputs.

Example domain mappings:

| Project Type | Domain A/B/C Examples | UI/Client Examples | Common Shared Hotspots |
|---|---|---|---|
| Web Fullstack | Auth, billing, search | Web application views | Central router, database migrations, OpenAPI schema |
| Microservices Backend | Service A, B, C | Consumer/client SDK | Protobuf schemas, event contracts, API gateway |
| Mobile App | Sync engine, user profile, cache | Native feature screens | Navigation graph, shared model types, API client |
| Distributed System | Scheduler, worker, storage | Operator CLI / admin console | Protocol schemas, state coordination, network ports |
| CLI Tool | Flag parser, command groups, config | Terminal UX, help formats | Root command registry, global config schema |

## 2. Project input as the single source of truth

The Lead begins with actual repository inputs rather than pre-baked case study templates:

```text
Architecture documentation (docs/ or equivalent)
Formal specifications (spec/ or equivalent)
Implementation plans (plan/ or equivalent)
Engineering workflows (workflow/ or equivalent)
Repository filesystem tree
Build and test manifests
CI/CD configuration files
Database schema and migration models (if applicable)
Environment profiles and constraints
```

Directory names like `docs/`, `spec/`, `plan/`, and `workflow/` are common examples. When a project uses ADRs, RFCs, issues, Makefiles, Cargo workspaces, Xcode configurations, Gradle, Bazel, Helm, or Terraform, the Lead discovers and compiles them into the Project Execution Profile.

## 3. Generalized six-agent topology

```text
Lead Orchestrator
  |
  +-- A1 Domain A          [Resource Slot R1]
  +-- A2 Domain B          [Resource Slot R2]
  +-- A3 Domain C          [Resource Slot R3]
  +-- A4 UI / Client       [Resource Slot R4 or contract consumer]
  +-- A5 Infrastructure    [Isolated infrastructure ownership]
  +-- A6 CI/CD             [Pipeline workflow ownership]

All workers -> Completion Claim -> Independent Audit
Accepted queue -> DAG-aware Sequential Integration -> Global QA -> Cloud/External CI
```

A `Resource Slot` may represent a database migration sequence number, network port, queue topic, schema namespace, CLI command flag, or feature flag. Repositories without databases do not construct artificial migration slots.

## 4. Incident A: Worker reports completion while omitting required layers

Suppose Work Package D specifies:
```text
Backend service implementation
Client UI integration
Contract adaptation
Unit and integration tests
```

The worker reports that backend logic and unit tests pass cleanly, but client interface components were never created. The Auditor concludes:

```text
Implemented subset: PASS
Tests for implemented subset: PASS
Required client interface deliverables: MISSING
Work Package disposition: REJECT_REWORK / INCOMPLETE
```

In a CLI project, "client interface" might correspond to root command registration and help outputs; in microservices, it might represent consumer integration adapters. The governing principle is **Atomic Completion**, not a specific file path.

## 5. Incident B: Worker attempts uncoordinated edits to shared hotspots

Suppose the Infrastructure Worker discovers a need to modify a shared routing table, gateway configuration, or central bootstrap entrypoint outside its assigned boundary.

The Lead rejects direct write access because:
- The resource belongs to another package or is designated `INTEGRATION_ONLY`.
- Shared hotspots carry broad cross-package blast radiuses.
- The worker does not hold authority over the global domain contract.

Resolution: The worker maintains changes within its assigned scope and submits a formal `Integration Request` or `Resource Request`, which the Integrator applies during sequential integration.

Operational rule: **Rejecting scope expansion early is far less costly than resolving merge collisions late.**

## 6. Incident C: Controlled and bounded scope extensions

Not every out-of-scope request is invalid. When a worker proves that modifying a shared build script is required to expose its deliverable, the Lead grants **minimal write access to that specific file**, accompanied by strict invariants and extra test gates:

```text
Granted path: <shared-build-configuration-file>
Conditions:
- Existing build tasks and targets must remain intact
- Repository quality gate must exit with code 0
- Contract schema validation must pass
```

The framework does not mandate specific build runners; projects may use `Makefile`, `package.json`, `Cargo.toml`, `build.gradle`, `justfile`, Bazel, or Xcode settings.

## 7. Incident D: Direct verification of external CI runs

A worker claiming that cloud CI passed represents an unverified assertion. The Lead or Auditor inspects the pipeline execution directly:

```bash
gh run list --branch <branch> --limit 10
gh run view <run-id>
```

For GitLab, Bitbucket, Jenkins, or Buildkite, use equivalent APIs or CLIs. When external pipeline access is unavailable, record `UNVERIFIED_EXTERNAL_CI` rather than assuming success.

## 8. Sequential integration in practice

Following independent audit approval, branches are never merged simultaneously. The Lead and Integrator execute sequential merges governed by DAG prerequisites, running global cross-package test suites after every step:

```text
ACCEPTED WP-A
   -> merge to main
   -> global verification gate
ACCEPTED WP-B
   -> merge to main
   -> global verification gate
```

## 9. Key takeaways

1. Actual repository inputs determine task decomposition, not generic case studies.
2. The Lead maps expected deliverables to actual project types and concrete Work Packages.
3. Semantic resources must be allocated before concurrent workers begin execution.
4. Shared hotspots must not be assigned to multiple concurrent workers simultaneously.
5. The Lead rejects unapproved scope expansion requests by default.
6. Legitimate scope extensions must be strictly bounded, explicit, and audited with additional verification gates.
7. External CI runs require direct evidence; missing access must be recorded as `UNVERIFIED`.
8. `WORKER_DONE != ACCEPTED`.
9. Integration sequence follows the dependency DAG, never worker completion speed.
10. The framework assumes no mandatory technology stack: it operates on any programming language, build tool, or deployment model.