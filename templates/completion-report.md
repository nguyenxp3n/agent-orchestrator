# Template: Worker Completion Report

```text
WP_ID:
GENERATION:
BRANCH:
WORKSPACE:
BASE_SHA:
HEAD_SHA:
STATUS: WORKER_COMPLETE_CLAIM | BLOCKED | AWAITING_DECISION

CHANGED_FILES:
  - <path>

EXPECTED_OUTPUTS_CHECKLIST:
  - [x] <deliverable file on disk>

COMMAND_EVIDENCE:
  - command: <exact command string>
    exit_code: 0
    summary: <test counts and execution summary>

RESOURCE_USAGE:
  - slot: <allocated identifier>
    action: created | modified

DECISION_REQUESTS:
  - <request id or none>

INTEGRATION_REQUESTS:
  - target_file: <shared hotspot path>
    patch_summary: <exact route registration or bootstrap edit>

UNRESOLVED_ITEMS:
  - <known limitation or none>
```