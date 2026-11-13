# NOVA v1.0 — AI Operations Center and Autonomous Operations Policy

**Status:** Product operating specification. This document defines intended authority and workflows; it is not evidence that runtime automation has been implemented or enabled.

## 1. Objective

NOVA Operations Center is an AI-managed operations system for the NOVA v1.0 production service. It monitors service health, detects and classifies faults, gathers evidence, opens and updates incidents, performs explicitly allowlisted low-risk recovery actions, verifies outcomes, and reports to the Owner.

It is not an unrestricted administrator. AI agents operate under least privilege, deterministic policy enforcement, tenant isolation, action allowlists, audit logging, and approval gates.

## 2. Operating principles

1. **Observe before acting:** collect timestamped evidence, service identity, release version, correlation ID, and impact scope.
2. **Fail closed:** uncertain market data, authorization, risk state, or security state disables the affected decision path.
3. **Bounded autonomy:** an agent may execute only a named, pre-authorized action within defined scope and rate limits.
4. **Human approval for material risk:** irreversible, customer-impacting, security-sensitive, financial, schema, access-control, deployment, and trading changes require explicit Owner approval.
5. **Tenant isolation:** every observation and action is tenant-scoped; cross-tenant access is prohibited.
6. **No silent success:** verify the postcondition before marking an action or incident resolved.
7. **Auditable operation:** record actor, policy version, evidence, action, target, before/after state, result, and correlation ID. Never log secrets.
8. **No false telemetry:** report unknown or unavailable metrics as unknown, not zero or healthy.

## 3. AI agent roles

| Agent | Responsibility | Authority |
|---|---|---|
| Health Monitor | Probe APIs, workers, DB health, queues, provider and MT5 heartbeat | Read-only |
| Log & Anomaly Analyst | Correlate errors, identify patterns and affected scope | Read-only |
| Bug Triage Agent | Deduplicate, classify severity, create bug records and reproduction plans | Create/update tickets |
| Incident Commander Agent | Assemble timeline, impact, runbook and status updates | Coordinate; no unrestricted remediation |
| Recovery Agent | Run approved, reversible recovery playbooks | Allowlisted low-risk actions only |
| Security Sentinel | Detect auth anomalies, secret exposure indicators and tenant-boundary violations | Read-only plus pre-authorized emergency containment |
| Release Watcher | Verify deployed version, smoke tests, regression signals and rollback readiness | Read-only; no deployment |
| Customer Support Agent | Draft status and support responses from verified facts | Draft/respond within approved support policy; no sensitive disclosure |
| Maintenance Planner | Identify overdue maintenance, vulnerabilities and capacity risks | Create tasks/reports |

A deterministic Policy Enforcement Point (PEP) must validate every proposed action. The model itself is never the authorization mechanism.

## 4. Action authority matrix

### 4.1 Allowed autonomously after implementation and testing

- Read service health, metrics, redacted logs, release metadata and queue status.
- Correlate alerts and deduplicate repeated incidents.
- Create, classify, assign and update bug/incident records.
- Retry idempotent jobs within configured retry budgets.
- Reprocess a known-safe event only when idempotency keys and replay protections are verified.
- Restart a stateless worker or service only through an approved runbook, within rate limits, and after checking blast radius.
- Clear a specifically allowlisted non-authoritative cache.
- Disable a non-critical feature using a pre-approved feature flag when the runbook explicitly permits it.
- Produce diagnostics, daily reports, customer notices for approved templates, and maintenance recommendations.

### 4.2 Emergency containment pre-authorization

For a confirmed or strongly evidenced active security or trading-safety incident, the Security Sentinel may invoke only pre-approved, narrowly scoped containment controls, such as disabling the affected integration or decision capability. It must:
- use a dedicated kill switch with strict scope and expiry;
- preserve evidence and emit a high-priority alert;
- avoid deleting data or changing customer credentials;
- record the exact trigger, policy, target, and postcondition;
- require Owner review immediately after containment.

If evidence is ambiguous, the agent must stop the affected decision path safely and escalate rather than attempt broad remediation.

### 4.3 Owner approval required

- Production deployment, release promotion, rollback, or configuration changes with broad customer impact.
- Database schema changes, migrations, RLS, grants, SECURITY DEFINER functions, or tenant authorization changes.
- Creating, rotating, exposing, or revoking secrets and credentials.
- Deleting, exporting, modifying, or restoring customer or financial data.
- Changing user roles, account status, subscription entitlements, billing, refunds, or access policy.
- Changing broker connections, account ownership, trading limits, or risk policy.
- Enabling any order, execution, or live-trading capability.
- Running unreviewed scripts, arbitrary shell commands, or model-generated SQL against production.
- Disabling security controls, audit logging, backups, monitoring, or rate limits.
- Any action outside the explicit allowlist or with uncertain blast radius.

Approval must be bound to a specific action, target, parameters, evidence, expiry, and approver. A generic approval or chat instruction must not authorize future unrelated actions.

## 5. End-to-end operations flow

1. **Observe:** scheduled probes and event-driven telemetry collect health signals.
2. **Normalize:** map signals to service, tenant scope, release, severity and correlation ID.
3. **Detect:** threshold, anomaly and rule engines identify an incident candidate.
4. **Correlate:** group duplicate alerts and determine whether failures share a root cause.
5. **Triage:** classify severity, customer impact, security implications and trading-safety impact.
6. **Decide:** deterministic policy engine checks whether an action is allowed, requires approval, or is prohibited.
7. **Act:** execute only the exact approved playbook and bounded parameters.
8. **Verify:** check explicit postconditions, health probes and customer-impact signals.
9. **Escalate:** request Owner approval or human response when gates fail or evidence is incomplete.
10. **Communicate:** issue factual status updates without exposing tenant data or secrets.
11. **Learn:** create a root-cause record, corrective tasks and regression tests. Do not self-modify production policy or code.

## 6. Bug management workflow

- Sources: frontend exceptions, API errors, worker logs, database failures, queue dead letters, model/quant errors, stale MT5 data, security alerts and customer reports.
- Each bug record includes: Bug ID, first/last seen, service/version, sanitized error, correlation ID, tenant impact count (not customer data), severity, reproducibility, suspected cause, workaround, source evidence and status.
- The Triage Agent may deduplicate and classify; it must not claim root cause until verified.
- Reproduction occurs in isolated staging or synthetic test tenants.
- Code fixes belong in the NOVA Developer repository.
- Product acceptance verifies the exact source commit, relevant tests, security boundaries and compatibility before importing the fix into the Production repository.
- Close only after post-release verification and a defined observation window.

## 7. Incident severity and response

| Level | Examples | Required handling |
|---|---|---|
| SEV-1 | Cross-tenant exposure, secret compromise, unauthorized broker mutation, risk-gate bypass | Immediate containment, Owner alert, evidence preservation, incident review |
| SEV-2 | Broad login/API/controller outage, widespread data corruption or MT5 outage | Incident coordination, bounded recovery, Owner escalation |
| SEV-3 | Degraded feature, repeated analysis failures, delayed data for a subset | Ticket, workaround, prioritized fix |
| SEV-4 | Cosmetic or documentation defect | Normal backlog |

For trading safety, stale or unverifiable data, missing risk context, model contract violations, or Risk Governor failure must cause abstention/WAIT/REJECT. Never infer permission to trade from an AI response.

## 8. Maintenance schedule

| Cadence | Automated checks | Human review |
|---|---|---|
| Continuous | API probes, error rates, queue age, worker health, auth anomalies, MT5 heartbeat, data freshness | SEV-1/2 alerts |
| Daily | Error trends, failed jobs, AI/Quant failures, tenant denials, resource/cost anomalies, backup job status | Review daily operations report |
| Weekly | Bug aging, dependency/security alerts, DB growth, queue/DLQ, provider reliability | Approve maintenance priorities |
| Monthly | Restore sample, access review, retention checks, dependency upgrades assessment | Review access and capacity |
| Quarterly | Disaster recovery exercise, threat model, RPO/RTO and runbook review | Owner sign-off |

Do not claim backup recoverability from a successful backup job alone; periodically restore into an isolated environment and verify integrity.

## 9. Release and rollback

- Source changes are developed and tested in NOVA Developer.
- Daily intake checks the source repository and selects only complete, relevant, evidence-backed components.
- Product integration is performed directly on the Production repository's stable product line; Developer branch/PR workflow rules do not automatically govern the Product repository.
- Production deployment remains a separate controlled action requiring Owner approval.
- Before release: capture current production version, database compatibility, migration plan, smoke tests, rollback artifact and customer impact.
- After release: run health, auth/tenant, AI, Quant/Risk, MT5 read-only and error-rate checks relevant to the change.
- If postconditions fail, stop rollout and request/execute rollback only under the explicit Owner-approved release policy.

## 10. AI safety and trading boundaries

The initial product remains non-executing and read-only for MT5:
`ordersEnabled=false`
`executionEnabled=false`
`liveTradingEnabled=false`

AI agents may monitor and explain market-data or decision-service health but may not place orders, modify broker accounts, alter risk policy, bypass the Risk Governor, or turn on execution. No agent may treat a user prompt, model output, or upstream repository status as production authorization.

## 11. Required technical components

- Read-only telemetry adapters with tenant-safe query boundaries.
- Structured redacted logs and distributed correlation IDs.
- Health probes and service-level indicators.
- Event ingestion, deduplication, idempotency and durable incident state.
- Deterministic policy engine and action allowlist.
- Approval service with scoped, expiring approvals.
- Sandboxed runbook executor with parameter validation, timeouts and rate limits.
- Audit ledger with append-only semantics.
- Notification adapter and customer-safe status templates.
- Kill switches with narrow scope, expiry and audit.
- Test harness for action simulation, policy denial, retries, duplicate events and failure injection.

## 12. Acceptance gates before enabling autonomous actions

- [ ] Agent identity and least-privilege credentials are defined.
- [ ] Tenant isolation tests cover telemetry, tickets, queues and action execution.
- [ ] Policy enforcement is deterministic and independent of LLM output.
- [ ] Every action has a documented allowlist entry, scope, rate limit and postcondition.
- [ ] Approval tokens are action-bound, parameter-bound and expiring.
- [ ] Runbook executor is sandboxed; arbitrary code/SQL/shell is unavailable.
- [ ] Idempotency, retry budgets, circuit breakers and duplicate-event tests pass.
- [ ] Failure injection proves safe behavior when tools, DB, queues or providers are unavailable.
- [ ] Emergency containment is narrowly scoped and tested.
- [ ] Audit logs are tamper-resistant, redacted and queryable.
- [ ] Restore and rollback have been rehearsed.
- [ ] Owner explicitly approves activation of each autonomous action class.

Until these gates pass, AI Operations runs in **Observe / Recommend mode** only. The policy document does not itself enable any automation or grant production access.
