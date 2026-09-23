# 02: Prompt Compiler Protocol

## Objective

The **Prompt Compiler** converts Project Truth + Work Package + Ownership/Resource State into a dispatch-ready prompt. This is a documentation protocol, not a runtime engine.

```text
PROJECT TRUTH
    + WORK PACKAGE
    + OWNERSHIP MATRIX
    + RESOURCE REGISTRY
    + ACCEPTANCE CONTRACT
    + CURRENT EVIDENCE
           ↓
      PROMPT COMPILER
           ↓
    ROLE EXECUTION PROMPT
           ↓
    PROMPT QUALITY GATE
           ↓
       DISPATCH / REJECT
```

## Stage 1: Capture Contract

Determine:

- target role;
- objective;
- desired artifact;
- success predicate;
- inputs and source-of-truth;
- output format;
- known failure cases;
- required autonomy level.

If the success predicate cannot be written yet, the task is not ready for dispatch. Return to decomposition or create a Decision Request.

## Stage 2: Resolve Authority

Classify input:

```text
AUTHORITATIVE
VERIFY_BEFORE_USE
UNTRUSTED_DATA
```

If two authoritative sources conflict on the same subject, do not choose by intuition; create a DR.

## Stage 3: Select Context

Load context on demand:

1. project rules/source index;
2. relevant spec/architecture fragment;
3. relevant files/interfaces/tests;
4. current errors/evidence;
5. conversation state only when it is still relevant.

Do not broadcast the entire repository, spec, or transcripts from other agents when unnecessary.

## Stage 4: Bind Permissions & Resources

Compile explicitly:

- `allowed_paths`;
- `readonly_paths`;
- `forbidden_paths`;
- semantic resource allocations;
- frozen contracts;
- shared hotspots;
- workspace/branch/base identity when applicable.

## Stage 5: Define Completion

Write:

- Success Predicate;
- expected outputs;
- acceptance criteria;
- Non-Counting Outcomes: near misses that do not count as complete.

For a simple task, non-counting outcomes may contain only 1–2 items. For a long/high-risk task, define them in greater detail.

## Stage 6: Bind Evidence

Every important criterion must have a verification method:

```text
Criterion -> Evidence source -> Command/check -> Expected signal
```

If the verifier requires an external service or cloud CI, state the identity that must be matched, such as SHA/run ID.

## Stage 7: Bind Escalation

Stop/Request when:

- scope must expand;
- a resource has not been allocated;
- spec conflict;
- security/secret decision;
- destructive/irreversible action;
- a frozen contract must change;
- candidate/base identity stale.

## Stage 8: Choose Prompt Mode

### Compact Mode

Use for a small, deterministic task with one owner and limited context. Keep role + objective + scope + expected output + verification + output contract.

### Standard Mode

Default for a coding Work Package. Use the complete agent-grade contract.

### Long-Horizon Mode

Use when a task is expensive, long-running, open-ended, or coordinates multiple workers. Add definitions, detailed non-counting outcomes, adversarial failure modes, evidence-ledger/return gate, and contamination rules.

## Stage 9: Render

Write prompts in the imperative mood, with clear sections and action/outcome emphasis. Do not bind canonical prompts to Claude/Codex/Copilot-specific tool names unless the task actually requires them.

## Stage 10: Red-Team

Before dispatching an expensive or high-risk task, ask:

> How could the agent satisfy the literal wording of the prompt while violating the intent?

Check for loopholes:

- narrowed scope;
- missing artifact layer;
- tests are green but belong to the wrong candidate;
- file/resource outside scope is used;
- a plan is returned instead of implementation;
- an unverified assumption is used;
- the agent self-declares PASS.

Patch the prompt through success/non-counting/evidence semantics, not by adding a large number of meaningless MUST/NEVER statements.

## Stage 11: Prompt Quality Gate

Run `prompt-engineering/07-prompt-quality-gate.md`. If a critical field is missing, the state is `NOT_READY`.
