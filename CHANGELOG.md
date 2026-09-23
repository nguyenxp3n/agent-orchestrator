# Changelog

## Prompt Engineering Upgrade: 2026-09-23

- Thêm **Prompt Engineering System** gồm 7 module: anatomy, compiler protocol, context engineering, boundary design, verification/return gates, model-agnostic portability và Prompt Quality Gate.
- Thêm `prompts/prompt-compiler.md`, Prompt Compile Input, Prompt Quality Report và compiler cheat sheet.
- Nâng 6 canonical role prompts để dùng chung objective/success predicate/context references/ownership/resources/non-counting outcomes/evidence/stop/output semantics.
- Thêm ba mode: Compact, Standard, Long-Horizon.
- Tích hợp progressive disclosure, context trust classes và just-in-time context selection.
- Thêm adversarial red-team step cho prompt đắt/rủi ro và non-counting outcomes để chống answer-shaped near miss.
- Canonical prompt content chuyển sang action/outcome wording để tăng portability giữa Claude, Codex, Copilot, Cursor, Gemini và harness tương đương.
- Bổ sung source synthesis từ addyosmani/agent-skills, github/awesome-copilot, anthropics/claude-plugins-official, wshobson/agents và Agent-Skills-for-Context-Engineering.

## Release: 2026-09-23

- Project-Agnostic hardening: loại bỏ project identity/case-specific naming khỏi artifact; generalize six-agent case, domain labels, resource slots, shared-config examples và intake semantics.
- Định vị lại framework thành field manual + operational toolkit, không phải Python orchestration runtime.
- Dung hợp V1 discipline, V2 DAG/state/resource/recovery, finalization assurance levels và các field incidents đã được anonymize.
- Chuẩn hóa bốn invariants: Zero Hallucination, Zero Trust, Mandatory Verification, Atomic Completion.
- Thêm Adaptive Strictness để giữ chất lượng nhưng không ép Docker/toolchain cụ thể.
- Tách prompt library, templates, checklists, playbooks, 15 scenarios, examples và quick references.
- Giữ `WORKER_DONE != ACCEPTED`, immutable audit identity, generation fencing và sequential dependency-aware integration.
