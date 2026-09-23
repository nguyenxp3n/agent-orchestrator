# Scenario: Combined WP Failure

## Trigger
WP-A and WP-B pass independently but fail when merged.

## Risk
Cross-module contract/resource mismatch.

## Evidence
Both audit reports; integration diff; global test failures; contracts.

## Immediate Action
Stop queue; classify mismatch and create corrective work.

## Forbidden Response
Do not continue merging later WPs to “see whether it resolves itself.”

## Recovery Procedure
Fix at correct ownership boundary; rerun independent + combined gates as needed.

## Exit Criteria
Integrated SHA passes cross-WP gate.

## Example Lead Response
```text
Backend returns enum old value while frontend accepted against different frozen snapshot.
```
