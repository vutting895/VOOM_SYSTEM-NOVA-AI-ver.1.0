# NOVA v1.0 — Detailed Project Execution Plan

**Product:** NOVA AI Trading SaaS  
**Repository:** vutting895/VOOM_SYSTEM-NOVA-AI-ver.1.0  
**Development source:** vutting895/VOOM_SYSTEM-NOVA-AI  
**Status:** IN PROGRESS — planning and source intake; not production ready  
**Safety baseline:** MT5 Demo read-only; ordersEnabled=false; executionEnabled=false; liveTradingEnabled=false.

## 1. Product objective

Deliver a standalone multi-tenant SaaS web application for customers to register, manage a private workspace, connect an MT5 Demo account through a verified read-only integration, ask NOVA AI for market analysis, inspect Quant and Risk decisions, and review durable evidence and audit history.

The language model is an orchestration and explanation component (AI authority C), not a trading authority. Quant evaluation and deterministic Risk Governor are independent controls. No model output, user prompt, client-side field, or operator convenience path may bypass them.

## 2. Product principles

1. Evidence before claims: a feature is Done only with exact commit, test result, and runtime proof where applicable.
2. Tenant isolation by construction: tenant identity is derived from authenticated server context, never trusted from user-supplied payloads.
3. Fail closed: stale/missing data, unknown account, policy conflict, service outage, invalid schema, or uncertain authorization results in WAIT/REJECT.
4. Read-only first: no broker write capability is included in the initial release.
5. Reproducibility: strategy/model results require data provenance, immutable run artifacts, versioned parameters, and repeatable evaluation.
6. Cost control: prefer existing zero-cost/free-tier architecture; no paid plan, billing, or provisioned paid service without owner approval.
7. Separate release product: source code is imported from the development repository only through documented review and target-repository validation.

## 3. Users and permissions

### Customer
- Owns profile, workspace, broker connections, preferences, conversations, analysis and audit history.
- Can revoke their broker connection and export/delete eligible data under published policy.
- Cannot alter platform-level risk policies or other tenants' data.

### Workspace administrator
- Manages membership and workspace preferences inside their own tenant.
- Cannot access another tenant or retrieve raw broker secrets.

### Platform operator
- Manages service health, provider configuration, incident response, and approved platform policy versions.
- Uses least privilege and auditable support access. No silent customer-secret access.

### NOVA AI Controller
- Interprets intent, chooses approved analytical tools, requests Quant evaluation, and explains results.
- Has no direct broker credentials, order authority, tenant override, or Risk override.

### Quant Engine
- Returns typed candidate, abstention, or evidence insufficiency.
- May only use approved model versions and point-in-time data.

### Risk Governor
- Independently checks identity, account/environment, market freshness, exposure, limits, policy version, idempotency and state.
- Returns ALLOW_FOR_RECORD, WAIT, or REJECT for the non-executing v1.0 workflow. It does not grant broker execution permission.

## 4. Functional workstreams

### WS-01 Product foundation and repository governance
- Maintain product boundary, architecture decision records, source intake register, contribution rules, release checklist and issue tracker.
- Record exact upstream source SHA, path, PR, test evidence, license/dependency implications and limitations for every imported component.
- Reject mock UI metrics, fake Drive files, fabricated manifests, unverified model claims and secrets.

**Acceptance:** product repository has one release branch strategy, documented source provenance, independent CI and a current status register.

### WS-02 Frontend application
- Resolve the upstream Vite root SPA versus separately documented Next.js frontend conflict before importing either.
- Select one frontend stack; define route structure, design system, responsive shell, loading/error/empty states and accessibility.
- Implement customer routes: landing, signup/login, onboarding, dashboard, AI chat, connections, market analysis, decisions, strategy/research, reports, notifications, profile/security and help.
- Implement operator routes separately with explicit role checks.

**Acceptance:** clean install, lint, typecheck, unit tests, production build, route smoke tests, mobile/desktop checks, no sample data presented as real customer data.

### WS-03 Identity, tenant and data security
- Implement Supabase Auth lifecycle and server-verified sessions.
- Define tenant/workspace/member/role model and database constraints.
- Enforce RLS for every tenant-owned table; tenant context must be derived server-side.
- Add account recovery, session revocation, MFA readiness, audit events and retention policy.
- Test two or more users and cross-tenant denial at API and database levels.

**Acceptance:** unauthenticated requests return 401; tenant A cannot read/write tenant B; revoked sessions fail; no service-role key is exposed to clients.

### WS-04 AI Chat and Controller
- Define versioned schemas for user intent, tool request, tool response, decision candidate, explanation and audit event.
- Persist conversations and lifecycle states.
- Implement approved tool registry, allowlist, timeout, retry and idempotency.
- Treat LLM output as untrusted; validate all fields and permissions server-side.
- Keep tenant, account, symbol and environment binding sourced from trusted authenticated context.
- Implement refusal and clarification paths for unsupported or unsafe requests.

**Acceptance:** prompt injection, tool misuse, malformed JSON, replay, tenant spoofing, stale context and provider outage tests fail safely; no broker mutation path is reachable.

### WS-05 Quant Engine and model governance
- Begin with deterministic SMC analysis and only add model adapters after their evidence packet passes.
- Adapter candidates: XGBoost, LSTM, Vision, Mamba and Wave; each requires dataset provenance, feature contract, model hash/version, training cutoff, OOF/OOS evidence, calibration, baseline comparison and walk-forward results.
- Keep model registry states: RESEARCH, CANDIDATE, VALIDATED, APPROVED, RETIRED.
- Do not train/tune on final OOS; prevent look-ahead leakage and duplicate overlapping samples.
- No strategy is advertised as profitable based on in-sample results or synthetic metrics.

**Acceptance:** reproducible evaluation, point-in-time data checks, model approval controls, abstention on missing evidence and immutable run reports.

### WS-06 Risk Governor
- Implement deterministic policy evaluation separate from the AI provider and Quant adapter.
- Validate account identity, Demo environment, symbol, timeframe, quote freshness, spread/data quality, session, exposure, daily loss, max open risk, order count and configured policy version.
- For the initial release, decision records only; reject all execution/order intents at API and adapter boundaries.
- Add global kill switch and per-tenant disable state.
- Persist reason codes and input evidence hashes.

**Acceptance:** adversarial tests prove AI cannot change limits, bypass rejection, replay approval, or turn execution flags on. Missing evidence is WAIT/REJECT.

### WS-07 MT5 integration
- Import only audited read-only System Hub and Bridge/Relay/Proxy components.
- Use a secure tenant-scoped connection lifecycle and secret custody; never accept passwords in chat or store them in browser state.
- Verify authenticated owner-scoped account reads, quote/candle reads, server timestamps and account/environment identity.
- Treat connected socket or HTTP 200 as transport health only, not broker-session verification.
- Test reconnect, disconnect, revocation, stale quotes, timeout, duplicate events, broker restart and cross-tenant denial.

**Acceptance:** isolated staging proves owner read succeeds, foreign tenant is denied, stale data is rejected, disconnect revokes access, and no order endpoint is exposed.

### WS-08 Customer experience and support
- Provide onboarding checklist, connection status, data freshness indicator, AI explanation, risk reasons, notifications, help center and support intake.
- Explain limitations and trading risk in clear Thai/English.
- Provide export, disconnect, delete-request and privacy workflows consistent with retention obligations.

**Acceptance:** a test customer can complete the supported read-only journey without operator intervention; errors are understandable and supportable.

### WS-09 Platform operations
- CI on exact PR head: dependency install, lint, typecheck, unit, contract, integration, security, build and artifact verification.
- Separate development, staging and production environments; no secret reuse across environments.
- Add health endpoints, structured redacted logs, metrics, alerts, backups, restore test, incident response and rollback.
- Add cost/resource monitoring and quotas before public beta.

**Acceptance:** staging deploy, smoke tests, rollback rehearsal, backup restore evidence, alert test and on-call/runbook ownership.

## 5. Delivery phases and exit gates

| Phase | Deliverables | Exit gate |
|---|---|---|
| 0. Foundation | Scope, architecture, intake register, tracker | Approved product boundary and source policy |
| 1. App foundation | Single frontend, CI, design shell | Build/typecheck/tests pass on exact head |
| 2. SaaS identity | Auth, tenant, RLS, roles | Multi-user isolation and revocation tests pass |
| 3. AI Controller | Chat, typed tools, durable events | Controller contract and adversarial tests pass |
| 4. Quant/Risk | SMC adapter, model registry, Risk Governor | Reproducible evaluation and independent risk tests |
| 5. MT5 Demo | Read-only Bridge, owner-scoped reads | Staging proof of account and market data ownership |
| 6. Product ops | Dashboard, reports, support, observability | Customer journey and operations checks pass |
| 7. Release | Security review, rollback, production candidate | All gates pass and owner explicitly approves |

No calendar date is promised until phase estimates, developer capacity and dependency blockers are known.

## 6. Required test matrix

- Unit: schema validation, state transitions, strategy functions, risk rules.
- Contract: frontend/API, Controller/tool, Quant adapter, Risk request/response, MT5 Bridge.
- Integration: Supabase Auth/RLS, durable event lifecycle, Cloudflare worker/DO, broker read path.
- Security: tenant spoofing, IDOR, secret leakage, prompt injection, replay, privilege escalation.
- Reliability: timeout, retry, idempotency, duplicate event, stale data, provider outage, reconnect.
- Trading safety: order-like intent rejection, flags immutable false, no broker write route.
- ML validity: leakage, OOS isolation, walk-forward, baseline, calibration and artifact provenance.
- Operations: deploy smoke, alert, backup restore, rollback and incident drill.

## 7. Definition of Done

A work item is DONE only when:
- Source path and exact commit SHA are recorded.
- Tests run on the exact candidate commit and results are inspectable.
- Security and tenant boundaries are verified for affected surfaces.
- Runtime/staging proof exists where the feature requires external services.
- Known limitations and rollback path are documented.
- No unapproved paid resource or production secret is introduced.

## 8. Current known blockers

1. The source repository contains a Vite root SPA and a separately documented Next.js customer frontend; architecture must be reconciled.
2. Upstream main CI had a CircleCI Pipeline error and a Vercel MT5 read-only bridge failure in the inspected snapshot.
3. MT5 Decision API PR #322 is not a validated Quant/Risk-integrated controller.
4. Hybrid Meta Decision PR #319 lacks trained/calibrated OOF evidence and hosted CI was not green.
5. Quant datasets/model evidence do not currently justify customer-facing performance claims.
6. Real customer MT5 owner-scoped staging proof is outstanding.

## 9. Immediate next actions

1. Re-check source and target main SHAs before every implementation batch.
2. Audit exact auth/thread/approval source files and tests; import only the smallest verified slice.
3. Resolve frontend stack conflict with a documented decision.
4. Add target CI and minimal application skeleton after stack selection.
5. Build tenant isolation and read-only connection path before AI tool authority.
6. Keep all execution flags false throughout v1.0.
