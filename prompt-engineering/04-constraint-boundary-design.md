# 04: Constraint & Boundary Design

## Objective

Constraints must protect invariants and ownership without turning the prompt into an endless prohibition list.

## Path boundaries

```text
ALLOWED_PATHS    = write/delete/rename region
readonly_paths / READONLY_PATHS = readable for context, not writable
forbidden_paths / FORBIDDEN_PATHS = strictly off-limits in the current WP
```

If the project does not use filesystem ownership, replace it with equivalent domain boundaries: package/module/service/schema/resource.

## Semantic resources

Path isolation is insufficient for shared resources. Compile allocations only for resource types that actually exist in the project:

- migration/version slot;
- network port;
- route/endpoint namespace;
- database table/schema;
- event/topic name;
- environment variable namespace;
- feature flag;
- deployment target;
- shared generated registry.

Do not invent a resource type merely because the framework includes an example.

## Constraint taxonomy

### Hard invariant

Violations that invalidate work:

- do not modify forbidden paths;
- do not self-allocate shared resources;
- do not change a frozen public contract;
- do not use a secret from another trust domain;
- do not merge a protected branch.

### Quality constraint

Define the bar:

- follow project conventions;
- tests must prove behavior;
- maintain backward compatibility when required by the contract.

### Preference

Flexibility is acceptable when the trade-off is better:

- naming style for a local helper;
- internal function decomposition;
- implementation technique within ownership.

Do not elevate a preference into a hard rule without a reason.

## Explain the rationale when useful

A non-obvious constraint should include a short rationale:

```text
Do not reuse the staging signing secret in test environments because the trust domains must remain isolated.
```

Rationale helps the model generalize to cases not explicitly listed.

## Scope extension

If the task requires crossing `allowed_paths` or resource allocation:

```text
STOP affected change
-> describe necessity
-> identify requested path/resource
-> explain architectural impact
-> send Decision/Resource/Integration Request
```

Do not “temporarily fix it and report later.”
