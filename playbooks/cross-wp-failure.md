# Playbook: Cross-Package Integration Failure

## Trigger
Two Work Packages pass individual audits, but the test suite fails when both are merged.

## Procedure
1. **Halt integration queue**: Block further merges to main.
2. **Isolate the conflict**: Compare diffs across both packages to identify implicit dependencies, shared database state, or port collisions.
3. **Classify root cause**:
   - Interface mismatch: Package B made invalid assumptions about Package A.
   - Shared resource contention: Both packages modified the same table or namespace.
   - Ordering dependency: Packages must execute in a specific sequential order.
4. **Assign corrective work**: Create a targeted corrective Work Package assigned to the appropriate boundary, or submit a Decision Request if specifications are ambiguous.