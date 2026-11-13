# NOVA AI Trading — Version 1.0

**Product goal:** a standalone, multi-user production web application with an AI Trading Controller.

> **Release status: NOT PRODUCTION READY.**
> This repository is newly initialized. No application source, runtime, broker integration, model artifact, or deployment has been imported or verified yet. A repository existing is not evidence that the product is usable.

## Product boundary

NOVA v1.0 is a separate product repository. The canonical development repository remains
[`vutting895/VOOM_SYSTEM-NOVA-AI`](https://github.com/vutting895/VOOM_SYSTEM-NOVA-AI).
The two repositories have independent branches, CI, release tags, deployment configuration,
secrets, and release evidence. Changes in the development repository do not automatically
change this product repository.

Only source code and assets that pass the v1.0 intake review and have reproducible,
inspectable test evidence may be imported. Do not copy the whole development repository,
unreviewed branches, mock UI data, fabricated metrics, credentials, or environment files.

## Product scope

### Customer web application
- Multi-user registration, authentication, session lifecycle, and account recovery.
- Tenant-isolated customer workspace and settings.
- NOVA AI chat and AI-assisted market research.
- Broker connection onboarding with explicit provider/environment/capability status.
- Trading dashboard: account state, market data provenance/freshness, positions, risk, decisions,
  event history, and audit trail.
- Strategy/research workspace, backtesting, evaluation, and model registry.
- User-facing notifications, support, and security controls.

### AI Trading Controller
- Typed intent and decision contracts; AI output is untrusted input.
- Deterministic Quant Engine and Risk Governor are independent of model authority.
- Market-data provenance, freshness, timestamp, symbol, timeframe, and account checks.
- Strategy evidence and model-version governance.
- Idempotent event processing, durable state, audit records, and reconciliation.
- Explicit tenant/account/environment authorization before every protected operation.
- Fail-closed behavior on missing, stale, inconsistent, or unverifiable evidence.

### MT5 first integration
The first broker target is customer-owned MT5 Demo through a tenant-isolated bridge/API boundary.
The product must prove authenticated owner-scoped account and market-data reads. A connected
socket, saved credential, or HTTP 200 alone is not proof of a verified broker session.

## Target architecture

- **Frontend:** customer-facing NOVA web application.
- **Identity and data:** Supabase Auth + PostgreSQL with enforced tenant isolation/RLS.
- **Backend/API:** Cloudflare Workers and Durable Objects where suitable; private services for
  workloads that cannot run in the Workers runtime.
- **AI orchestration:** provider gateway, NOVA AI Controller, typed tool/intent contracts.
- **Quant and risk:** deterministic server-side services; model outputs cannot bypass risk.
- **Broker connectivity:** tenant-scoped MT5 Bridge/Relay/API; no public terminal ports.
- **Operations:** exact-head CI, staged deployments, health checks, audit, rollback, and monitoring.

Hosting choices are targets, not proof of an existing deployment. No paid service or billing
resource may be provisioned without explicit approval.

## Trading safety defaults

```text
ordersEnabled=false
executionEnabled=false
liveTradingEnabled=false
```

Version 1.0 development and acceptance must begin with read-only market/account data and
non-executing AI decisions. Any later Demo order execution requires a separately reviewed
implementation, independent risk controls, end-to-end tests, and explicit owner approval.
Real-money trading is a separate release authorization and cannot be enabled by AI, a customer
prompt, a model response, or a configuration supplied by an untrusted client.

## Source intake and release policy

1. Inspect the exact source branch/commit in the development repository.
2. Verify PR review, ancestry, exact-head CI, tests, security boundaries, and known limitations.
3. Reject mock/sample/fabricated data and any code with unverified production claims.
4. Import only the reviewed files/commits into a dedicated v1.0 feature branch.
5. Run the target repository's own typecheck, unit/contract/integration tests, build, and security
   checks. Upstream CI is supporting evidence, not a substitute for target-repository validation.
6. Validate staging with isolated test tenants and non-production broker accounts.
7. Promote only an immutable, tagged release with deployment, rollback, and operational evidence.

## Initial acceptance gates

- [ ] Source intake inventory and provenance recorded.
- [ ] Frontend build, typecheck, unit tests, and route smoke tests pass on exact release candidate.
- [ ] Authentication, tenant isolation, RLS, and cross-tenant denial tests pass.
- [ ] AI Controller contract and adversarial-input tests pass.
- [ ] Quant Engine and deterministic Risk Governor tests pass.
- [ ] MT5 owner-scoped read-only account and market-data integration passes.
- [ ] Durable event/idempotency/retry/reconciliation tests pass.
- [ ] No broker mutation path is reachable; execution flags remain false.
- [ ] Security review, secrets review, dependency review, and threat model complete.
- [ ] Staging acceptance, rollback rehearsal, and production operations checks complete.
- [ ] Production release explicitly approved by the owner.

## Repository rules

- This repository is the release candidate/product source for NOVA v1.0.
- Never use the development repository's `main` as a deployable release without this intake process.
- Never mark a feature DONE without exact commit + test evidence.
- Never store API keys, broker passwords, private keys, service-role keys, or production secrets
  in Git, issues, logs, test fixtures, or AI prompts.
- Preserve customer data isolation across every API, worker, queue, cache, object, and database query.
- Keep Affiliate and NOVA Video projects outside this repository.

## Project and service documentation

- [Detailed execution plan](docs/PROJECT-EXECUTION-PLAN.md)
- [Go-to-market and marketing strategy](docs/GO-TO-MARKET.md)
- [Service delivery and operations model](docs/SERVICE-OPERATIONS.md)
- [Product scope and customer workflow](docs/PRODUCT-SCOPE.md)
- [Source intake register](docs/SOURCE-INTAKE.md)

## Current status

Only repository initialization and product/release boundaries are established. The application
and AI Trading Controller still require source intake, implementation, and full verification.
