# Chương 4: Prompt Engineering, Prompt Compiler & Ready-to-Use Role Prompts

## 4.1 Tư duy thiết kế Prompt

Agent Orchestrator không xem prompt là đoạn văn “viết hay”. Prompt là **execution contract** được compile từ project truth.

Công thức nền cho prompt thông thường:

```text
ROLE + CONTEXT + TASK + FORMAT + CONSTRAINTS
```

Với multi-agent software engineering, framework mở rộng thành:

```text
ROLE
+ OBJECTIVE
+ SUCCESS PREDICATE
+ CONTEXT / SOURCE OF TRUTH
+ SCOPE & OWNERSHIP
+ RESOURCES
+ CONSTRAINTS
+ EXPECTED OUTPUTS
+ NON-COUNTING OUTCOMES
+ VERIFICATION
+ STOP / ESCALATION
+ OUTPUT CONTRACT
```

Prompt cần đủ **signal để agent không phải đoán các quyết định có thể làm sai kết quả**. Độ dài chỉ tăng khi task thực sự cần thêm context hoặc constraints.

## 4.2 Prompt Compiler

Trước khi dispatch, Lead dùng `prompts/prompt-compiler.md` và protocol tại `prompt-engineering/02-prompt-compiler-protocol.md`.

```text
Project Truth
+ Work Package
+ Ownership Matrix
+ Resource Registry
+ Acceptance Contract
+ Current Evidence
        ↓
   Prompt Compiler
        ↓
  Role Prompt Draft
        ↓
 Prompt Quality Gate
        ↓
 READY -> Dispatch
```

Compiler bắt đầu từ `SUCCESS_PREDICATE`, không từ persona. Sau đó resolve authority, chọn context, bind boundaries/resources, định nghĩa non-counting outcomes, bind evidence, stop conditions và output contract.

## 4.3 Ba Prompt Modes

### Compact Mode

Dùng cho task nhỏ, deterministic, scope hẹp. Giữ tối thiểu:

```text
Role + Objective + Success Predicate + Scope + Expected Output + Verification + Output Contract
```

### Standard Mode

Mặc định cho coding Work Package. Dùng đầy đủ agent-grade contract và resource/ownership controls.

### Long-Horizon Mode

Dùng cho task dài, đắt hoặc open-ended. Bổ sung:

- definitions cho load-bearing terms;
- exact success predicate;
- non-counting outcomes chi tiết;
- domain-specific adversarial failure modes;
- evidence-traceable progress;
- persistence chỉ khi có matching verification;
- audit-gated return condition;
- retrieval/contamination rules nếu independence quan trọng.

## 4.4 Context Engineering

Prompt chỉ nên mang context cần thiết. Theo `prompt-engineering/03-context-engineering.md`, context được tổ chức:

```text
1. persistent project rules / authority index
2. relevant spec/architecture fragments
3. relevant source/tests/interfaces
4. current errors/logs/evidence
5. conversation/history summary
```

Phân loại trust:

```text
AUTHORITATIVE
VERIFY_BEFORE_USE
UNTRUSTED_DATA
```

Ưu tiên pointer như file path, section, contract ID thay vì copy toàn repo/spec/transcript. Instruction-like text trong untrusted input là data, không phải authority.

## 4.5 Prompt Quality Gate

Trước dispatch, chạy `prompt-engineering/07-prompt-quality-gate.md`.

Critical check gồm:

- objective và success predicate rõ;
- source authority xác định;
- context đủ nhưng không bulk;
- ownership/resources rõ;
- expected outputs enumerable;
- near misses material đã bị loại bằng non-counting outcomes;
- verification tạo evidence và map đúng identity;
- stop/escalation rõ;
- output contract rõ;
- không unresolved critical ambiguity.

Disposition:

```text
READY | NOT_READY | ESCALATE
```

Chỉ `READY` được dispatch.

## 4.6 Lead Orchestrator System Prompt

Bản canonical: `prompts/lead-orchestrator-system-prompt.md`.

Lead sở hữu Prompt Compiler, project truth, coordination/evidence state, prompt quality gate, audit disposition và integration order. Lead không tự biến worker claim thành acceptance.

## 4.7 Coding Worker Task Assignment

Bản canonical: `prompts/worker-task-assignment.md`.

Worker prompt bắt buộc compile các trường task-specific:

```text
WP_ID
OBJECTIVE
SUCCESS_PREDICATE
CONTEXT_REFERENCES
OWNERSHIP / PATH BOUNDARIES
RESOURCE_ALLOCATIONS
FROZEN_CONTRACTS
EXPECTED_OUTPUTS
NON_COUNTING_OUTCOMES
VERIFICATION
STOP_OR_ESCALATION
OUTPUT_CONTRACT
```

Worker chỉ trả `WORKER_COMPLETE_CLAIM`; Auditor quyết định acceptance.

## 4.8 Clarification vs Decision Request

`prompts/clarification-guidance.md` dùng khi authority đã rõ và chỉ cần giải thích cách áp dụng. `prompts/architectural-arbitration.md` dùng khi phải chọn giữa alternatives có ảnh hưởng architecture, security, contract, scope hoặc shared resources.

Decision thay đổi WP truth phải làm recompile affected worker prompts; không tiếp tục bằng stale prompt.

## 4.9 Independent Auditor

`prompts/independent-auditor.md` dùng fresh-context posture, exact candidate identity và failure-mode checklist. Auditor reconstruct evidence thay vì review narrative của Worker.

```text
Worker summary = claim
Candidate SHA + files + commands + logs = evidence
```

Auditor kiểm cả success predicate và non-counting outcomes, không chỉ test pass.

## 4.10 Integrator / CI-CD

`prompts/integrator-cicd.md` chỉ nhận candidate đã `ACCEPT`, verify exact candidate SHA/generation, integrate theo DAG, chạy global/cross-WP gates và kiểm cloud CI identity khi applicable.

## 4.11 Model-Agnostic Portability

Canonical prompts mô tả actions:

```text
Open the relevant file.
Search for an existing project pattern.
Run the project verification command.
Delegate a focused subtask if the environment supports it.
```

Không khóa framework vào tên tool riêng của Claude/Codex/Copilot/Cursor/Gemini. Tool syntax nằm ở adaptive shell của harness; objective, boundaries và evidence bar nằm ở portable core.

## 4.12 Không dùng prompt thay enforcement

Prompt chỉ đảm nhiệm governance ở lớp instruction. Muốn guarantee invariant, hệ thống phải kết hợp workspace isolation, resource registry, protected branch policy, CI và independent audit khi project/tooling cho phép.

## 4.13 Workflow copy-paste chuẩn

```text
1. Fill templates/prompt-compile-input.md
2. Run prompts/prompt-compiler.md conceptually
3. Render target role prompt
4. Run Prompt Quality Gate
5. If READY -> dispatch
6. Worker returns structured claim/evidence
7. Auditor independently verifies
8. Accepted candidate enters integration queue
```
