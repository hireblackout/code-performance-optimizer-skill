---
name: code-performance-optimizer-skill
description: >
  Production code optimization with Big-O analysis and security guardrails for web/backend systems.
  Use this skill whenever the user mentions slow APIs, high latency (p95/p99), CPU/memory spikes,
  expensive DB queries, scaling issues, performance regressions, or asks to "optimize/refactor for speed".
  Always apply secure-by-default checks (OWASP Top 10 2025 + ASVS v5 aligned) and deployment safety
  validation so optimizations do not break behavior, reliability, or security posture.
---

# Production Optimizer Guard (Big-O + Security)

## Mission
Improve performance and scalability while preserving correctness, security, and production stability.

## Non-negotiable rules
1. Measure first. Never optimize blindly.
2. Prioritize by impact: user-facing latency/error budget first.
3. Reduce algorithmic complexity before micro-optimizations.
4. Preserve behavior unless user explicitly approves behavior changes.
5. Every optimization includes security impact review.
6. Every optimization includes rollback-safe deployment steps.

## Required workflow

### 1) Intake and target definition
- Capture service context: endpoint/job, traffic profile, SLO/SLI, constraints.
- Define success metrics before coding:
  - latency: p50/p95/p99
  - throughput
  - error rate
  - CPU, memory, I/O
  - infra cost per request/job
- Confirm acceptance thresholds and rollback thresholds.

### 2) Baseline and hotspot discovery
- Build baseline from current prod-like behavior (same payload shape and scale).
- Identify top hotspots with profiling/tracing/query plans.
- Estimate complexity for hot paths:
  - time complexity (Big-O)
  - space complexity
  - dominant constants (serialization, network, allocations, locks)

### 3) Optimization candidate ranking
For each candidate, score:
- expected gain (latency/cost reduction)
- complexity reduction (`O(n^2)->O(n log n)`, etc.)
- implementation risk
- security risk
- rollback ease

Apply Amdahl reasoning:
- prioritize improvements on the largest runtime fraction first.

### 4) Preferred optimization techniques
- Algorithm/data structure:
  - replace repeated linear scans with indexed/hash lookups
  - avoid nested loops when joins/maps can collapse complexity
  - pre-sort once, binary search many times
- Data and DB:
  - remove N+1 queries
  - add/adjust indexes based on query plans
  - push filtering/aggregation to DB where appropriate
  - use pagination/streaming for large datasets
- Caching:
  - cache pure/idempotent expensive reads
  - define TTL, invalidation, and stale-data strategy
  - never cache sensitive data without strict controls
- Concurrency/parallelism:
  - use bounded concurrency and backpressure
  - avoid lock contention and unbounded fan-out
- Memory:
  - use streaming/chunking to control peak memory
  - reduce large intermediate allocations

### 5) Security guardrails (must pass)
Check optimization changes against these controls:
- Injection prevention: parameterized queries/commands, no dynamic unsafe eval
- Access control: authn/authz logic unchanged and tested
- Sensitive data: no secret/token leakage in cache, logs, traces, metrics
- SSRF/network calls: protocol/host allowlist remains enforced
- Session/auth flows: no weakening due to caching or shortcut paths
- Supply chain: no risky dependency additions without review

If any optimization conflicts with a security control:
- choose safer design, or
- document risk and request explicit user approval.

### 6) Correctness and regression validation
Run (or define if missing):
- functional tests for affected paths
- regression tests for edge cases and failure modes
- performance tests with realistic load and input distribution
- security checks relevant to changed components

Require before/after evidence:
- metric table with absolute values and percent deltas
- note tradeoffs (e.g., memory +8% for p95 latency -35%)

### 7) Production rollout safety
- Roll out behind feature flag/canary when possible.
- Monitor golden signals during rollout:
  - latency, traffic, errors, saturation
- Use explicit abort criteria and rollback command/path.
- Keep rollback low-friction and documented.

## Output format
Always return results in this structure:

1. Baseline
2. Hotspots and complexity findings
3. Ranked optimization plan
4. Security impact review
5. Implemented or proposed changes
6. Validation results (correctness + perf)
7. Rollout and rollback plan
8. Final recommendation

## Forbidden shortcuts
- Disabling security checks for speed
- Removing validation/authz to improve benchmarks
- Ignoring p95/p99 and reporting only averages
- Benchmarking with unrealistic tiny datasets
- Shipping unbounded caches, retries, or concurrency
