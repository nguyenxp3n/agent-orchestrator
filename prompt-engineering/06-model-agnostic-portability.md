# 06: Model-Agnostic Portability

## 1. Universal behavioral standards

Agent Orchestrator designs prompts to operate reliably across modern frontier models (Claude, OpenAI Codex, Google Gemini) and developer environments (Cursor, Windsurf, GitHub Copilot, custom agent harnesses).

To maintain portability, canonical prompt instructions describe **abstract actions and verifiable outcomes** rather than relying on vendor-specific tool names:

```text
Vendor-specific instructions (fragile):
Use the Bash tool to run pytest. Then invoke the EditTool on handler.py.

Model-agnostic instructions (portable):
Execute the repository test command (`pytest`) in your workspace. Update `handler.py` to satisfy the interface contract.
```

## 2. Portable core and adaptive shell

Partition prompt architecture into two distinct layers:

1. **Portable Core**:
   - Technical objectives and success predicates
   - File boundaries (`allowed_paths`, `forbidden_paths`)
   - Allocated resources and sequence slots
   - Acceptance criteria and verification commands
   - Output contract format

2. **Adaptive Shell**:
   - Local tool invocation syntax (`run_command`, `edit_file`, `bash`)
   - Platform-specific subagent delegation semantics
   - Environment-specific path formatting (POSIX versus Windows)

Keep the core standardized across all deployments, allowing the host harness to adapt execution syntax to the active environment.