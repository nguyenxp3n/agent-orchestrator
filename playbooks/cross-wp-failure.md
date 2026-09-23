# Playbook: Cross-WP Failure

## Trigger
Two or more WPs pass independent audit but fail after integration.

## Procedure
Stop promotion. Reconstruct merge order, contract versions, migrations/resources, and integration-only changes. Determine whether the failure is contract mismatch, ordering, hidden shared state, or baseline drift.

```text
Independent PASS + Independent PASS != Combined PASS
```

Create a corrective WP at the correct ownership boundary or a DR if architecture is unresolved.

## Exit Criteria
The combined gate passes on the integrated SHA; corrective evidence is traceable; the failure is not hidden by disabling tests.
