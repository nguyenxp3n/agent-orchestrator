# Playbook: Specification Contradiction

## Trigger
Authoritative documentation contradicts existing code, or two specification documents make conflicting claims.

## Procedure
1. **Pause affected package**: Halt work on the conflicting component; continue orthogonal tasks.
2. **Determine domain authority**: Identify which source holds governing authority for the specific domain (e.g., OpenAPI schemas govern API wire contracts; migrations govern database schemas).
3. **Escalate equal-authority conflicts**: If sources of equal precedence conflict without documented precedence, construct a formal Decision Request presenting trade-offs to the human project authority.
4. **Recompile task prompt**: Once resolved, update authoritative documentation, record the decision in the Decision Ledger, and recompile the affected worker prompt.