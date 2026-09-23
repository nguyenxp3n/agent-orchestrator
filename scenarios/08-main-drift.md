# Scenario: Main Drift

## Trigger
Main thay giữa audit và merge.

## Risk
Audit evidence dựa trên baseline cũ, combination mới chưa verified.

## Evidence
Audited base SHA; current main SHA; candidate SHA.

## Immediate Action
Block automatic promotion; rebuild/revalidate merge candidate.

## Forbidden Response
Không nói candidate đã audit nên main drift không quan trọng.

## Recovery Procedure
Rebase/merge current main theo policy, tạo identity mới nếu cần, rerun gates.

## Exit Criteria
Resulting candidate/integration evidence fresh.

## Example Lead Response
```text
Main nhận security patch sau audit WP; Integrator revalidates with patch.
```
