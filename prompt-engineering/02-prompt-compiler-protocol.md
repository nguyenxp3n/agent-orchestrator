# 02: Prompt Compiler Protocol

## 1. Purpose

The Prompt Compiler protocol translates project truth, Work Package definitions, boundary constraints, and verification requirements into structured, dispatch-ready role prompts.

## 2. Compilation sequence

```text
Project Truth
+ Work Package Contract
+ Ownership Matrix
+ Resource Registry
+ Acceptance Criteria
+ Verified Evidence
        |
        v
   Prompt Compiler
        |
        v
   Role Prompt Draft
        |
        v
  Prompt Quality Gate
        |
        v
  READY -> Dispatch
```

1. **Establish objective and success predicate**: Formulate the boolean condition defining successful delivery before writing background text.
2. **Resolve source authority**: Identify governing documents by technical domain; mark contradictions as `UNRESOLVED`.
3. **Filter context**: Classify available inputs into `AUTHORITATIVE`, `VERIFY_BEFORE_USE`, and `UNTRUSTED_DATA`. Include file pointers rather than dumping raw code.
4. **Bind ownership and resources**: Inject explicit `allowed_paths`, `readonly_paths`, and `forbidden_paths`. Assign specific resource slots (e.g., migration slot `R2`, port `8081`).
5. **Enumerate deliverables**: List every required file, schema, and documentation artifact.
6. **Formulate non-counting outcomes**: Identify failure modes where literal instructions could be met without satisfying technical intent.
7. **Define verification commands**: Specify exact test commands, required flags, and expected exit codes.
8. **Set stop conditions**: Define explicit triggers requiring immediate cessation and escalation.
9. **Select prompt mode**: Apply `Compact`, `Standard`, or `Long-Horizon` mode based on task risk.
10. **Red-team the draft**: Audit the generated prompt for loopholes, ambiguities, or missing path permissions.
11. **Run Prompt Quality Gate**: Dispatch only when the disposition evaluates to `READY`.

## 3. Operational modes

| Mode | Target Scope | Key Inclusions |
|---|---|---|
| **Compact** | Deterministic bug fixes, minor refactors | Role, Objective, Scope, Expected Deliverable, Test Command, Output Contract |
| **Standard** | Routine feature Work Packages | Full boundary envelope, allocated resources, Success Predicate, verification commands |
| **Long-Horizon** | Open-ended investigations, architectural changes | Term definitions, exhaustive non-counting outcomes, red-team checks, audit gates |