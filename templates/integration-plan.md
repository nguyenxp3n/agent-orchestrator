# Template: Sequential Integration Plan

```text
INTEGRATION_RUN_ID: INT-<timestamp>
INTEGRATOR_ROLE: <integrator identifier>
TARGET_BRANCH: main | integration

INTEGRATION_QUEUE (Topological DAG Order):
1. WP-<id> (Candidate SHA: <sha>, Audit ID: <audit_id>, Status: QUEUED)
2. WP-<id> (Candidate SHA: <sha>, Audit ID: <audit_id>, Status: PENDING_DEPENDENCY)

PER-PACKAGE MERGE PROTOCOL:
For each accepted package in sequence:
1. Verify candidate SHA matches audited SHA exactly.
2. Verify main branch baseline has not drifted.
3. Merge branch:
   git merge --no-ff <branch> -m "merge: integrate <WP_ID>"
4. Apply approved Integration Requests to shared hotspots.
5. Execute cross-package verification gate:
   - Run canonical global test suite
   - Run contract schema validations
   - Run critical end-to-end user journeys
6. If tests pass, proceed to next package.
7. If tests fail: halt, isolate failing candidate, and revert merge.

FINAL RELEASE GATE:
- Cloud CI pipeline run ID: <id>
- Cloud CI status: SUCCESS
- Final commit SHA: <merged main sha>
```