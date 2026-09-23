# Template: Project Execution Profile

## Profile Header

```text
Project:
Baseline SHA:
Architecture summary:
Primary runtime(s):
Primary package/build system:
```

## Source-of-Truth Matrix

| Subject | Primary authority | Secondary evidence | Conflict rule |
|---|---|---|---|
| Business behavior | | | |
| API wire shape | | | |
| Physical DB schema | | | |
| Security | | | |
| Build/QA | | | |
| Deployment | | | |

## Canonical Commands

```text
bootstrap:
build:
unit_backend:
unit_frontend:
lint:
typecheck:
schema_contract:
global_qa:
e2e:
cloud_ci_inspection:
```

## Architecture and Boundaries

```text
Components:
Public interfaces:
Frozen contracts:
Generated files:
Integration hotspots:
```

## Resource Model

```text
Migration policy:
Port policy:
DB ownership:
Route/event ownership:
Secret/env policy:
```

## Workspace Strategy

```text
Preferred isolation: git worktree | clone | container | remote workspace | other
Fallback isolation:
Maximum safe concurrent writers:
```

## Baseline Health

```text
Known passing commands:
Known failing commands:
Known warnings:
Last verified evidence:
```

## Explicit Unknowns

For each unknown, record impact as `BLOCKS_DISPATCH`, `BLOCKS_ACCEPTANCE`, `BLOCKS_RELEASE`, or `NON_BLOCKING`.