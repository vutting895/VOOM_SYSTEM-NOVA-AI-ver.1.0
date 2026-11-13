# NOVA v1.0 — Service Delivery and Operating Model

**Status:** Operating model proposal; customer-facing service must not launch until release gates pass.

## 1. Service model

NOVA provides software for AI-assisted market analysis and trading decision support. Initial service scope is:
- Account registration and private workspace.
- AI chat for market and platform questions.
- Verified MT5 Demo account and market-data reads where supported.
- Quant analysis and risk-governed non-executing decision records.
- Reports, notifications, audit history and customer support.

Not included in initial service:
- Broker order placement or execution.
- Real-money trading.
- Guaranteed signals, returns or individualized financial advice.
- Any broker integration that has not passed provider-specific acceptance.

## 2. Customer lifecycle

### Onboarding
1. Customer reviews product scope, risk disclosure, privacy notice and terms.
2. Customer registers and verifies email/identity as required.
3. System creates a tenant and default workspace.
4. Customer configures security settings and reviews data permissions.
5. Customer opens MT5 Demo connection instructions.
6. Customer authorizes a secure connection flow; secrets never go into AI chat or frontend logs.
7. System marks connection PENDING until a tenant-authorized Bridge read verifies account identity, environment and fresh data.
8. Dashboard explains connection state and any missing setup step.

### Normal usage
1. Customer selects symbol/timeframe and requests analysis.
2. Controller validates identity, tenant, permissions and request schema.
3. Market data is fetched from approved provider and freshness/provenance are checked.
4. Quant Engine evaluates available approved strategy/model components or abstains.
5. Risk Governor independently evaluates context and policy.
6. Controller presents result, rationale, timestamps, limitations and reason codes.
7. Decision record and audit event are persisted.
8. Customer can inspect prior analyses and report a problem.

### Disconnect and account closure
- Customer may revoke a broker connection.
- Revocation invalidates connection/session access and stops subsequent reads.
- Customer can request data export or deletion under published legal and retention rules.
- Deletion jobs must be auditable and must not erase legally required security/audit records where retention is required.

## 3. Service tiers and entitlements

Initial preview should use explicit quotas rather than unbounded access. Entitlements may include:
- AI messages/analysis requests per time window.
- Supported symbols/timeframes.
- Conversation and report retention.
- Number of broker connections.
- Workspace seats.
- Research/backtest access only when validated.
- Support response target.

All quotas must be enforced server-side, not only hidden in UI. Pricing and limits remain hypotheses until real costs and customer demand are measured.

## 4. Support operations

### Support channels
- In-app support request with category and correlation ID.
- Published support email or ticket channel after one is provisioned.
- Public status/known-issues page for incidents.

### Ticket categories
- Account/login/security.
- MT5 connection and data freshness.
- AI analysis quality or explanation.
- Incorrect/unclear risk decision.
- Billing/subscription (only after enabled).
- Bug, outage or data/privacy request.

### Support workflow
1. Create ticket and correlation ID.
2. Confirm tenant identity through authenticated support workflow.
3. Classify severity and impact.
4. Gather redacted logs and event IDs; never request passwords or tokens.
5. Reproduce in staging or approved test tenant.
6. Escalate security, cross-tenant or suspected trading-safety incidents immediately.
7. Communicate status and workaround.
8. Resolve with root cause, fix version and customer confirmation where appropriate.

Do not promise response-time SLAs until staffing and monitoring are in place.

## 5. Incident severity and response

| Severity | Example | Required response |
|---|---|---|
| SEV-1 | Cross-tenant data exposure, unauthorized broker mutation, secret compromise | Disable affected capability, preserve evidence, incident lead and owner escalation |
| SEV-2 | Core login/controller/MT5 service unavailable for broad cohort | Mitigate, status update, rollback or fail closed |
| SEV-3 | Feature degraded, repeated analysis failures, delayed data | Investigate, publish known issue, prioritize fix |
| SEV-4 | Cosmetic defect or documentation issue | Backlog and normal release |

For any trading-safety uncertainty, disable the affected integration or decision path and fail closed. Do not continue based on optimistic assumptions.

## 6. Operational controls

- Separate development, staging and production projects/accounts.
- Least-privilege credentials and secret rotation.
- No secrets in Git, browser storage, chat, telemetry or support tickets.
- Tenant-scoped authorization for every request, event, queue, cache and object.
- Redacted structured logs with correlation IDs.
- Metrics for request success/error, latency, queue age, provider health, data freshness, AI usage and quota.
- Alerts for auth anomalies, cross-tenant denial spikes, stale market data, repeated retries, DLQ growth and cost spikes.
- Database backups, restore drills and documented RPO/RTO targets before launch.
- Release tags, immutable build artifacts, deployment smoke tests and rollback plan.
- Customer-facing status page and incident communication templates.

## 7. Data handling

Classify data:
- Public: educational content and public product documentation.
- Customer workspace: profile, preferences, chat and analysis history.
- Financial/account data: broker account metadata, balances, positions and market-linked records.
- Secrets: broker credentials, API keys, service credentials and signing material.

Controls:
- Store secrets only in approved secret custody; never in chat or client bundles.
- Encrypt data in transit and use provider-supported encryption at rest.
- Apply tenant RLS and server authorization.
- Minimize retention and analytics collection.
- Define export, deletion, legal hold and backup expiry policies.
- Audit privileged support/operator access.

## 8. Service health and reporting

Internal daily/weekly operational dashboard should include:
- Availability and error rate by service.
- P50/P95 latency.
- AI request volume, failure and timeout rates.
- Market-data age and provider failures.
- MT5 connection verified/pending/disconnected counts.
- Tenant authorization denials and security alerts.
- Queue retry and dead-letter counts.
- Cost per tenant and per analysis.
- Support ticket volume, age and severity.
- Release version and rollback readiness.

Do not display synthetic metrics as production telemetry.

## 9. Release and change management

1. All changes enter via feature branch and PR.
2. Record source provenance for imported upstream code.
3. Require exact-head CI and relevant security review.
4. Deploy to isolated staging.
5. Run migration, tenant, broker-read, controller, risk, smoke and rollback tests.
6. Obtain explicit owner approval for production promotion.
7. Monitor release and retain rollback artifact.
8. Publish release notes and known limitations.

No production secret creation, paid infrastructure provisioning, or broker execution activation without explicit owner approval.

## 10. Service readiness checklist

- [ ] Authentication, tenant isolation and RLS pass.
- [ ] Customer onboarding and connection revocation pass.
- [ ] Controller and tool authorization tests pass.
- [ ] Quant and Risk independent tests pass.
- [ ] MT5 Demo owner-scoped reads pass in staging.
- [ ] No broker mutation endpoint is reachable.
- [ ] Logging is redacted and correlated.
- [ ] Monitoring and alerts are exercised.
- [ ] Backups and restore are tested.
- [ ] Incident and support runbooks are assigned.
- [ ] Terms, privacy and risk disclosures reviewed.
- [ ] Production deployment and rollback are rehearsed.
