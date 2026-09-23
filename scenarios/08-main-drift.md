# Scenario: Main Drift

## Trigger
Main changes between audit and merge.

## Risk
Audit evidence is based on an old baseline; the new combination has not been verified.

## Evidence
Audited base SHA; current main SHA; candidate SHA.

## Immediate Action
Block automatic promotion; rebuild/revalidate merge candidate.

## Forbidden Response
Do not claim that main drift is irrelevant because the candidate was already audited.

## Recovery Procedure
Rebase/merge current main according to policy, create a new identity when necessary, and rerun gates.

## Exit Criteria
Resulting candidate/integration evidence fresh.

## Example Lead Response
```text
Main receives a security patch after the WP audit; Integrator revalidates with the patch.
```
