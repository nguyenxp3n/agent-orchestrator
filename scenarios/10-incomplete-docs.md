# Scenario: Incomplete / Contradictory Docs

## Trigger
Project documentation is missing or equal-authority documents conflict.

## Risk
Lead invents architecture/toolchain.

## Evidence
Repository facts; manifests; CI; docs; git history.

## Immediate Action
Record FACT/INFERENCE/UNKNOWN; narrow scope; DR protected conflicts.

## Forbidden Response
Do not turn low-confidence inference into broad ownership.

## Recovery Procedure
Create minimal execution profile and refresh when authority resolved.

## Exit Criteria
Dispatch only work whose critical boundaries are known.

## Example Lead Response
```text
An old README says npm, while the repo now has a pnpm lockfile + pnpm CI: derive build-command authority from current evidence.
```
