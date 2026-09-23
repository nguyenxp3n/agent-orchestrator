# Scenario: Duplicate or Redundant Worker Reports

## Trigger
A worker submits multiple conflicting completion claims for the same task.

## Risk
Processing duplicate reports or evaluating stale candidate commits.

## Evidence
Multiple completion messages with identical or differing commit SHAs.

## Immediate Action
Inspect the assignment generation counter. Process only the message matching the active generation.

## Forbidden Response
Never process multiple reports simultaneously or evaluate unverified commits.

## Recovery Procedure
Verify branch HEAD on disk. Bind audit evaluation strictly to the current commit SHA.

## Exit Criteria
Single audit executed against the verified branch HEAD commit SHA.

## Example Lead Response
```text
Duplicate completion report received. Discarding duplicate. Auditing active branch HEAD (commit a1b2c3d, Generation 1).
```