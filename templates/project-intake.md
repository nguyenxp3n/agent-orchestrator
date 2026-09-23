# Template: Project Intake Record

```text
PROJECT_NAME:
REPOSITORY_PATH:
INTAKE_DATE: <timestamp>
DISCOVERY_STATUS: COMPLETE | IN_PROGRESS

1. DISCOVERED INPUTS:
- Architecture documentation: <path or none>
- Formal specifications: <path or none>
- Implementation plans: <path or none>
- Engineering guidelines: <path or none>
- Build and test manifests: <list of files>
- CI/CD configurations: <list of files>

2. CLASSIFIED EVIDENCE:
- FACTS (Verified directly from repository files):
  * <fact>
- INFERENCES (Deduced patterns requiring verification):
  * <inference>
- UNKNOWNS (Missing documentation or unverified tools):
  * <unknown>

3. TOOLCHAIN VALIDATION:
- Verified runtime versions:
- Verified test runner exit codes:
- Clean working tree confirmed:

4. CRITICAL CONTRADICTIONS:
- <identified contradictions between specifications and code, or none>

DISPOSITION: READY_FOR_DECOMPOSITION | BLOCKED_MISSING_INPUTS
```