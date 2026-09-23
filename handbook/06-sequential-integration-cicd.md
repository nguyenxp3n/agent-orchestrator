# Chương 6: Tích hợp tuần tự và CI/CD đám mây

## 6.1 Tại sao Sequential Integration

Nhiều WPs pass riêng không chứng minh chúng pass cùng nhau. Merge đồng loạt làm mất khả năng xác định candidate nào gây regression và dễ phá thứ tự migration/contract. Sequential integration giữ causal evidence.

```text
ACCEPTED WP-A -> merge -> global gate
ACCEPTED WP-B -> merge -> global gate
ACCEPTED WP-C -> merge -> global gate
```

Có thể batch khi WPs hoàn toàn độc lập và project policy cho phép, nhưng “merge tất cả vì đều xanh” không phải default.

## 6.2 Integration order theo DAG

Không merge theo thời gian worker báo xong. Nếu WP-B phụ thuộc contract/data của WP-A, A phải integrate trước ngay cả khi B `ACCEPTED` sớm hơn.

```text
WP-CONTRACT -> WP-BACKEND -> WP-E2E
            -> WP-FRONTEND -> WP-E2E
```

Integration queue lưu dependency readiness và candidate audit identity.

## 6.3 Pre-merge gate

Trước merge:

```bash
git fetch --all --prune
git status --short
git rev-parse HEAD
# compare candidate SHA with audited SHA
# compare current main with integration baseline
```

Nếu main drift từ baseline sau audit, đánh giá impact: rebase/merge-main vào candidate sẽ tạo candidate identity mới và thường cần revalidation.

## 6.4 Merge `--no-ff`

Khi project policy muốn giữ lịch sử WP branch:

```bash
git switch main
git pull --ff-only
git merge --no-ff feat/wp-210 -m "merge: integrate WP-210 review backend"
```

`--no-ff` không phải invariant tuyệt đối; repository có thể dùng squash/rebase merge. Invariant là integration phải truy vết được WP, audit evidence và candidate identity. Nếu repo policy bắt squash, ghi mapping commit cũ → merge commit.

## 6.5 Shared hotspot resolution

Integrator có quyền hạn hẹp để áp dụng approved Integration Requests vào router/bootstrap/root config. Không dùng quyền này để refactor domain code tiện thể.

Nếu conflict đơn thuần do text nhưng semantics rõ từ frozen contract, Integrator resolve và verify. Nếu semantics mâu thuẫn, tạo DR.

## 6.6 Cross-WP Gate

Sau mỗi merge hoặc safe batch, chạy:

- canonical build/lint/typecheck/test;
- contract/schema/event validation;
- migration ordering + upgrade/downgrade nếu project yêu cầu;
- critical E2E journeys;
- smoke/startup checks;
- security checks áp dụng.

```bash
<project-global-quality-command>
<contract-validation-command>
<critical-e2e-command>
```

Independent WP success không phải project success.

## 6.7 Pull Request workflow

Nếu dùng GitHub/GitLab/Bitbucket, PR/MR nên chứa:

```text
WP ID + objective
Dependencies
Candidate SHA
Audit disposition + evidence location
Changed paths/resources
Verification commands + results
Integration requests
Known risks/waivers
```

PR là audit surface, không phải bằng chứng thay cho local/CI checks.

## 6.8 Cloud CI/CD verification

Không tin câu “GitHub Actions xanh” nếu có thể kiểm trực tiếp. Ví dụ GitHub CLI:

```bash
gh run list --branch <branch> --limit 10
gh run view <run-id>
```

GitLab/Bitbucket dùng API/CLI tương ứng. Ghi run ID, commit SHA, jobs và conclusion. Đảm bảo CI run thực sự thuộc candidate đang merge.

Một case thực tế dùng đúng pattern này để xác minh cloud jobs trực tiếp thay vì dựa vào worker report. Provider cụ thể không thay đổi invariant: run phải bind với đúng candidate identity.

## 6.9 Cloud runner issues

Các lỗi phổ biến:

- cache chứa dependency cũ;
- wrapper/shim khác local;
- case-sensitive filesystem khác;
- missing secret/permission;
- service container chưa ready;
- network/rate limit;
- matrix job chỉ fail một platform.

Xử lý theo lớp:

```text
candidate bug?
environment/tooling bug?
CI config bug?
stale cache?
external outage?
```

Không “fix code” cho lỗi hạ tầng chưa được phân loại.

## 6.10 Main drift

Nếu main thay đổi giữa audit và integration:

```text
AUDITED CANDIDATE + OLD MAIN
            X
CURRENT MAIN differs
```

Lead phải recompute merge candidate và chạy gate phù hợp. Không dùng audit cũ như bằng chứng cho tổ hợp mới.

## 6.11 Migration integration

Allocated resource slots giải quyết duplicate identifiers nhưng integration vẫn phải kiểm dependency semantics. Với DB project, migration N+1 có thể phụ thuộc schema do migration N tạo; với event/proto/CLI project, resource R2 có thể phụ thuộc contract R1. DAG phải phản ánh dependency; identifier riêng không thay thế semantic ordering.

## 6.12 Promotion và release evidence

Sau final integration, record:

```text
main/release SHA
included WP IDs
included audit IDs/reports
global QA commands + exit codes
CI run IDs
migration/schema result
known waivers
unverified external environments
```

Chỉ khi release-specific evidence tồn tại mới dùng trạng thái `RELEASE_VERIFIED`. Local green không tự động bằng production verified.