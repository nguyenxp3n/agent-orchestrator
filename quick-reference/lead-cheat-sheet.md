# Quick Reference: Lead Cheat Sheet

## Before Dispatch
- Complete Project Intake and Project Execution Profile.
- Verify DAG dependencies and freeze shared interface contracts.
- Define `allowed_paths`, `readonly_paths`, and `forbidden_paths` with zero overlaps.
- Pre-allocate migration numbers, ports, routes, and queue topics in Resource Registry.
- Compile prompt using Prompt Compiler; confirm Prompt Quality Gate status is `READY`.

## During Execution
- Answer in-scope questions with concrete constraints; reject out-of-scope modifications.
- Handle specification conflicts, contract edits, and security decisions via Decision Requests.
- If a worker crashes: freeze workspace, capture diffs, increment generation counter, and reassign.

## During Verification and Integration
- `WORKER_DONE != ACCEPTED`. Enforce independent forensic audits on exact commit SHAs.
- Issue actionable `REWORK` directives if any deliverable layer is missing.
- Merge accepted branches sequentially according to the DAG. Run global QA after every merge.
- Report actual assurance levels (`ACCEPTED`, `INTEGRATED`, `RELEASE_VERIFIED`); never claim unverified completion.