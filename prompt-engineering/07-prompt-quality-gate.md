# 07: The Prompt Quality Gate

## 1. Purpose

The Prompt Quality Gate is a pre-dispatch evaluation protocol. The Lead inspects compiled prompts against six critical dimensions to prevent dispatching incomplete, ambiguous, or dangerous instructions.

## 2. Evaluation dimensions

1. **Objective clarity**: Is the success predicate boolean and verifiable?
2. **Context hygiene**: Are context references minimal, authoritative, and free of bulk code dumps?
3. **Boundary rigor**: Are `allowed_paths`, `readonly_paths`, and `forbidden_paths` explicitly declared?
4. **Deliverable completeness**: Are all expected files, schemas, migrations, and documentation items enumerated?
5. **Anti-near-miss protection**: Are non-counting outcomes defined to block superficial completion claims?
6. **Verifiability**: Are exact test commands and expected exit codes specified?

## 3. Quality Gate evaluation matrix

| Gate Check | Evaluation Criterion | Pass Condition |
|---|---|---|
| G-1: Objective | Success predicate is defined and measurable | Yes / No |
| G-2: Boundaries | No path overlaps with active concurrent workers | Yes / No |
| G-3: Resources | All sequence numbers, ports, and routes are pre-allocated | Yes / No |
| G-4: Contracts | Prerequisite upstream interface contracts are frozen | Yes / No |
| G-5: Verification | Commands are discovered from repository truth (no assumed tools) | Yes / No |
| G-6: Escalation | Triggers for halting and filing Decision Requests are explicit | Yes / No |

## 4. Gate disposition

- **`READY`**: All criteria satisfied. Approved for immediate dispatch.
- **`NOT_READY`**: Deficiencies detected. The Lead must recompile the prompt.
- **`ESCALATE`**: Architectural contradictions detected. Escalate to the project authority.

Never dispatch a prompt evaluated as `NOT_READY` or `ESCALATE`.