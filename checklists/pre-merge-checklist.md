# Pre-Merge Checklist

The Integrator verifies this checklist before merging an accepted candidate branch into main.

## 1. Candidate identity and audit currency
- [ ] Candidate commit SHA matches the exact SHA approved in the signed audit report.
- [ ] Audit disposition is verified as `ACCEPT`.
- [ ] Main branch has not drifted since the audit was performed. If main has advanced, candidate must be rebased and re-audited.

## 2. Dependency ordering
- [ ] Upstream prerequisites in the DAG are already merged into the target branch.
- [ ] Merge sequence strictly follows topological order in the DAG.

## 3. Shared hotspot preparation
- [ ] Any required edits to shared routers or bootstrap files are documented in an approved Integration Request.
- [ ] Approved integration patches adhere strictly to frozen interface specifications.