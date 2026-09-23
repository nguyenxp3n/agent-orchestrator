# Example: Project 10 Agents

## Nguyên tắc

Mười agents chỉ được dùng khi có đủ boundaries độc lập. Không chia giả tạo chỉ để đạt con số 10.

## Team

```text
A1 Contract/API
A2 Domain Backend A
A3 Domain Backend B
A4 Frontend A
A5 Frontend B
A6 Database/Migrations
A7 Infrastructure
A8 Test/E2E
A9 Security/Review
A10 CI/Integration Support
Lead Orchestrator separate from workers
```

## Safe Waves

Wave 0: A1 freeze public contracts; A6 reserve migration plan.

Wave 1: A2/A3/A4/A5/A7/A10 chạy song song với non-overlapping paths/resources. A8 chuẩn bị fixtures/read-only tests nếu không sửa hotspots. A9 review architecture/security without candidate mutation.

Wave 2: auditors verify each candidate; rework loops remain on original ownership.

Wave 3: Integrator merges accepted WPs by DAG and runs cross-WP gates.

## Resource Registry Example

```text
MIG-31 -> Domain A
MIG-32 -> Domain B
PORT-DEV-A -> 8121
PORT-DEV-B -> 8122
ROUTE-/a/* -> Contract A
ROUTE-/b/* -> Contract B
ROOT-ROUTER -> INTEGRATION_ONLY
```

## Concurrency Cap

Nếu local machine chỉ chạy được 4 heavy services, schedule 10 logical agents nhưng tối đa 4 concurrent execution slots. Framework tối ưu safe throughput chứ không ép tất cả cùng active.