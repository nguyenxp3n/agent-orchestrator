# Playbook: Cloud CI/CD Failure

## Trigger
Local quality gates pass, but cloud CI pipeline fails on the candidate branch or main branch.

## Procedure
1. **Inspect pipeline logs directly**: Use platform CLI tools (`gh run view <id> --log-failed`) to inspect failing jobs.
2. **Isolate failure category**:
   - Environment / Runner: Missing secrets, outdated OS images, or runner cache drift.
   - Filesystem: Case-sensitivity differences between local OS and Linux runners.
   - Timing: Race conditions or service container readiness timeouts.
   - True candidate defect: Missed dependency or uncommitted test file.
3. **Reproduce locally**: Run the failing step with identical environment variables and clean cache.
4. **Fix policy**: If the failure is caused by CI infrastructure, address the configuration file. Never modify application logic to mask infrastructure failures.