# Scenario: Combined Multi-Package Test Failure

## Trigger
Branches for Package A and Package B merge cleanly without textual conflicts, but the combined test suite fails.

## Risk
Deploying broken software resulting from latent integration bugs.

## Evidence
Global test suite failures following sequential merge.

## Immediate Action
Halt integration. Revert the last merged branch from the integration baseline.

## Forbidden Response
Never push forward and attempt live patches on the broken integration branch.

## Recovery Procedure
Isolate the semantic interaction causing the regression. Create a corrective Work Package to resolve the incompatibility.

## Exit Criteria
Clean integration branch passes full global test suite and end-to-end user journeys.

## Example Lead Response
```text
Combined integration test failed on user checkout flow. Reverting WP-220 merge. Creating corrective package WP-225.
```