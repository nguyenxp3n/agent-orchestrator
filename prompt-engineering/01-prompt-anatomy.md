# 01: Prompt Anatomy: từ Prompt 5 phần đến Agent Execution Contract

## 1. Công thức nền

Một prompt thông thường có thể bắt đầu từ năm thành phần:

```text
ROLE + CONTEXT + TASK + FORMAT + CONSTRAINTS
```

- Role: agent đang đóng vai trò gì và có thẩm quyền đến đâu.
- Context: sự thật nền cần thiết để hiểu nhiệm vụ.
- Task: kết quả phải tạo ra.
- Format: hình dạng đầu ra.
- Constraints: giới hạn không được vượt qua.

Công thức này tốt cho tác vụ ngắn. Với coding agent hoặc multi-agent orchestration, nó chưa đủ để chống scope drift, false completion và resource collision.

## 2. Agent-grade contract

Agent Orchestrator mở rộng thành:

```text
ROLE
+ OBJECTIVE
+ SUCCESS PREDICATE
+ CONTEXT
+ INPUTS / SOURCE OF TRUTH
+ SCOPE & OWNERSHIP
+ RESOURCES
+ CONSTRAINTS
+ EXPECTED OUTPUTS
+ NON-COUNTING OUTCOMES
+ VERIFICATION
+ STOP / ESCALATION CONDITIONS
+ OUTPUT CONTRACT
```

### Role

Role phải xác định **trách nhiệm và ranh giới quyền hạn**, không chỉ persona trang trí.

Yếu:

```text
Bạn là senior developer giỏi.
```

Mạnh:

```text
Bạn là Scoped Coding Worker của WP-210. Bạn chỉ triển khai artifact trong ownership envelope đã cấp; không tự merge, không tự mở scope, không tự tuyên bố ACCEPTED.
```

### Objective

Mô tả outcome, không mô tả hoạt động mơ hồ.

```text
OBJECTIVE: tạo endpoint idempotent đáp ứng contract đã frozen và có regression test chứng minh duplicate request không tạo record thứ hai.
```

### Success Predicate

**Success Predicate** là điều kiện có thể kiểm chứng để phân biệt DONE thật với “có vẻ xong”.

```text
SUCCESS_PREDICATE:
- expected artifact tồn tại;
- contract tests pass;
- duplicate replay test pass;
- diff không vượt allowed_paths;
- không dùng resource ngoài allocation.
```

### Non-Counting Outcomes

Liệt kê các near-miss dễ bị agent trả thay cho kết quả thật.

```text
NON_COUNTING_OUTCOMES:
- chỉ viết backend trong khi WP yêu cầu client artifact;
- test unit pass nhưng canonical quality gate chưa chạy;
- mô tả giải pháp thay vì tạo artifact;
- “blocked by spec” khi spec thực tế đã resolve;
- dùng workaround ngoài ownership để làm test xanh.
```

### Verification

Verification phải tạo evidence:

```text
command -> exit code -> artifact/log -> criterion
```

Không chấp nhận:

```text
"Tôi đã kiểm tra và mọi thứ ổn."
```

## 3. Prompt altitude

Prompt quá thấp cấp sẽ hard-code từng thao tác và làm agent mất khả năng thích nghi. Prompt quá cao cấp lại mơ hồ.

Framework dùng **heuristic altitude**:

- khóa outcome, invariants, boundaries và evidence;
- chỉ định procedure ở những bước có rủi ro;
- để implementation detail cho Worker trong allowed envelope.

## 4. Positive instructions trước, prohibitions sau

Ưu tiên chỉ dẫn agent **phải làm gì**. Dùng `FORBIDDEN`/`DO NOT` cho invariants thật sự quan trọng như protected branch, secrets, destructive action, forbidden paths.

## 5. Cấu trúc theo mức phức tạp

- Tác vụ nhỏ: Markdown headers ngắn là đủ.
- Tác vụ phức tạp: chia section rõ `OBJECTIVE`, `CONTEXT`, `BOUNDARIES`, `VERIFICATION`, `OUTPUT`.
- Nếu nhiều lớp dữ liệu dễ lẫn, có thể dùng XML-like tags, nhưng đây là lựa chọn trình bày, không phải invariant của framework.

## 6. Quy tắc cuối

Prompt không cần dài nhất. Prompt cần **đủ tín hiệu để agent không phải đoán những điều có thể làm sai kết quả**.
