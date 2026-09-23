# Scenario: Path Ownership Collision

## Trigger
Two WPs require write access to the same exclusive path/hotspot.

## Risk
Overwrite, merge conflict, ownership ambiguity.

## Evidence
Exact allowed paths; planned edits; repository hotspot list.

## Immediate Action
Reject the parallel plan; split the hotspot into an Integration Request or serialize/transfer ownership.

## Forbidden Response
Do not use “be careful not to edit the same lines” as the primary control mechanism.

## Recovery Procedure
Recompile WP boundaries and workspaces.

## Exit Criteria
Only one active writer owns the region; the new plan has a clear audit boundary.

## Example Lead Response
```text
Collision at services/api/router.go: mark the file INTEGRATION_ONLY and have workers output route modules only.
```
