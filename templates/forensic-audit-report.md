# Template: Forensic Audit Report

```text
AUDIT_ID: AUDIT-<WP_ID>-<timestamp>
WP_ID: <package id>
WORKER_ROLE: <worker identifier>
ASSIGNMENT_GENERATION: <integer>
BRANCH: <branch name>
BASE_SHA: <base commit sha>
CANDIDATE_SHA: <audited commit sha>

VERIFICATION_STEPS:
1. Git identity: MATCH | MISMATCH (SHA verified)
2. Workspace hygiene: CLEAN | DIRTY (status porcelain check)
3. Diff inspection: COMPLIANT | SCOPE_VIOLATION (respects allowed_paths)
4. Targeted unit tests: PASS | FAIL (exit code 0)
5. Repository quality gate: PASS | FAIL (zero regressions against baseline)
6. Expected deliverables: 100% PRESENT | INCOMPLETE (all layers exist)
7. Contract and security checks: PASS | FAIL (invariants preserved)

COMMAND_LOGS:
- command: <targeted test command>
  exit_code: 0
  output_snippet: <summary log>
- command: <global quality gate command>
  exit_code: 0
  output_snippet: <summary log>

FINDINGS:
- [SEVERITY] <description and location of issue, or none>

DISPOSITION: ACCEPT | REJECT/REWORK | ESCALATE
REWORK_INSTRUCTIONS: <actionable feedback if rejected, or none>
RESIDUAL_RISKS: <untested cloud/deployment dependencies>
```