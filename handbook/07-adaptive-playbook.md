# Chương 7: Ứng biến linh hoạt và xử lý tình huống biên

## 7.1 Nguyên tắc ứng biến

Framework không được biến thành checklist cứng đến mức phá project. Lead giữ invariants nhưng thay mechanism theo evidence thực tế.

```text
Invariant stays fixed -> mechanism may change
Evidence requirement  -> command/source adapts
Ownership discipline  -> repository structure adapts
Isolation             -> worktree/clone/container/remote adapts
```

## 7.2 Không có Docker hoặc máy thiếu RAM

Không ép Docker. Xác định mục tiêu Docker đáng lẽ bảo vệ: dependency isolation, service reproducibility, DB/queue availability.

Thay thế có thể là:

- SQLite/in-memory DB nếu semantics đủ cho unit tests;
- local Postgres instance với database/schema riêng cho từng worker;
- remote disposable service;
- mocks/fakes cho external dependency trong unit tests;
- sequentialize tasks nếu không thể cô lập resource.

```text
No Docker -> reduce concurrency if isolation cannot be proven.
```

Không hy sinh correctness chỉ để giữ đủ 10 agents chạy.

## 7.3 Agent timeout/crash

Không xóa worktree ngay. Freeze và capture:

```bash
git status --short
git diff
git log -3 --oneline
```

Record leases, pending DRs, last safe commit, warnings. Phân loại clean/dirty/corrupted/unsafe. Tạo Recovery Bundle và tăng assignment generation. Agent cũ quay lại là zombie; output generation cũ bị reject.

## 7.4 Spec mâu thuẫn với repository

Không chọn ngẫu nhiên “code là truth” hay “spec là truth”. Xác định subject authority. Ví dụ build command trong README cũ có thể thua CI config hiện tại; API wire shape có thể do frozen OpenAPI làm authority dù implementation đang lệch.

Nếu equal-authority conflict:

```text
STATUS: AWAITING_DECISION
Fact A: ... evidence path
Fact B: ... evidence path
Impact: ...
Options: ...
Recommendation: ...
```

## 7.5 Git conflict giữa branches

Phân loại conflict:

1. Textual only, semantics thống nhất → Integrator resolve.
2. Shared hotspot có Integration Request → apply request theo frozen contract.
3. Contract semantics khác nhau → DR.
4. Hai WPs cùng sửa vùng lẽ ra exclusive → planning violation; không che bằng manual merge.

```bash
git diff --ours -- <file>
git diff --theirs -- <file>
```

Resolve dựa trên source of truth và contracts, không dựa trên “branch mới hơn”.

## 7.6 Worker xin mở rộng scope

Dùng playbook `scope-expansion.md`. Nguyên tắc: mặc định không mở rộng nếu có phương án trong scope. Nếu bắt buộc, extension phải exact paths/resources, reason, invariants, extra verification và expiry/ownership rõ.

## 7.7 Migration collision

Nếu hai branches cùng claim một allocated identifier/resource slot `R2`, dừng integration. Với DB project đó có thể là migration number; với project khác có thể là port, event/schema version, command namespace hoặc resource semantic khác. Không tự renumber/reassign khi chưa kiểm dependency references. Xác định ownership, reassign một WP, cập nhật artifacts/references/tests và re-audit candidate bị thay identity.

## 7.8 CI fail nhưng local pass

Không kết luận “CI flaky” ngay. So commit SHA, runtime versions, env/secrets, service readiness, cache, OS/filesystem differences. Nếu rerun pass mà không có root cause, ghi flaky evidence và policy xử lý; đừng xóa dấu vết.

## 7.9 Contract thay đổi khi downstream đang chạy

Frozen contract bị thay đổi làm downstream context stale. Tạm dừng affected WPs, xác định change compatibility. Nếu breaking, issue new contract version/generation, refresh context và re-run affected verification. Không để worker hoàn tất trên contract cũ rồi merge “sửa sau”.

## 7.10 Main drift sau audit

Audit candidate dựa trên base/main cụ thể. Main drift có thể tạo conflict hoặc regression mới. Integrator phải rebuild/revalidate combination. Nếu candidate commit thay, stale audit invalid.

## 7.11 Cross-WP failure

Hai WPs pass riêng nhưng fail chung: không đổ lỗi theo cảm tính. Reconstruct order, contract, resource and integration diff. Block promotion. Tạo corrective WP ở đúng ownership boundary hoặc DR nếu architecture ambiguity.

## 7.12 Harness/tool outage

Nếu coding-agent provider, Git host hoặc CI outage:

- preserve canonical state locally;
- mark affected check `BLOCKED_ENVIRONMENT`/`UNKNOWN`, không `FAIL_IMPLEMENTATION`;
- không consume retry budget cho lỗi external nếu policy phân biệt;
- resume từ checkpoint khi service về.

## 7.13 Safe Mode

Khi canonical coordination state không còn đáng tin: duplicate ownership, registry corrupt, unknown active writer, inconsistent migration slots… dừng dispatch/integration mới.

Safe Mode procedure:

```text
1. Freeze mutations
2. Snapshot Git/workspaces/resources
3. Reconstruct truth from repository + evidence
4. Resolve contradictions
5. Rebuild ownership/resource ledger
6. Revalidate affected candidates
7. Resume with new baseline/generation
```

## 7.14 Chọn ít concurrency hơn là một quyết định đúng

“5–10 agents” là capability, không phải quota. Nếu project chỉ có ba boundaries độc lập, chạy ba agents là đúng. Chia giả tạo 10 agents làm tăng hotspots, coordination cost và hallucination surface.

## 7.15 Decision heuristic cuối cùng

Khi không có playbook phù hợp, Lead dùng thứ tự:

```text
Protect security/irreversible data
Protect source-of-truth/contracts
Protect ownership/resources
Protect audit identity/evidence
Preserve recoverability
Then optimize speed/convenience
```

Nếu vẫn không đủ evidence, trạng thái đúng là `UNKNOWN` hoặc `AWAITING_DECISION`, không phải một đáp án tự tin được bịa ra.