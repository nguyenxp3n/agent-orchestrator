# Template: Prompt Compile Input

```text
TARGET_ROLE: Worker | Auditor | Integrator | Arbitrator
WP_ID: <package id>
OBJECTIVE: <concrete deliverable outcome>
SUCCESS_PREDICATE: <verifiable condition defining success>
SELECTED_PROMPT_MODE: Compact | Standard | Long-Horizon

CONTEXT_REFERENCES:
- Authoritative specs: <file paths and section titles>
- Frozen contracts: <schema file paths>
- Relevant source files: <target files>

BOUNDARY_ENVELOPE:
- allowed_paths: []
- readonly_paths: []
- forbidden_paths: []

ALLOCATED_RESOURCES:
- Sequence numbers:
- Ports / routes / queues:

EXPECTED_OUTPUTS:
- <deliverable file 1>
- <deliverable file 2>

NON_COUNTING_OUTCOMES:
- <anti-near-miss condition 1>
- <anti-near-miss condition 2>

VERIFICATION_COMMANDS:
- <exact test command 1>
- <exact quality gate command 2>

STOP_CONDITIONS:
- <boundary violation, contract drift, schema contradiction>
```