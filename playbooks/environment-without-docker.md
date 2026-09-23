# Playbook: Environment Without Docker / Low RAM

## Trigger
Project documentation references containers, but the execution environment has no Docker or insufficient resources.

## Adaptation
Identify the properties that must be preserved: isolation, DB/service semantics, reproducibility. Choose a separate local service, in-memory/fake dependency for unit tests, remote disposable dependency, separate clone, or reduced concurrency.

```text
If isolation cannot be proven -> reduce parallel writers.
If production-specific behavior cannot be reproduced -> mark that evidence UNKNOWN and block the assurance level that requires it.
```

## Exit Criteria
The replacement mechanism is recorded in the Project Execution Profile; relevant tests run; limitations remain visible.
