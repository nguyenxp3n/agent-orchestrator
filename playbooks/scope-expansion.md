# Playbook: Scope Expansion Request

## Trigger
A worker discovers that completing its assigned task requires modifying files outside `ALLOWED_PATHS`.

## Procedure
1. **Explore in-scope alternatives**: Check whether the requirement can be satisfied using dependency injection, local configuration, or interface abstractions.
2. **File formal request**: If modification is unavoidable, the worker submits an explicit Scope Expansion Request detailing:
   - Target file paths requested
   - Technical justification
   - Invariant impact analysis
3. **Evaluate and rule**: The Lead inspects the Ownership Matrix. If the path is unowned, the Lead updates the package contract. If the path belongs to another active worker, the request is rejected and deferred to an Integration Request.