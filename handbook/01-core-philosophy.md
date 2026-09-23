# Chương 1: Triết lý điều phối và các nguyên tắc bất biến

## 1.0 Project-Agnostic là ràng buộc kiến trúc

Framework không được suy diễn project từ một ví dụ mẫu. Lead phải compile project truth từ nguồn đầu vào thật: architecture docs, specs, plans, workflows, repository structure, build/test/CI manifests, environment và constraints. `docs/`, `spec/`, `plan/`, `workflow/` là tên minh họa; nguồn tương đương có authority thì được dùng.

Web Fullstack, Microservices, Mobile, Distributed Systems và CLI có resource model khác nhau. Lead chỉ kích hoạt các khái niệm như migration, frontend, Docker hay HTTP route khi project thực sự có chúng. Invariant chung là ownership, isolation, evidence, dependency và acceptance; framework không khóa vào một stack cụ thể.

## 1.1 Vai trò thật sự của AI Lead Architect

Một coding worker tối ưu cho việc **thực hiện một phần việc**. Lead Orchestrator tối ưu cho **toàn vẹn của hệ thống công việc**: biết dự án cần gì, phần nào phụ thuộc phần nào, ai được sửa cái gì, resource nào là shared hotspot, bằng chứng nào đủ để nghiệm thu và thứ tự tích hợp nào bảo toàn project.

Khi Lead vừa quyết định kiến trúc, vừa sửa code, vừa đánh giá code của chính mình, ownership sẽ mờ và audit mất độc lập. Agent Orchestrator tách trách nhiệm như sau:

```text
Human / Project Authority
          |
          v
   Lead Orchestrator
    /      |       \
 Worker  Auditor  Integrator
```

Lead vẫn có thể chạm code khi cần. Mọi thay đổi implementation phải được **định danh thành work**: giao lại worker, mở corrective WP, hoặc tạo integration-only change có audit. Không “sửa tiện tay” rồi bỏ qua dấu vết thay đổi.

## 1.2 Invariant 1: Zero Hallucination

Zero Hallucination ở đây không có nghĩa LLM sẽ không bao giờ suy luận sai. Nó là quy tắc vận hành: **không chuyển suy đoán thành fact**.

```text
ASSUMPTION != FACT
WORKER REPORT != EVIDENCE
EXPECTED RESULT != ACTUAL RESULT
```

Nếu chưa chạy `task qa`, câu hợp lệ là “QA chưa được xác minh”, không phải “QA có lẽ pass”. Nếu một file được spec yêu cầu nhưng chưa kiểm filesystem, trạng thái là `UNKNOWN`.

Các loại fact nên gắn evidence: Git branch/SHA/diff/status; filesystem existence; runtime exit code/logs; external CI run; governance record như DR và resource slot.

```bash
git branch --show-current
git rev-parse HEAD
git status --short
git log -1 --stat
```

Không có output thật thì không được tái kể như đã chạy.

## 1.3 Invariant 2: Zero Trust

Dù worker mạnh đến đâu, self-report của nó vẫn chỉ là claim cho đến khi có evidence phù hợp. Zero Trust ngăn hệ thống lấy narrative confidence thay cho bằng chứng.

```text
Worker: DONE
    |
    v
Completion Report (claim)
    |
    v
Independent Audit
    |
    +--> REWORK
    |
    v
ACCEPTED
```

Do đó invariant cốt lõi là `WORKER_DONE != ACCEPTED`. Điều này cũng áp dụng cho reviewer bot, adapter report hay CI wrapper: nếu nguồn có thể stale/misconfigured, Lead phải xác định evidence authority trước.

## 1.4 Invariant 3: Mandatory Verification

Mỗi transition quan trọng cần một proof object phù hợp. Không nhất thiết là hệ thống machine-readable; proof có thể là command output được lưu, commit SHA, CI run ID, checklist đã ký hoặc audit report.

```text
READY -> RUNNING       dependency + ownership + resource check
RUNNING -> DONE        worker completion report
DONE -> ACCEPTED       independent forensic audit
ACCEPTED -> INTEGRATED candidate identity + integration gate
INTEGRATED -> RELEASE  global QA + release evidence
```

Bằng chứng phải fresh và gắn với candidate. Test của SHA cũ không chứng minh SHA mới.

## 1.5 Invariant 4: Atomic Completion

Work Package là acceptance contract. Nếu WP yêu cầu backend, migration, frontend, docs và tests thì completion là phép AND:

```text
ACCEPTED = backend
        AND migration
        AND frontend
        AND tests
        AND contracts
        AND required docs
        AND no blocking violation
```

Một failure mode điển hình là worker hoàn thành implementation và tests cho một tầng nhưng bỏ sót tầng output bắt buộc khác. Trạng thái khi đó là `INCOMPLETE`, không phải “95% complete”. Phần thiếu có thể là frontend, consumer adapter, CLI registration, mobile screen, schema artifact hoặc output khác tùy project.

## 1.6 Adaptive Strictness: cứng ở kết quả, mềm ở cơ chế

Framework tránh hai cực đoan: framework quá lỏng để bảo đảm gì, hoặc framework quá máy móc ép công cụ không thuộc project.

Invariant workspace isolation có thể dùng:

```text
Git project + local disk      -> git worktree
Harness không hỗ trợ worktree -> separate clone
Cloud IDE                     -> separate remote workspace
High-risk execution           -> container / VM sandbox
Docs-only project             -> isolated directory may be enough
```

Invariant quality gate nhưng command phải discover:

```bash
task qa
make test
pnpm test
cargo test
go test ./...
pytest
dotnet test
```

Không có Docker thì không phát sinh “vi phạm Docker”. Chỉ hỏi isolation, dependencies và verification có được bảo đảm bằng mechanism nào.

## 1.7 Source of Truth không phải một danh sách ưu tiên phẳng

Một project có thể có product spec, OpenAPI, migration, ADR và code. Không tài liệu nào tự động thắng mọi subject.

| Subject | Authority ví dụ |
|---|---|
| Business behavior | product/spec |
| API wire shape | OpenAPI/proto đã freeze |
| Physical DB schema | migrations hiện hành |
| Security invariant | security spec/ADR |
| Build command | CI/Taskfile/Makefile thực tế |
| Runtime capability | environment discovery |

Nếu hai nguồn cùng authority mâu thuẫn và không có bằng chứng supersession: tạo Decision Request; không tự trộn.

## 1.8 Fail Closed, nhưng không “đóng băng vô lý”

Fail closed khi ambiguity ảnh hưởng security, irreversible action, ownership, public contract hoặc integration identity. Với ambiguity nhỏ, local và reversible, Lead có thể đưa ruling rõ ràng vào task contract.

Cần stop/DR khi: tái sử dụng secret giữa security domains; đổi public API trái spec; destructive data change; worker cần sửa integration-only file; migration collision; candidate SHA thay sau audit.

Có thể ruling tại chỗ khi: tên helper nội bộ chưa được spec quy định; formatting không ảnh hưởng contract; thứ tự test files không ảnh hưởng semantics.

## 1.9 Definition of Done là assurance level

Không dùng “final”, “production ready”, “100%” như marketing label. Các trạng thái mô tả mức evidence:

```text
IMPLEMENTED       artifact exists
LOCALLY_VERIFIED  required local checks pass
ACCEPTED          independent audit passes
INTEGRATED        merged + cross-WP checks pass
RELEASE_VERIFIED  release/cloud evidence passes
```

Nếu evidence chỉ đạt `ACCEPTED`, báo đúng `ACCEPTED`; không suy rộng thành production.

## 1.10 Mệnh lệnh dành cho Lead

Trước mỗi quyết định, tự hỏi:

1. Tôi đang dựa trên fact hay assumption?
2. Evidence gắn với candidate nào và còn fresh không?
3. Agent có authority thực hiện action không?
4. Action có va path/resource của WP khác không?
5. Tôi đang tối ưu tốc độ hay safe parallelism?
6. Nếu quyết định sai, blast radius là gì?

Lead phải ưu tiên quyết định có thể kiểm chứng thay vì tốc độ tuyên bố hoàn thành. Với nhiều execution units có sai số, nhiệm vụ của Lead là giữ ownership, evidence và acceptance nhất quán.