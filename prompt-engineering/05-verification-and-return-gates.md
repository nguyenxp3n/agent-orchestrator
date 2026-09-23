# 05: Verification, Non-Counting Outcomes, and Return Gates

## 1. The challenge of answer-shaped near misses

Language models naturally generate conversational responses that mimic successful outcomes. Common failure patterns include:
- Generating unit tests that contain no assertions or mock all business logic trivially.
- Claiming full implementation after writing only backend models, omitting required frontend views.
- Reporting "All tests passed" without executing the command in the actual workspace.
- Modifying test assertion thresholds to force failing test suites to exit with code 0.

## 2. Non-counting outcome design

Do not evaluate feature completeness by counting files created or lines of code written. Define qualitative predicates that verify true system behavior:

```text
Bad:  Verify that three test files exist in the tests directory.
Good: Execute `pytest tests/review` and verify that the test suite exercises database constraint violations, exiting with code 0.
```

## 3. Designing verifiable return gates

Every task prompt must specify an unambiguous machine-readable return contract:

```text
OUTPUT CONTRACT:
- Report status strictly as WORKER_COMPLETE_CLAIM, BLOCKED, or AWAITING_DECISION.
- Provide exact terminal command strings and integer process exit codes.
- Map every required deliverable file to its verified path on disk.
- Record git diff stats confirming zero modifications outside allowed_paths.
```

Workers report claims backed by command output. The independent Auditor validates these claims before granting acceptance.