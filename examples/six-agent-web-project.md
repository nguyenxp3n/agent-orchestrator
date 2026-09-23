# Example: Web Fullstack with 6 Agents

## Objective

Implement a feature set covering contracts, auth/backend, review/backend, review/frontend, infra, and CI without allowing workers to modify the same hotspot concurrently.

## DAG

```text
WP-100 Contract Freeze
  +--> WP-210 Auth Backend --------+
  +--> WP-220 Review Backend ------+--> WP-600 Integration/E2E
  +--> WP-230 Review Frontend -----+
WP-300 Infra ----------------------+ 
WP-400 CI -------------------------+
```

## Allocation

```text
A1 WP-210: services/auth/**, migration 000021
A2 WP-220: internal/review/**, migration 000022
A3 WP-230: apps/web/src/features/review/**
A4 WP-300: deploy/**, no migration
A5 WP-400: .github/workflows/** only
A6 Auditor: read-only + command execution, no candidate edits
```

The root router and Taskfile are `INTEGRATION_ONLY`.

## Dispatch Wave

After WP-100 is accepted/frozen, A1–A5 may run in parallel if resource checks pass. The Auditor processes candidates as each worker reports completion.

## Integration

```text
merge WP-210 -> global QA
merge WP-220 -> global QA
merge WP-230 + approved route integration -> global QA/E2E
merge Infra/CI according to dependency -> cloud CI verify
```

If the frontend is missing an expected component even though the backend passes, WP-230 is not accepted.