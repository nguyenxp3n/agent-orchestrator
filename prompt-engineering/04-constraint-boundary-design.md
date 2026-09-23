# 04: Constraint and Boundary Design

## 1. Principles of boundary enforcement

Autonomous coding agents naturally expand their scope when encountering minor friction. A worker tasked with writing an API handler will modify routing manifests, database connection pools, global styles, or third-party dependencies unless strictly bounded.

Constraint design establishes **immutable perimeter envelopes** that allow autonomous problem-solving within assigned directories while blocking uncoordinated modifications to shared infrastructure.

## 2. Defining permission envelopes

Every worker assignment must declare three mutually exclusive path categories:

```yaml
allowed_paths:
  - services/review/internal/**
  - services/review/tests/**
readonly_paths:
  - packages/contracts/review.yaml
  - docs/architecture/review-service.md
forbidden_paths:
  - services/api/router.go
  - db/migrations/**
  - .github/workflows/**
```

- **`allowed_paths`**: Exclusive write, creation, and deletion permissions within the assigned boundary.
- **`readonly_paths`**: Immutable reference material for interfaces, types, and architectural standards.
- **`forbidden_paths`**: Protected global hotspots, shared infrastructure, and directories owned by concurrent workers.

## 3. The fail-closed constraint model

Apply a fail-closed policy: any path not explicitly listed in `allowed_paths` is prohibited by default.

When an implementation requires touching a file outside `allowed_paths`:
1. The worker must immediately halt modifications on that component.
2. The worker files an explicit Decision Request, Scope Expansion Request, or Integration Request.
3. The worker must not apply temporary workarounds or bypass constraints under the assumption that changes will be cleaned up later.