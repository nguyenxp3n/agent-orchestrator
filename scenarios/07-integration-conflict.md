# Scenario: Git Merge Conflict During Integration

## Trigger
Git reports merge conflicts when integrating an accepted candidate into main.

## Risk
Corrupting codebase logic through incorrect conflict resolution.

## Evidence
Git merge conflict markers and failure logs during sequential integration.

## Immediate Action
Halt integration. Compare conflicting hunks against frozen specifications.

## Forbidden Response
Never accept code hunks blindly based on which branch was committed more recently.

## Recovery Procedure
The Integrator resolves textual overlaps matching frozen contracts. If semantic contradictions exist, submit a Decision Request.

## Exit Criteria
Merged main branch passes global quality gates and cross-package test suites.

## Example Lead Response
```text
Merge conflict in router.go. Semantics governed by WP-100 contract. Integrator resolves conflict and executes full QA gate.
```