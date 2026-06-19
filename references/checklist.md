# Production Optimization + Security Checklist

Use this checklist on every optimization task.

## 1) Scope and objective
- Define target endpoint/job/component and user impact.
- Capture SLO-linked goal (example: p95 latency from 420 ms to <= 300 ms).
- Define non-functional constraints: security, correctness, compatibility.

## 2) Baseline evidence
- Record baseline metrics before changes:
  - p50/p95/p99 latency
  - throughput
  - error rate
  - CPU and memory
  - DB query count and slow query list (if relevant)
- Ensure test/load data resembles production shapes and cardinality.

## 3) Hotspot and complexity analysis
- Identify top bottlenecks with profiler/trace/query plans.
- Annotate hot paths with current complexity:
  - time complexity
  - space complexity
- Flag clear reductions (example: O(n^2) to O(n log n)).

## 4) Optimization design quality gates
- Prefer algorithm/data-structure improvements before micro-tuning.
- Remove N+1 calls and redundant round trips.
- Use bounded concurrency and backpressure.
- For caching: define key strategy, TTL, invalidation, and stale behavior.
- Document tradeoffs explicitly (latency vs memory, cost vs complexity).

## 5) Security guardrails
- No unsafe dynamic eval/exec introduced.
- Parameterized queries/commands maintained.
- Authorization checks unchanged for optimized path.
- No sensitive data leakage via logs/traces/metrics/cache.
- SSRF/network constraints (allowlist, protocol rules) preserved.
- Session/authn/authz semantics unchanged.
- Dependency additions pass supply-chain policy and review.

## 6) Correctness and regression validation
- Run/define unit and integration tests for changed logic.
- Add regression tests for discovered edge cases.
- Verify failure-mode behavior (timeouts, partial failures, retries).
- Re-run security checks relevant to touched code paths.

## 7) Performance validation
- Run comparable before/after workload.
- Report absolute metrics and percentage deltas.
- Include variance notes and test conditions.
- Confirm no unacceptable regressions in non-target dimensions.

## 8) Production rollout and safety
- Use feature flag/canary when possible.
- Set explicit abort thresholds (errors, saturation, latency).
- Define rollback command/path before release.
- Monitor golden signals after rollout:
  - latency
  - traffic
  - errors
  - saturation

## 9) Final decision template
- Change summary:
- Measured gain:
- Security impact:
- Residual risks:
- Rollout plan:
- Rollback plan:
