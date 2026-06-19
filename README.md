# code-performance-optimizer-skill

**Production code optimization with Big-O analysis and security guardrails.**

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/hireblackout/code-performance-optimizer-skill?style=social)](https://github.com/hireblackout/code-performance-optimizer-skill)

Optimize backend/API code for production without breaking security posture or reliability. This skill enforces a structured pipeline: measure → complexity rank → algorithm-first redesign → security review → canary rollout. Aligned with OWASP Top 10 (2025), ASVS v5, NIST SSDF, and Google SRE golden signals.

## Who is this for

| Role | Use case |
|---|---|
| Backend/SRE engineer | Slow endpoints, high p99, CPU/memory spikes |
| Platform team | Cost reduction via code efficiency |
| Security engineer | Reviewing perf changes for security regressions |
| Tech lead | Enforcing optimization standards across the team |

## Supported stacks

| Layer | Technologies |
|---|---|
| Languages | Python, Node.js, Go, Java, TypeScript |
| Frameworks | FastAPI, Django, Express, NestJS, Gin, Spring |
| Databases | PostgreSQL, MySQL, MongoDB, Redis, SQLite |
| Queues | Celery, Bull, SQS, RabbitMQ |
| Infra | Docker, Kubernetes, AWS Lambda, Cloud Run |

## Quick start

**Trigger phrases:** slow API, high p99, CPU spike, expensive query, scale issue, "make it faster", optimization

The skill loads automatically when you describe a performance problem. Or invoke directly:

```bash
# The skill will detect the problem and run through the 7-step pipeline
optimize my slow endpoint with prod safety checks
```

### Verify installation

```bash
ls ~/.agents/skills/code-performance-optimizer-skill/SKILL.md
# Should return the skill file path
```

## Workflow (7 steps)

1. **Intake + target definition** — capture endpoint, SLO, constraints
2. **Baseline + hotspot discovery** — profile, trace, query plans, Big-O annotation
3. **Optimization ranking** — score candidates by gain, risk, security impact
4. **Preferred techniques** — algorithm, data structure, caching, concurrency, DB
5. **Security guardrails** — OWASP/ASVS injection, authz, sensitive data, SSRF checks
6. **Correctness + perf validation** — before/after metrics, tradeoff docs
7. **Production rollout** — feature flag, canary, golden signals, rollback plan

## Example outputs

| Scenario | Before | After | Security note |
|---|---|---|---|
| Checkout API promo validation | p95 640ms, CPU 85%, DB pool saturated | p95 210ms, CPU 52%, pool stable | Authz model preserved, no cache of PII |
| Financial reconciliation job (8M rows) | 3h 40m, OOM at peak | 51 min, memory flat at 2.1 GB | PII scrubbed from logs, no data loss |
| Search suggestions endpoint | p99 1.2s, full scan per request | p99 145ms, indexed + bounded cache | Rejected skip-auth shortcut, safe alternative |

## Installation

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/hireblackout/code-performance-optimizer-skill ~/.agents/skills/code-performance-optimizer-skill
```

Or install via Claude plugin marketplace when registered.

## License

Apache 2.0. See [LICENSE](LICENSE).
