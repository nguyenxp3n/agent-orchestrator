# Playbook: Environment Without Docker / Low RAM

## Trigger
Project docs nhắc container nhưng execution environment không có Docker hoặc không đủ resource.

## Adaptation
Xác định mục tiêu cần bảo toàn: isolation, DB/service semantics, reproducibility. Chọn local service riêng, in-memory/fake cho unit tests, remote disposable dependency, separate clone hoặc giảm concurrency.

```text
If isolation cannot be proven -> reduce parallel writers.
If production-specific behavior cannot be reproduced -> mark that evidence UNKNOWN and block the assurance level that requires it.
```

## Exit Criteria
Mechanism thay thế được ghi trong Project Execution Profile; tests phù hợp chạy; limitations không bị che.
