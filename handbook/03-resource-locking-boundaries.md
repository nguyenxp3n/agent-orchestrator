# Chapter 3: Resource governance and conflict prevention

## 3.1 Permission Envelope for each Worker

Each worker receives three path groups:

```yaml
allowed_paths:
  - <owned-domain-path>/**
readonly_paths:
  - api/openapi.yaml
  - docs/architecture/**
forbidden_paths:
  - services/api/main.go
  - .github/workflows/**
```

- `allowed_paths`: write/delete/rename within the contract.
- `readonly_paths`: readable but not writable.
- `forbidden_paths`: do not touch; if access is required, create a request.

A rename/move counts as delete + create, so both source and destination must be valid.

## 3.2 Ownership modes

AGENT-ORCHESTRATOR uses the following conceptual modes:

- `EXCLUSIVE_WRITE`: one active writer.
- `SHARED_READ`: multiple readers.
- `INTEGRATION_ONLY`: only the Integrator/an approved change request may modify it.
- `ALLOCATED_WRITE`: only the allocated instance may write, for example a migration slot.
- `APPEND_ONLY`: append according to rules; do not rewrite history.
- `GENERATED`: generated file; workers do not edit it directly.

A runtime lock manager is not required to apply these modes. The Lead may manage them with an Ownership Matrix + ledger as long as allocation is explicit and auditable.

## 3.3 Semantic Resources can matter more than paths

Collisions may occur even when paths do not overlap: two migrations target the same table; two services use the same port; two modules claim the same route; two workers assign different meanings to the same env var; two consumers use the same event name with different schemas.

The Resource Registry should include:

```text
resource_id | type | owner_wp | mode | value/slot | status | notes
```

## 3.4 Migration Slots

Migrations are a common example of `ALLOCATED_WRITE`. The Lead allocates slots before dispatch:

```text
R1 -> WP-A / Agent 1
R2 -> WP-B / Agent 2
R3 -> WP-C / Agent 3
R4 -> WP-D / Agent 4
```

A Worker must not â€œtake the next numberâ€ by inspecting the directory, because parallel branches may independently choose the same number.

```text
You own allocated resource slot R2 only.
Do not allocate R1, R3, R4 or create an additional shared identifier.
If another shared resource becomes necessary, stop and send RESOURCE_REQUEST.
```

## 3.5 Port, env, route, and queue allocation

```text
PORT-API-DEV      -> 8081 -> WP/API
PORT-RUNNER-DEV   -> 8092 -> WP/RUNNER
ENV-REVIEW-KEY    -> reserved by security decision, not reusable
ROUTE-/review/*   -> contract owner WP-CONTRACT
QUEUE-ai.jobs     -> event contract owner
```

Do not copy secret values into the Resource Registry; record only the name/owner/policy. The value remains in the secret manager or controlled environment.

## 3.6 Integration Hotspots

Files that many workers often want to modify include: root router, application bootstrap, central DI, `Taskfile.yml`, root package manifest, global schema registry, shared CI workflow.

Mark them `INTEGRATION_ONLY` when multiple WPs run in parallel. The Worker submits an Integration Request:

```text
Request: register /review routes
Candidate artifact: <owned-domain-path>/<candidate-artifact>
Desired hotspot: services/api/router.go
Reason: accepted WP requires registration
Verification: repository global tests + route contract test
```

The Integrator applies the change after audit, reducing merge conflicts and broad ownership.

## 3.7 Block conflicts early

When an agent requests an out-of-scope change:

1. Determine whether the file/resource is actually required for acceptance.
2. Check ownership/DAG to identify the current owner.
3. Find an in-scope solution.
4. If none exists, choose a constrained scope extension, Integration Request, ownership transfer, new resource allocation, or new WP.
5. Record the decision.

```text
Do not grant write access to migrations/** or services/api/** for WP-INFRA.
Both regions belong to other WPs and Migration Slots are already allocated.
Keep the solution within deploy/** and services/runner-worker/**.
If the health-check requires API wiring, submit an Integration Request; do not modify the hotspot directly.
```

This general pattern prevents an Infrastructure/Platform Worker from opportunistically changing a shared schema, root registration, or domain resource.

## 3.8 Controlled scope extension

Some requests are valid. If a Worker must modify shared build/task configuration to expose a deliverable, the Lead opens only the exact file/resource required and adds an invariant:

```text
Granted: <shared-build-or-task-config>
Conditions:
- preserve all existing commands/tasks
- project quality gate must remain green
- contract/spec validation must remain green when applicable
- no unrelated refactor
```

A good scope extension is small, explicit, reversible, and subject to additional verification.

## 3.9 Security boundary

Resource reuse can create a security defect. If an agent asks to reuse a deletion HMAC key for content review, the Lead must reject it because the two security domains are different.

```text
Reuse secret across domains? -> reject by default
Weaken fail-closed behavior?  -> escalate
Broaden privilege?            -> prefer narrower design
```

## 3.10 Lease transfer and crash handling

A resource belongs to the WP and does not necessarily belong to the process/agent:

```text
WP-B owns resource slot R2
Agent-4 generation 2 crashes
Agent-7 generation 3 resumes WP-B
R2 remains WP-B's slot
Agent-4 generation 2 returning later is stale
```

Do not recycle a migration identifier merely because the previous agent disappeared.

## 3.11 Boundary Audit

```bash
git diff --name-status <base>...HEAD
git status --short
```

Compare changed paths against allowed/readonly/forbidden scope. Then inspect semantic diff: routes, migration identifiers, DB objects, env vars, ports. Filename-only inspection is insufficient.

## 3.12 Chapter operating principle

Safe parallelism depends on **explicit ownership + preallocated resources + isolation + post-work audit**. As agent count increases, the Lead must narrow boundaries and move shared hotspots out of worker branches whenever possible.
