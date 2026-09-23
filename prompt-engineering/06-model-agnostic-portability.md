# 06: Model-Agnostic & Harness-Agnostic Prompt Portability

## Nguyên tắc

Canonical prompt mô tả **actions và outcomes**, không gắn với vocabulary của một harness.

Ưu tiên:

```text
Open the relevant file.
Search for the existing pattern.
Run the project test command.
Delegate a focused subtask if the environment supports subagents.
```

Tránh biến thành invariant:

```text
Use the Read tool.
Use Bash.
Use Task tool.
Use TodoWrite.
```

Tên tool cụ thể chỉ xuất hiện khi environment/project contract thật sự yêu cầu.

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
- syntax gọi subagent
- tool names
- model selector
- permission frontmatter
- workspace command
- CI provider command
```

Lead khám phá adaptive shell từ environment thay vì hard-code vào framework.

## Model differences

Không giả định mọi model cần cùng verbosity hoặc reasoning cue. Agent Orchestrator không yêu cầu chain-of-thought, hidden reasoning tags hay một câu “think harder” cố định.

Nếu environment có reasoning-effort control, coi đó là execution setting; prompt vẫn phải chứa success/evidence contract.

## Structured sections

Markdown headers là default portable. XML-like tags có thể dùng khi cần tách instruction/context/example/input rõ ràng, nhưng không bắt buộc.

## Examples

Few-shot examples hữu ích khi cần steer format hoặc edge case. Chỉ dùng ví dụ canonical và đa dạng; không nhồi nhiều ví dụ trùng nhau.

## Portability test

Một canonical prompt đạt portability khi thay Claude Code bằng Codex/Cursor/Gemini/Copilot mà:

- objective không đổi;
- boundaries không đổi;
- evidence bar không đổi;
- chỉ adapter/tool syntax cần thay.
