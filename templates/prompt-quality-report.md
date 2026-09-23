# Template: Prompt Quality Report

```text
EVALUATION_DATE: <timestamp>
COMPILED_FOR_WP: <package id>
TARGET_ROLE: <role>
EVALUATOR: Lead Orchestrator

QUALITY_DIMENSIONS:
1. Objective & Success Predicate clarity: PASS | FAIL
2. Context hygiene (no raw code dumps): PASS | FAIL
3. Boundary & permission envelope precision: PASS | FAIL
4. Deliverable enumeration completeness: PASS | FAIL
5. Anti-near-miss outcome protection: PASS | FAIL
6. Verification commands and exit codes bound: PASS | FAIL
7. Stop and escalation conditions explicit: PASS | FAIL
8. Machine-readable output contract included: PASS | FAIL

RED-TEAM ADVERSARIAL INSPECTION:
- Potential loophole identified: <description or none>
- Loopholes closed in prompt draft: <description or none>

DISPOSITION: READY | NOT_READY | ESCALATE
RECOMPILE_ACTIONS_REQUIRED: <list of fixes if NOT_READY>
```