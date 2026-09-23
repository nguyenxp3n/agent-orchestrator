# Chapter 3: Resource Governance, Boundaries, and Lock Allocation

## 3.1 Permission Envelope for Workers

Every coding worker receives three explicit path groups:

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

- `allowed_paths`: Write, delete, and rename permissions strictly within the assigned package contract.
- `readonly_paths`: Read-only access for reference and type validation.
- `forbidden_paths`: Protected files that must not be modified. If modifications become necessary, submit a formal change request.

Renaming or moving a file constitutes a deletion followed by a creation; both the source and target paths must reside within `allowed_paths`.

## 3.2 Ownership modes

Agent Orchestrator defines explicit permission modes:

- `EXCLUSIVE_WRITE`: Single active writer on the designated path.
- `SHARED_READ`: Concurrent read access across all workers.
- `INTEGRATION_ONLY`: Modifications reserved exclusively for the Integrator upon approved change requests.
- `ALLOCATED_WRITE`: Restricted write access limited to pre-assigned identifiers (such as dedicated migration sequence numbers).
- `APPEND_ONLY`: Sequential append operations under strict formatting rules without rewriting existing history.
- `GENERATED`: Automated build outputs; workers must not manually edit these files.

These controls do not require automated lock daemon software; the Lead enforces them through the Ownership Matrix and operational audit logs.

## 3.3 Semantic Resources beyond directory paths

Collisions frequently occur even when file paths do not overlap:
- Two workers create migrations modifying the same database table.
- Two services claim the same network port.
- Two endpoints register overlapping URL route patterns.
- Two workers assign conflicting semantics to the same environment variable.
- Two consumers publish conflicting payload schemas to the same event topic.

Maintain an explicit Resource Registry:

```text
resource_id | type | owner_wp | mode | value/slot | status | notes
```

## 3.4 Allocation of sequential identifiers

Database migrations and sequence numbers represent classic `ALLOCATED_WRITE` resources. The Lead assigns sequence slots prior to dispatch:

```text
Slot R1 -> WP-A / Worker 1
Slot R2 -> WP-B / Worker 2
Slot R3 -> WP-C / Worker 3
Slot R4 -> WP-D / Worker 4
```

Workers must never determine sequence numbers by scanning local directory listings, as parallel branches will inspect identical baselines and select conflicting numbers.

```text
Worker boundary constraint:
You own allocated resource slot R2 only.
Do not assign R1, R3, R4 or generate additional shared identifiers.
If additional shared resources become necessary, halt and file a RESOURCE_REQUEST.
```

## 3.5 Network ports, environment variables, routes, and message queues

```text
PORT-API-DEV      -> 8081 -> Assigned to WP/API
PORT-RUNNER-DEV   -> 8092 -> Assigned to WP/RUNNER
ENV-REVIEW-KEY    -> Reserved by architecture decision; immutable
ROUTE-/review/*   -> Owned by WP-CONTRACT specification
QUEUE-ai.jobs     -> Event schema owned by core messaging contract
```

Never store actual secret values in the Resource Registry. Record the secret identifier, owner, and access policy; values reside in secure secret managers or protected runtime environments.

## 3.6 Managing shared integration hotspots

Certain centralized files require updates from multiple features: application entrypoints (`main.go`, `index.ts`), dependency injection containers, root router manifests, `Taskfile.yml`, central build manifests, and shared CI pipelines.

Designate these files as `INTEGRATION_ONLY` during parallel execution waves. Workers submit structured Integration Requests upon completing their domain work:

```text
Request: Register /review endpoints
Candidate artifact: <owned-domain-path>/<candidate-artifact>
Target hotspot: services/api/router.go
Rationale: Accepted package requires public route registration
Verification: Full repository test suite + route contract tests
```

The Integrator applies approved registrations sequentially during integration, eliminating merge conflicts and keeping worker write boundaries narrow.

## 3.7 Early boundary enforcement

When a worker requests edits outside its assigned envelope:

1. Validate whether the requested file or resource is truly required to satisfy acceptance criteria.
2. Inspect the Ownership Matrix and DAG to identify the current owner.
3. Guide the worker toward in-scope alternatives (such as dependency injection or interface abstraction).
4. If an external change is unavoidable, stop execution and file a formal Decision Request or Scope Expansion Request.