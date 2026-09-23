# 02: Prompt Compiler Protocol

## Mục tiêu

**Prompt Compiler** biến Project Truth + Work Package + Ownership/Resource State thành prompt sẵn sàng dispatch. Đây là protocol tài liệu, không phải runtime engine.

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

Xác định:

- target role;
- objective;
- desired artifact;
- success predicate;
- inputs và source-of-truth;
- output format;
- known failure cases;
- required autonomy level.

Nếu chưa thể viết success predicate, task chưa đủ chín để dispatch. Quay lại decomposition hoặc Decision Request.

## Stage 2: Resolve Authority

Phân loại input:

```text
AUTHORITATIVE
VERIFY_BEFORE_USE
UNTRUSTED_DATA
```

Nếu hai authoritative sources mâu thuẫn trên cùng subject, không chọn theo cảm tính; tạo DR.

## Stage 3: Select Context

Nạp context theo nhu cầu:

1. project rules/source index;
2. relevant spec/architecture fragment;
3. relevant files/interfaces/tests;
4. current errors/evidence;
5. conversation state chỉ khi còn tác dụng.

Không broadcast toàn repo, toàn spec hoặc transcript của các agent khác nếu không cần.

## Stage 4: Bind Permissions & Resources

Compile rõ:

- `allowed_paths`;
- `readonly_paths`;
- `forbidden_paths`;
- semantic resource allocations;
- frozen contracts;
- shared hotspots;
- workspace/branch/base identity nếu applicable.

## Stage 5: Define Completion

Viết:

- Success Predicate;
- expected outputs;
- acceptance criteria;
- Non-Counting Outcomes: các near miss không được tính là complete.

Với task đơn giản, non-counting có thể chỉ 1–2 mục. Với task dài/rủi ro, phải chi tiết hơn.

## Stage 6: Bind Evidence

Mỗi criterion quan trọng phải có cách verify:

```text
Criterion -> Evidence source -> Command/check -> Expected signal
```

Nếu verifier cần external service hoặc cloud CI, ghi rõ identity cần đối chiếu như SHA/run ID.

## Stage 7: Bind Escalation

Stop/Request khi:

- scope phải mở rộng;
- resource chưa được cấp;
- spec conflict;
- security/secret decision;
- destructive/irreversible action;
- frozen contract phải đổi;
- candidate/base identity stale.

## Stage 8: Choose Prompt Mode

### Compact Mode

Dùng cho task nhỏ, deterministic, một owner, ít context. Giữ role + objective + scope + expected output + verification + output contract.

### Standard Mode

Mặc định cho coding Work Package. Dùng đầy đủ agent-grade contract.

### Long-Horizon Mode

Dùng khi task đắt, kéo dài, open-ended hoặc orchestration nhiều worker. Bổ sung definitions, non-counting outcomes chi tiết, adversarial failure modes, evidence-ledger/return gate và contamination rules.

## Stage 9: Render

Viết prompt theo imperative, section rõ, ưu tiên action/outcome. Không gắn prompt canonical với tên tool riêng của Claude/Codex/Copilot nếu task không thực sự cần.

## Stage 10: Red-Team

Trước dispatch task đắt hoặc rủi ro, hỏi:

> Agent có thể thỏa chữ nghĩa của prompt nhưng vi phạm ý định bằng cách nào?

Kiểm các loophole:

- narrowed scope;
- artifact thiếu tầng;
- test xanh nhưng không đúng candidate;
- dùng file/resource ngoài scope;
- trả kế hoạch thay vì implementation;
- dùng unverified assumption;
- tự tuyên bố PASS.

Patch prompt bằng success/non-counting/evidence, không bằng cách chất thêm hàng loạt MUST/NEVER vô nghĩa.

## Stage 11: Prompt Quality Gate

Chạy `prompt-engineering/07-prompt-quality-gate.md`. Nếu critical field thiếu, trạng thái là `NOT_READY`.
