# Scenario: Safe Parallel Work

## Trigger
Hai WPs độc lập cùng ready, không overlap path/resource.

## Risk
Lãng phí concurrency nếu serialize vô cớ, hoặc collision nếu giả định độc lập sai.

## Evidence
Ownership Matrix; resource registry; hard dependencies; frozen contracts.

## Immediate Action
Schedule cùng parallel wave sau readiness gate.

## Forbidden Response
Không broadcast quyền rộng chỉ vì hai tasks “có vẻ khác nhau”.

## Recovery Procedure
Nếu phát hiện overlap giữa chừng, pause WP bị ảnh hưởng và re-plan ownership.

## Exit Criteria
Cả hai candidates audit độc lập và không tạo cross-WP regression.

## Example Lead Response
```text
WP-A và WP-B không chia sẻ exclusive path/resource; dispatch song song trong worktrees riêng.
```
