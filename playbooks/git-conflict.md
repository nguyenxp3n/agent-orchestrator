# Playbook: Git Merge Conflict Resolution

## Trigger
Git reports merge conflicts during candidate branch integration.

## Procedure
1. **Inspect conflicting hunks**:
   ```bash
   git diff --ours -- <file>
   git diff --theirs -- <file>
   ```
2. **Determine conflict type**:
   - Textual overlap with identical technical intent: The Integrator resolves the conflict directly.
   - Shared integration hotspot with approved Integration Request: The Integrator applies the approved registration patch.
   - Divergent contract semantics: Halt integration immediately and submit a Decision Request.
   - Two workers modified the same exclusive path: Planning boundary defect. Roll back and re-align package definitions.