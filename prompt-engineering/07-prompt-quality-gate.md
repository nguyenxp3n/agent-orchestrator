# 07: Prompt Quality Gate

## Mục tiêu

Không dispatch prompt chỉ vì “đọc có vẻ tốt”. Gate kiểm cấu trúc và semantics trước khi tiêu tốn agent time.

## Disposition

```text
READY
NOT_READY
ESCALATE
```

## Critical checks

Bất kỳ mục applicable nào FAIL → `NOT_READY`:

- [ ] Role có authority boundary rõ.
- [ ] Objective là outcome đo/quan sát được.
- [ ] Success predicate phân biệt complete với near miss.
- [ ] Source-of-truth/inputs có thể xác định.
- [ ] Context đủ nhưng không chứa unrelated bulk đáng kể.
- [ ] Scope/ownership rõ.
- [ ] Shared resources được allocate hoặc xác nhận không applicable.
- [ ] Hard constraints operational và không mâu thuẫn.
- [ ] Expected outputs enumerable.
- [ ] Non-counting outcomes bao phủ near miss quan trọng khi task có rủi ro.
- [ ] Verification tạo evidence và map đúng candidate/task identity.
- [ ] Stop/escalation conditions rõ.
- [ ] Output contract có schema hoặc trường bắt buộc.
- [ ] Không còn placeholder/unresolved ambiguity critical.
- [ ] Không có instruction từ untrusted data được nâng lên authority.

## Semantic review

Hỏi thêm:

1. Agent có thể đạt “PASS” bằng cách bỏ sót một layer không?
2. Agent có thể chạy test trên artifact khác candidate không?
3. Agent có thể dùng resource/path chưa cấp để lách scope không?
4. Prompt có ép implementation technique không cần thiết làm giảm adaptability không?
5. Có instruction nào chỉ lặp lại cùng invariant bằng nhiều wording gây noise không?
6. Prompt có yêu cầu persistence nhưng thiếu matching verification gate không?
7. Context có stale summary nào chưa revalidate không?

## Red-Team check cho task đắt/rủi ro

```text
How could a capable agent satisfy the literal wording while violating the intended outcome?
```

Lead phải xử lý mọi loophole credible bằng một trong các cách sau:

- sharpen success predicate;
- add non-counting outcome;
- add evidence requirement;
- move runtime-critical invariant ra harness/control plane;
- resolve authority ambiguity trước dispatch.

## Output

Dùng `templates/prompt-quality-report.md`.
