# 06: Model-Agnostic & Harness-Agnostic Prompt Portability

## Principle

A canonical prompt describes **actions and outcomes**, not the vocabulary of a specific harness.

Prefer:

```text
Open the relevant file.
Search for the existing pattern.
Run the project test command.
Delegate a focused subtask if the environment supports subagents.
```

Do not turn these into invariants:

```text
Use the Read tool.
Use Bash.
Use Task tool.
Use TodoWrite.
```

Specific tool names appear only when the actual environment/project contract requires them.

## Portable core, adaptive shell

```text
Portable Core:
- role
- objective
- success predicate
- boundaries
- evidence contract
- stop conditions

Adaptive Shell:
- subagent invocation syntax
- tool names
- model selector
- permission frontmatter
- workspace command
- CI provider command
```

The Lead discovers the adaptive shell from the environment instead of hard-coding it into the framework.

## Model differences

Do not assume every model needs the same verbosity or reasoning cue. AGENT-ORCHESTRATOR does not require chain-of-thought, hidden reasoning tags, or a fixed â€œthink harderâ€ phrase.

If the environment exposes a reasoning-effort control, treat it as an execution setting; the prompt still requires a success/evidence contract.

## Structured sections

Markdown headers are the portable default. XML-like tags may be used when instructions/context/examples/input need stronger separation, but they are optional.

## Examples

Few-shot examples are useful when steering format or edge cases. Use only canonical, diverse examples; do not load many redundant examples.

## Portability test

A canonical prompt is portable when replacing Claude Code with Codex/Cursor/Gemini/Copilot preserves:

- the objective;
- the boundaries;
- the evidence bar;
- only adapter/tool syntax changes.

