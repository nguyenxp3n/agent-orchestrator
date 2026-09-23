# Chương 5: Quy trình khám nghiệm độc lập Zero-Trust Forensic Audit

## 5.1 Mục tiêu

Audit trả lời một câu: **candidate hiện tại có thực sự thỏa Work Package contract không?** Nó không hỏi worker có tự tin hay test của worker “hình như” pass. Audit phải bind vào exact base/head identity và sử dụng evidence fresh.

## 5.2 Bước 1: Git identity và commit

```bash
git branch --show-current
git rev-parse HEAD
git log -1 --stat --oneline
```

Ghi branch, HEAD SHA, base SHA. Kiểm commit message theo convention của project nếu có. Nếu candidate SHA khác SHA worker report, report worker đã stale và audit trên actual candidate hoặc yêu cầu clarification.

## 5.3 Bước 2: Workspace hygiene

```bash
git status --short
git ls-files --others --exclude-standard
```

Kiểm untracked files, generated junk, local credentials, dirty changes sau commit. Working tree dirty không phải lúc nào cũng auto-fail, nhưng phải giải thích và bind đúng candidate; với release candidate thường fail closed.

## 5.4 Bước 3: Diff boundary và destructive changes

```bash
git diff <base>...HEAD --name-status
git diff <base>...HEAD --stat
git diff <base>...HEAD
```

Đối chiếu từng path với `allowed_paths`, `readonly_paths`, `forbidden_paths`. Kiểm rename/delete riêng. Sau đó rà semantic resources: migration number, route, table, env var, port, event name.

Out-of-scope write là `SCOPE_VIOLATION`; không được bỏ qua vì code “hữu ích”.

## 5.5 Bước 4: Unit/target tests độc lập

Dùng command từ Project Execution Profile, không đoán. Ví dụ:

```bash
<project-specific targeted test command discovered during intake>
pnpm test -- review
pytest -q tests/review
cargo test review
```

Ưu tiên tắt cache khi framework/toolchain cho phép để tránh evidence stale. Ghi command, exit code, số tests và failure summary.

## 5.6 Bước 5: Repo-wide quality gate

Chạy gate canonical của project:

```bash
task qa
# hoặc make test / pnpm check / cargo test / go test ./... / pytest / dotnet test
```

Command trong tài liệu chỉ là ví dụ. Auditor phải chạy **quality gate mà project coi là canonical**, đồng thời tách baseline failure có từ trước khỏi candidate regression. Không gán lỗi cũ cho worker và cũng không che failure.

## 5.7 Bước 6: Barem / Expected Outputs

Đây là lớp mà test xanh thường không thay thế được. Đếm expected files/artifacts theo contract.

Ví dụ project-neutral:

```text
Required: <domain-implementation-artifacts>
Required: <client/consumer/integration-artifacts>
Actual: first group exists, second required group missing
Disposition: REWORK
```

Kiểm migration up/down, API wrapper, component/styles/barrel export, docs, generated artifact, fixtures… theo WP thật.

## 5.8 Bước 7: Contracts, security, regression và audit report

Kiểm frozen API/event/schema contracts; security invariants; secret handling; idempotency/transactions nếu WP yêu cầu; regression risk; downstream consumers.

Audit Report phải có:

```text
WP ID
Agent / generation
Branch / worktree
Base SHA / Candidate SHA
Changed files
Commands + exit codes
Acceptance criterion -> evidence mapping
Findings with severity
Disposition: ACCEPT | REJECT/REWORK | ESCALATE
Residual risks / unknowns
```

## 5.9 Missing evidence = UNKNOWN

Không suy từ “không thấy lỗi” thành PASS. Nếu cloud CI không truy cập được, ghi:

```text
Cloud CI: UNKNOWN
Reason: credential/network unavailable
Local gate: PASS, exit 0
Effect: release/cloud assurance not established
```

Tùy WP, unknown có thể block `ACCEPTED` hoặc chỉ block `RELEASE_VERIFIED`.

## 5.10 Immutable audit identity

Acceptance phải gắn exact candidate SHA/snapshot. Sau audit, nếu worker thêm commit dù “chỉ sửa README”, audit cũ không tự động chuyển sang SHA mới.

```bash
expected=<audited-sha>
actual=$(git rev-parse HEAD)
test "$expected" = "$actual"
```

Khác SHA → `STALE_AUDIT_SHA`, re-audit theo phạm vi phù hợp.

## 5.11 Biên bản REWORK tốt

Không nói “chưa ổn”. Chỉ rõ:

- criterion nào fail;
- evidence nào chứng minh fail;
- file/path nào thiếu hoặc vi phạm;
- worker được phép sửa ở đâu;
- command nào phải chạy lại;
- phần đã pass nào không cần làm lại nếu candidate identity/recovery cho phép.

## 5.12 Audit không phải rewrite

Auditor không sửa code trong cùng task rồi tự ký acceptance. Khi phát hiện lỗi, Auditor tạo corrective work cho Worker. Với integration-only fix do Integrator thực hiện, Auditor phải audit lại candidate mới hoặc áp dụng integration gate đã định nghĩa.

## 5.13 Seven-Step Checklist tóm tắt

```text
1. Git identity/commit
2. git status + untracked hygiene
3. git diff + scope/resource boundaries
4. independent unit/target tests
5. repository quality gate
6. 100% barem/expected outputs
7. contracts/security/regression + signed disposition
```

Bản checklist thao tác nằm tại `checklists/seven-step-forensic-audit.md`.