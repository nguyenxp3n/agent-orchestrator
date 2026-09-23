# Playbook: Database Migration Collision

## Trigger
Two branches generate migrations with identical sequence numbers or conflicting schema changes.

## Procedure
1. **Halt integration**: Block merging for both candidate branches.
2. **Validate assigned slots**: Check the Resource Registry to identify which package owned the allocated slot.
3. **Reassign sequence number**: Assign the next sequential migration slot to the unassigned package.
4. **Update candidate branch**: The affected worker updates its migration filename and internal references.
5. **Re-audit modified candidate**: Re-execute migration upgrade and downgrade test suites, capture the new commit SHA, and conduct a fresh audit.