# Scenario: Zombie Agent Returns

## Trigger
The previous Worker returns after the WP has been reassigned.

## Risk
Stale commits/messages may overwrite newer work.

## Evidence
Assignment generation; message generation; current owner.

## Immediate Action
Reject stale generation output.

## Forbidden Response
Do not merge because “the old agent completed more work.”

## Recovery Procedure
If the artifact is useful, treat it as an external candidate and reconcile it through an explicit new task; do not overwrite canonical state.

## Exit Criteria
Canonical WP state accepts only the current generation.

## Example Lead Response
```text
Generation 2 returns after generation 3: report STALE_GENERATION.
```
