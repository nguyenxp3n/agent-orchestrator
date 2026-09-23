# Source Synthesis Map

## V1: agent-orchestrator-framework

Giữ lại: Zero Trust/Zero Hallucination, exhaustive barem review, resource boundary enforcement, strict completion gate, forensic Git/filesystem/test verification và các field incidents đã được anonymize.

## V2: orchestrator-framework-v2.0.0

Giữ lại ở mức methodology: Project Intelligence; Work Package contract; DAG; ownership modes; semantic resources; locks/leases; assignment generation; separate agent/WP states; Recovery Bundle; stale generation rejection; independent audit; immutable audit target; dependency-aware integration; Cross-WP Gate; 15 conformance scenarios.

Loại bỏ khỏi Agent Orchestrator: yêu cầu phải có Python runtime, persistent control plane, executable scheduler, schema validator service hoặc adapter runtime.

## AI Project Finalization Workflow V2

Giữ lại: source authority theo subject; state names là assurance levels; protected stop conditions; audit closure discipline; không suy `BUILD_READY` thành production validation.

## Runtime prototype cũ

Chắt lọc role prompts Orchestrator/Worker/Auditor/Integrator và evidence-before-acceptance. Không tái sử dụng runtime code làm lõi sản phẩm.

## Field Experience: anonymized six-agent project

Chỉ giữ bài học tổng quát: isolated workspaces; allocated semantic resource slots; infrastructure scope rejection; bounded shared-config extension; security-domain separation; missing-required-layer detection; external CI verification; sequential merge.

# Prompt Engineering Research Inputs: 2026-09-23

Các nguồn sau được dùng để **synthesis nguyên lý**, không copy nguyên prompt hoặc biến framework thành hệ thống phụ thuộc vendor.

## addyosmani/agent-skills

Source: `https://github.com/addyosmani/agent-skills`

Giữ lại:

- skills phải mô tả executable workflow thay vì chỉ cung cấp reference prose;
- verification, exit criteria, anti-patterns và trigger conditions nên explicit;
- progressive context thay vì nhồi mọi kiến thức vào một prompt.

### Specific context-engineering skill

Source: `https://github.com/addyosmani/agent-skills/blob/main/skills/context-engineering/SKILL.md`

Giữ lại:

- context hierarchy: persistent rules → spec/architecture → relevant source → current errors/evidence → history;
- chỉ load relevant spec fragment;
- đọc file/test/interface liên quan trước khi edit;
- classify external/generated content thấp trust hơn project-owned source;
- context quality > context quantity.

## github/awesome-copilot

Source: `https://github.com/github/awesome-copilot`

Giữ lại:

- agent prompt anatomy: identity, responsibilities, methodology, constraints, output expectations;
- focused subagent wrapper với minimal shared context + exact expected outputs;
- imperative/action-oriented instructions;
- role-specific tool ceilings khi harness hỗ trợ;
- reusable skills/instructions/agents là các lớp khác nhau, không nên dồn vào một prompt khổng lồ.

## anthropics/claude-plugins-official

Source: `https://github.com/anthropics/claude-plugins-official`

Giữ lại:

- progressive disclosure: lean core + references/examples/scripts chỉ khi cần;
- skill/agent trigger descriptions phải cụ thể;
- imperative instructions và focused domain boundaries;
- validation/testing của trigger/quality trước khi coi artifact hoàn thành.

Vendor-specific frontmatter/tool syntax không trở thành invariant của framework.

## wshobson/agents

Source: `https://github.com/wshobson/agents`

Giữ lại:

- one Markdown source có thể phục vụ nhiều harness nếu body nói về actions thay vì tool vocabulary;
- context file nên là table of contents, chi tiết nằm ở on-demand references;
- enforce invariants, không hard-code implementation;
- quality evaluation nên có deterministic structural layer + semantic evaluation + repeated reliability checks khi đáng giá.

Model mapping và harness adapters của repo này chỉ là reference, không được hard-code vào Agent Orchestrator.

## muratcankoylan/Agent-Skills-for-Context-Engineering

Source: `https://github.com/muratcankoylan/agent-skills-for-context-engineering`

Giữ lại:

- context bao gồm toàn bộ inference state, không riêng prompt text;
- subagents chủ yếu tạo context isolation;
- long-horizon brief cần definitions, exact success predicate, non-counting outcomes, adversarial failure-mode checklist, artifact-based reporting và audit-gated return;
- persistence phải đi cùng verification;
- early worker independence/diversity có giá trị trong open-ended search;
- prompt nên lean/outcome-first, không over-prescribe path khi invariant/outcome đã đủ rõ.

## Prompt Synthesis Principle

Agent Orchestrator Prompt Engineering System áp dụng nguyên tắc:

```text
Compile project truth, do not decorate vague requests.
Curate context, do not dump context.
Lock outcomes and boundaries, not unnecessary implementation details.
Require evidence, not confidence.
Reject answer-shaped near misses explicitly when material.
Keep portable core independent from harness syntax.
```

# Overall Synthesis Principle

Agent Orchestrator giữ **governance strength** của V1/V2 nhưng delivery form là **field manual + operational toolkit + Prompt Compiler methodology**. Strictness nằm ở invariant/evidence; mechanism và syntax thích ứng theo project/model/harness.
