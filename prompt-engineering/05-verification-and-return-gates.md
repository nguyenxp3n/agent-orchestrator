# 05: Verification, Non-Counting Outcomes & Return Gates

## Verification là phần của prompt, không phải hậu kiểm tùy chọn

Mỗi prompt agent-grade phải nói rõ điều gì chứng minh kết quả.

```text
Claim -> Evidence -> Identity -> Criterion
```

Ví dụ:

```text
Claim: tests pass
Evidence: command + exit code + relevant summary
Identity: candidate SHA / workspace generation
Criterion: AC-03
```

## Success Predicate

Viết một điều kiện observable cho complete. Tránh “quality cao”, “đầy đủ”, “ổn định” nếu không có operational definition.

## Non-Counting Outcomes

**Non-Counting Outcomes** là những artifact trông giống hoàn thành nhưng không đạt intent.

Ví dụ software WP:

- chỉ làm một layer trong khi acceptance yêu cầu nhiều layer;
- chỉ thêm tests mà không implement behavior;
- build xanh nhờ bỏ/skip test;
- dùng assumption chưa verify;
- trả design/proposal thay vì code artifact;
- scope bị thu hẹp âm thầm;
- verification chạy trên SHA khác candidate;
- external CI xanh nhưng không thuộc commit cần audit.

## Failure-Mode Checklist cho Auditor

Generic “review kỹ” là yếu. Với task rủi ro, compile một danh sách các failure mode domain-specific để Auditor chủ động săn.

```text
AUDIT_FAILURE_MODES:
- duplicate side effect under retry
- migration ordering collision
- backward-incompatible contract change
- untracked generated artifact
- secret/log leakage
```

## Return Gate

Return condition phải là predicate trên artifact/evidence:

```text
Return WORKER_COMPLETE_CLAIM only when all required expected outputs exist and all worker-owned verification steps have current-session evidence.
```

Không dùng confidence làm gate:

```text
"Khi bạn cảm thấy chắc chắn thì báo xong"  # không hợp lệ
```

## Persistence Rule

Persistence chỉ dùng cho task dài khi đi kèm verification tương xứng. “Đừng dừng cho đến khi xong” mà không có success predicate tạo động lực reward-hacking/answer-shaped near miss.

## Fresh-context Verification

Khi có thể, Auditor nên dùng fresh context và candidate identity rõ ràng. Người tạo artifact dễ rationalize chính gap của mình; independent audit giảm rủi ro đó.

## Evidence freshness

Evidence stale khi:

- candidate thay sau audit;
- base/main drift thay assumptions;
- generated artifact được tái tạo;
- external dependency/contract đổi;
- command chạy ở workspace khác.

Stale evidence không tự động được carry forward.
