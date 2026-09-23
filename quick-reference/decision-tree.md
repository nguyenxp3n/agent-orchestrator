# Quick Reference: Decision Tree

## Can a Work Package be dispatched?

```text
Hard dependencies satisfied?      No  -> WAIT
Contracts frozen if required?     No  -> WAIT / DECISION_REQUEST
Path boundary collision?          Yes -> REPLAN OWNERSHIP
Resource identifier collision?    Yes -> REALLOCATE SLOTS
Workspace isolated?               No  -> PROVISION WORKTREE / CLONE
Critical unknowns remain?         Yes -> DECISION_REQUEST
All criteria satisfied?           Yes -> DISPATCH WORKER
```

## Worker requests changes outside scope?

```text
Can be solved within scope?       Yes -> Provide in-scope guidance
Target is a shared hotspot?       Yes -> Submit Integration Request
Path owned by another worker?     Yes -> Reject / Transfer / New WP
Implicates security or contracts? Yes -> Escalate via Decision Request
Bounded minor extension?          Yes -> Grant explicit path + extra audit
```

## Worker reports completion?

```text
Inspect Completion Report -> Exact candidate commit SHA -> Forensic Audit
Missing deliverables or tests fail -> REWORK
All required evidence verified    -> ACCEPTED
```

## Resolving merge conflicts?

```text
Textual overlap with identical semantics -> Integrator resolves directly
Semantics governed by frozen contract    -> Conform strictly to contract
Semantic ambiguity exists                -> Submit Decision Request
Planning boundary violation              -> Halt and reconstruct packages
```