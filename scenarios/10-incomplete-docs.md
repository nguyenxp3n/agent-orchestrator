# Scenario: Incomplete / Contradictory Docs

## Trigger
Project thiếu docs hoặc docs cùng authority mâu thuẫn.

## Risk
Lead invents architecture/toolchain.

## Evidence
Repository facts; manifests; CI; docs; git history.

## Immediate Action
Record FACT/INFERENCE/UNKNOWN; narrow scope; DR protected conflicts.

## Forbidden Response
Không biến low-confidence inference thành broad ownership.

## Recovery Procedure
Create minimal execution profile and refresh when authority resolved.

## Exit Criteria
Dispatch only work whose critical boundaries are known.

## Example Lead Response
```text
README cũ nói npm, repo hiện pnpm lockfile + CI pnpm: build command authority lấy evidence hiện hành.
```
