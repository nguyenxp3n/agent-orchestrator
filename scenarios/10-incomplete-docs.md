# Scenario: Incomplete or Contradictory Specifications

## Trigger
A worker discovers that required edge-case behavior is absent from specifications or directly contradicted by existing code.

## Risk
Arbitrary guessing by workers leading to architectural inconsistency.

## Evidence
Worker inquiry highlighting missing specifications or conflicting documents.

## Immediate Action
Instruct the worker to pause the affected function while continuing orthogonal components.

## Forbidden Response
Never encourage the worker to "use your best judgment" on architectural boundaries.

## Recovery Procedure
The Lead consults authoritative documentation, issues a technical ruling via Decision Request, and updates task parameters.

## Exit Criteria
Specification updated; worker implements code conforming to the formal ruling.

## Example Lead Response
```text
Ambiguity confirmed regarding null payload handling. Ruling: Return HTTP 400 with structured error envelope. Spec updated.
```