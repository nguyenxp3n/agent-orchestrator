# Example: Fullstack Web Project with 6 Agents

## Objective
Deliver a feature set encompassing frozen contracts, authentication backend, review backend, review frontend views, deployment infrastructure, and CI pipelines without concurrent contention on shared hotspots.

## Dependency DAG
```text
WP-100 Contract Freeze
  +--> WP-210 Auth Backend --------+
  +--> WP-220 Review Backend ------+--> WP-600 Integration/E2E
  +--> WP-230 Review Frontend -----+
WP-300 Infra ----------------------+ 
WP-400 CI -------------------------+
```

## Boundary and Resource Allocation
```text
Agent 1 (WP-210): services/auth/**, Migration Slot 000021
Agent 2 (WP-220): internal/review/**, Migration Slot 000022
Agent 3 (WP-230): apps/web/src/features/review/**
Agent 4 (WP-300): deploy/**, zero database migrations
Agent 5 (WP-400): .github/workflows/** only
Agent 6 (Auditor): Read-only repository access + command execution
```

Root application router and central build scripts are designated `INTEGRATION_ONLY`.

## Dispatch Wave
Following approval and freezing of WP-100 contracts, Agents 1 through 5 execute concurrently in isolated worktrees. The Auditor evaluates candidates as individual workers submit completion claims.

## Sequential Integration
```text
merge WP-210 -> global QA
merge WP-220 -> global QA
merge WP-230 + apply approved route registration -> global QA and E2E
merge Infra and CI branches -> cloud pipeline verification
```

If the frontend package omits required interface components, WP-230 is rejected even if backend endpoints pass all tests.