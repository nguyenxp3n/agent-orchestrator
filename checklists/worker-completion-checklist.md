# Checklist: Worker Completion Claim

Before the Worker submits a completion report:

```text
[ ] Correct branch/workspace/generation
[ ] All expected outputs checked one by one
[ ] No write outside allowed_paths
[ ] No modification of readonly/forbidden paths
[ ] Only allocated resources used
[ ] Target/unit tests run with actual exit codes
[ ] Repository-required quality gate run if assigned
[ ] git status inspected
[ ] Changed files listed
[ ] HEAD SHA recorded
[ ] Decision/Integration Requests listed
[ ] Unresolved items explicitly listed
[ ] Report says WORKER_COMPLETE_CLAIM, not ACCEPTED
```

Worker self-check reduces rework but does not replace independent audit.