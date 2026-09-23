# Case Study: Project-Neutral Six-Agent Parallel Delivery

## 1. Mục đích

Case study này **không mô tả một dự án bắt buộc**. Nó là một mô hình trung tính để minh họa cách Lead Orchestrator điều phối sáu agents trên bất kỳ software project nào có công việc song song, shared resources và integration hotspots.

Các nhãn `Domain A`, `Domain B`, `Domain C`, `UI/Client`, `Infrastructure`, `CI/CD` chỉ là vai trò minh họa. Khi áp dụng thực tế, Lead phải thay chúng bằng domain thật được discover từ project input.

Ví dụ ánh xạ:

| Project type | Domain A/B/C có thể là | UI/Client có thể là | Shared hotspot thường gặp |
|---|---|---|---|
| Web Fullstack | auth, billing, search | web feature/module | router, DB migrations, OpenAPI |
| Microservices Backend | service A/B/C | consumer/client SDK | proto/schema, event contract, gateway |
| Mobile App | sync, profile, offline cache | screens/features | navigation, shared model, API contract |
| Distributed System | scheduler, worker, storage | admin/control client | protocol/schema, leader state, ports |
| CLI Tool | parser, command groups, config | CLI UX/help | root command registry, config schema |

## 2. Project input là source of truth

Lead không bắt đầu từ tên case study. Lead bắt đầu từ project input thực tế:

```text
architecture docs / docs-equivalent
specifications / spec-equivalent
implementation plan / plan-equivalent
workflow / engineering process
repository tree
build + test manifests
CI configuration
DB/schema/migration model nếu có
environment constraints
```

Tên thư mục `docs/`, `spec/`, `plan/`, `workflow/` chỉ là ví dụ phổ biến. Nếu dự án dùng ADRs, RFCs, tickets, Makefile, Cargo workspace, Xcode project, Gradle, Bazel, Helm, Terraform hay cấu trúc khác, Lead phải discover và compile chúng vào Project Execution Profile.

## 3. Mô hình sáu agents khái quát

```text
Lead Orchestrator
  |
  +-- A1 Domain A          [Resource Slot R1]
  +-- A2 Domain B          [Resource Slot R2]
  +-- A3 Domain C          [Resource Slot R3]
  +-- A4 UI / Client       [Resource Slot R4 or contract consumer]
  +-- A5 Infrastructure    [isolated infra ownership]
  +-- A6 CI/CD             [workflow ownership]

All workers -> Completion Claim -> Independent Audit
Accepted queue -> DAG-aware Sequential Integration -> Global QA -> Cloud/External CI
```

`Resource Slot` có thể là migration ID, port, queue name, schema namespace, event name, CLI command namespace, feature flag hoặc resource semantic khác. Dự án không có migration thì không tạo migration concept giả tạo.

## 4. Incident A: Worker báo xong nhưng thiếu một tầng bắt buộc

Giả sử WP-D yêu cầu:

```text
backend/service implementation
client/UI integration
contract adaptation
unit tests
```

Worker báo backend và tests đã xanh nhưng client/UI artifact chưa tồn tại. Auditor phải kết luận:

```text
Implemented subset: PASS
Tests for implemented subset: PASS
Required client/UI outputs: MISSING
WP: REJECT_REWORK / INCOMPLETE
```

Nếu dự án là CLI thì “client/UI” có thể tương ứng root command registration/help output; nếu là microservices thì có thể là consumer contract hoặc integration adapter. Nguyên tắc là **Atomic Completion**, không phải một file path cụ thể.

## 5. Incident B: Worker xin lấn shared resource

Giả sử Infrastructure Worker phát hiện cần thay đổi shared schema/gateway/root bootstrap ngoài ownership.

Lead từ chối direct write vì:

- resource đã thuộc WP khác hoặc `INTEGRATION_ONLY`;
- shared hotspot có blast radius liên-WP;
- worker không sở hữu domain contract đó.

Giải pháp: giữ implementation trong allowed scope, gửi `Integration Request`, `Resource Request`, hoặc tạo corrective WP nếu dependency thật sự cần thay đổi.

Bài học: **từ chối scope expansion sớm rẻ hơn xử lý collision muộn**.

## 6. Incident C: Scope extension có kiểm soát

Không phải mọi request ngoài scope đều bị bác. Nếu Worker chứng minh một shared build/task configuration cần thay đổi để expose deliverable, Lead có thể grant **đúng file/resource nhỏ nhất**, kèm invariants và quality gates.

Ví dụ trung tính:

```text
Granted: <shared-build-or-task-config>
Conditions:
- preserve existing commands/tasks
- project quality gate remains green
- spec/contract validation remains green when applicable
```

Framework không ép `Taskfile.yml`; dự án có thể dùng `Makefile`, `package.json`, `Cargo.toml`, `build.gradle`, `justfile`, Bazel, Xcode settings hoặc công cụ khác.

## 7. Incident D: Xác minh External CI trực tiếp

Worker báo cloud CI thành công chỉ là claim. Lead/Auditor phải kiểm run gắn đúng candidate SHA bằng provider phù hợp.

Ví dụ GitHub:

```bash
gh run list --branch <branch> --limit 10
gh run view <run-id>
```

GitLab/Bitbucket/Jenkins/Buildkite hoặc hệ thống khác dùng API/CLI tương ứng. Nếu không có quyền truy cập, trạng thái là `UNVERIFIED_EXTERNAL_CI`, không tự nâng thành PASS.

## 8. Sequential Integration

Sau independent acceptance, candidates không được merge đồng loạt. Lead/Integrator chọn order theo DAG/resource dependencies, integrate từng candidate và chạy cross-WP/global verification sau mỗi bước.

```text
ACCEPTED WP-A
   -> integrate
   -> global gate
ACCEPTED WP-B
   -> integrate
   -> global gate
...
```

## 9. Các nguyên tắc rút ra

1. Project input thực tế quyết định decomposition, không phải case study.
2. Lead phải map expected outputs theo project type và WP thật.
3. Semantic resource phải cấp phát trước khi có concurrent writers.
4. Shared hotspots không giao đồng thời cho nhiều workers.
5. Lead phải chặn scope expansion không được phê duyệt.
6. Scope extension hợp lệ phải bounded, explicit và có extra verification.
7. External CI/status cần evidence trực tiếp hoặc ghi `UNVERIFIED`.
8. `WORKER_DONE != ACCEPTED`.
9. Integration order theo dependency, không theo tốc độ worker.
10. Framework không giả định Web, DB, Docker, migration hay bất kỳ toolchain cụ thể nào.
