# NOVA AI Trading v1.0 — Product Scope and User Workflow

## 1. Product promise

A customer-facing web application where each customer owns an isolated NOVA workspace, connects a supported trading account, receives explainable AI analysis, and can use a governed AI Trading Controller. The Controller must route every candidate through deterministic Quant and Risk services. The language model is never the execution authority.

The first release targets MT5 Demo. The application may support more providers later through the same provider adapter contract, but provider support must not be advertised until each adapter passes its own acceptance gates.

## 2. User roles

- **Customer:** owns profile, workspace, broker connections, preferences, research and audit history.
- **Workspace administrator:** manages members and workspace-level policy only within the tenant.
- **Platform operator:** manages provider health, system policy versions, deployment and incident response; cannot silently access customer secrets or bypass tenant ownership.
- **NOVA AI Controller:** proposes typed intents and explains decisions; it has no direct database, secret-store, broker or execution authority.
- **Quant Engine:** evaluates deterministic strategy/model evidence and returns a typed candidate/abstention.
- **Risk Governor:** independently validates account state, market freshness, exposure, policy, limits and idempotency. It may reject or abstain; the AI cannot override it.

## 3. Customer workflow

1. Register and verify identity; create a tenant/workspace.
2. Open Connections and choose MT5 Demo.
3. Read the permissions notice and follow a dedicated secure connection flow. Never submit broker passwords or API secrets in chat.
4. NOVA records a pending connection. A status becomes verified only after a tenant-authorized Bridge request returns real account/market data with fresh timestamps.
5. Open the dashboard and inspect connection state, account summary, market-data provenance, risk state and recent activity.
6. Ask NOVA AI to analyze a symbol, explain a setup, compare scenarios, or inspect a connection.
7. The AI returns structured analysis with source timestamps, assumptions, missing evidence and uncertainty.
8. If the customer asks for a trading decision, the Controller validates the request and requests Quant evaluation. The Quant layer returns a candidate or abstains.
9. The deterministic Risk Governor checks the candidate independently. Any missing, stale, inconsistent, unauthorized or over-limit evidence causes WAIT/REJECT.
10. v1.0 initially records decisions only; broker order execution stays disabled. The customer can inspect the full decision and audit history.
11. Customer may disconnect/revoke a connection; revocation must invalidate sessions/routes and stop future reads.

## 4. Application modules

### Core product
- Authentication, profile, tenant/workspace and role management.
- Customer dashboard and account/connection center.
- AI chat, typed intent router and explanation interface.
- Notifications, activity timeline and audit history.
- User preferences, locale, accessibility and security settings.

### Trading domain
- Broker/provider registry and connection lifecycle.
- MT5 Bridge/Relay/Proxy integration.
- Market data, candles, symbols and freshness/provenance.
- Strategy catalogue and deterministic SMC analysis.
- Quant Engine and model evidence registry.
- AI Trading Controller and Meta Decision Layer.
- Deterministic Risk Governor and policy management.
- Backtest and walk-forward evaluation with immutable artifacts.
- Decision ledger, paper/simulation records and reconciliation.
- Kill switch and environment/account isolation.

### Platform and operations
- Supabase Auth/PostgreSQL with RLS and tenant constraints.
- Cloudflare API/Workers and Durable Objects where appropriate.
- Secret custody adapter; no browser-held provider secrets.
- Job/event lifecycle, retries, idempotency and dead-letter handling.
- CI/CD, staging, release tags, health, monitoring, backups and rollback.
- Admin/operator console, support and incident audit.

## 5. AI authority model

- AI authority level: C — it may interpret user requests, select from approved analytical tools, request evaluations, and explain results.
- AI cannot invent or directly set account identity, tenant ID, provider URL, credentials, risk limits, model approval, execution flags or order parameters as trusted authority.
- All model outputs are untrusted proposals validated against strict versioned schemas.
- SMC is the deterministic setup/context gate. Supporting models (XGBoost, LSTM, Vision, Mamba, Wave) may contribute only when their point-in-time evidence, dataset hash, calibration, OOF provenance and freshness are verified.
- No trained model or strategy is promoted because of UI confidence, sample metrics, or an unverified backtest.
- Quant output is not execution permission. Risk Governor remains independent and authoritative.
- In v1.0, successful evaluation still creates a non-executing decision record.

## 6. Safety defaults and future activation

Initial release defaults:
```
accountEnvironment=DEMO
ordersEnabled=false
executionEnabled=false
liveTradingEnabled=false
```

The system must reject order-like intents at the API boundary while execution is disabled. A future Demo execution release requires a separate owner-approved scope, broker adapter implementation, durable order lifecycle, independent risk enforcement, reconciliation, tenant isolation, fault-injection tests and staged runtime proof. Real trading is a separate authorization and is not part of v1.0 source intake.

## 7. Non-functional requirements

- Strong tenant isolation across API, DB, RLS, queues, caches, object storage and broker routing.
- Least privilege and no secrets in frontend bundles, logs, test artifacts or Git.
- Fail closed on stale data, missing dependencies, provider failures and uncertain authorization.
- Idempotent state transitions and auditable decision provenance.
- Accessible responsive UI, predictable error states and clear not-connected/unverified labels.
- Reproducible builds and exact-head test evidence.
- Operational readiness: alerting, backups, restore/rollback rehearsal and incident runbooks.
- Cost-conscious architecture; no paid resource or billing change without explicit approval.

## 8. Release exit criteria

A production release requires all security, functional, data, controller, tenant, staging, operational and owner-approval gates in README to pass. A running website alone is not a production-ready trading product.
