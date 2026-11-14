# NOVA v1.0 — Source Intake Register

**Status:** IN PROGRESS — review required before import  
**Development source:** `vutting895/VOOM_SYSTEM-NOVA-AI`  
**Inspected source main:** `b3ba15c4e8eafc9ee839cb95d42865a0b193e114`  
**Target:** `main` — Production stable branch; accepted components are integrated directly under the Production repository's own controls, without inheriting Developer branch/PR rules.

This register separates reusable implementation from design-only, prototype, blocked, and unsafe material. "Tested upstream" does not mean production-ready. Every imported component must pass target-repository CI and security review.

## Intake decisions

| Component | Upstream evidence / state | v1.0 decision |
|---|---|---|
| Customer authentication + durable AI thread/approval lifecycle | Main docs record local Supabase stack tests with two Auth-issued users, owner/foreign-user denial, persistence after refresh, replay denial; Frontend CI run 40983936466 reported passed | **Candidate — import after exact source/test audit** |
| NOVA System Integration Hub | Checked-in authenticated owner-scoped routes and focused worker tests; real broker path still depends on external Bridge/Relay and live owner-session proof | **Candidate — import read-only slice only** |
| MT5 read-only Bridge/Relay/Proxy | Existing code and CI contracts; older local Windows evidence exists; live owner-scoped customer flow and current bridge runtime are not proven | **Candidate — staging gate required** |
| MT5 AI Decision API | PR #322 is open; its description says latest relevant CI failed and branch is not deployed/provisioned. It aggregates heuristic votes and returns WAIT/REJECT; does not invoke trained Hybrid Quant Engine | **BLOCKED — do not import as a finished controller** |
| Hybrid Meta Decision contract | PR #319 is open; 17 local tests reported passed, but hosted CircleCI is ERROR; no model training/calibration/OOF artifacts or integrated SMC adapter | **RESEARCH ONLY — not production authority** |
| Risk Governor | Proposal/contract work exists in open PRs; reservation and execution integration have separate gates | **Do not treat proposal tests as runtime enforcement** |
| Quant Engine | Main docs describe research foundation/prototypes; no production training or trading service; dataset #238 is training-ineligible | **Research workspace only; no claims of profitable models** |
| Backtest API | PR #321 explicitly removes synthetic metrics and returns 503 until a real data source, cost model and deterministic runner exist | **Preserve fail-closed behavior; feature is unavailable until implemented** |
| Root Vite React SPA | Root README/package describe a Vite SPA; current frontend README separately describes a Next.js App Router customer app | **Architecture conflict — choose and consolidate after file-level audit** |
| Dashboard metrics / audit screens | Main commit diff includes hardcoded OOS metrics, sample trades, synthetic manifests and checkpoint hashes in UI components | **REJECT these fixtures from customer-facing production UI** |
| Workspace / Drive export | Current main commit diff includes fabricated fallback files and generated sample content when Drive reads fail | **REJECT until replaced with real-source-only implementation** |
| Multi-broker adapters | Several designs and public-data prototypes; not all are wired/deployed or private-account verified | **Defer; MT5 first** |

## Required import packet per component

- Source repository, exact commit SHA, branch/PR and changed file list.
- Evidence that the PR is merged or an explicitly approved, reviewed source commit.
- Exact-head CI run URLs and job results; inspect logs for every failure.
- Unit, contract, integration, tenant-isolation and security test coverage.
- Runtime proof where applicable, with environment and limitations stated.
- Dependency and migration inventory; no production data or secrets.
- Target-repository import commit and target CI evidence.

## Known source-level blockers

1. The latest inspected source main commit has mixed product surfaces: root Vite SPA and a separately documented Next.js customer frontend. Do not transplant both as competing apps.
2. The source main commit's CI status includes a CircleCI Pipeline error and a Vercel MT5 read-only bridge failure. Other individual checks are green; this is not an all-green release baseline.
3. PR #322's MT5 Decision API is not a completed production controller. It lacks integrated trained Quant evidence and real broker-origin risk snapshot proof; its latest recorded CI was not green.
4. PR #319's hybrid decision contract has no trained/calibrated OOF model evidence and hosted CI is not green.
5. Backtest, model training, and telemetry must not show success unless backed by reproducible source data and run artifacts.
6. The customer MT5 bridge path must prove authenticated owner reads and foreign-tenant denial against an isolated staging environment before it can be offered as connected.

## Promotion policy

A component may move from Candidate to Imported only after its packet is complete. It may move from Imported to Accepted only after the target repository's exact-head CI and required staging tests pass. A customer-facing production feature is not Done until deployment and operational checks pass.

Never enable broker writes or Live Trading as part of source intake.


---

## Daily product intake audit — 2026-11-13

**Repository snapshots**
- Developer `main`: `b3ba15c4e8eafc9ee839cb95d42865a0b193e114`
- Production `main` before this audit note: `6758dd15fa9ea9a1f86fea2be3f36a3c6567f9cb`

**Current source baseline**
- Developer main combined status is not green: CircleCI Pipeline = error; Vercel MT5 read-only bridge = failure; Vercel MCP server and two individual CircleCI jobs = success. These checks do not establish an all-green build.
- Developer main branch metadata reports `protected=false`; required status checks are disabled.
- Production main currently has no reported commit status checks and branch metadata reports `protected=false`.
- Production currently has product/operations documentation but no application runtime, customer authentication implementation, MT5 integration, AI Controller runtime, or deployable product evidence verified by this audit.

**Candidate classifications**
| Candidate | Classification | Product intake decision |
|---|---|---|
| Main commit adding Google Workspace export and Independent Audit UI | MOCK/UNVERIFIED | HOLD. Open PR #314 explicitly documents fabricated fallback file content, hard-coded OOS metrics, placeholder hashes/run IDs and unverified evidence labels; do not import these views. |
| PR #314 unverified evidence UI containment | INCOMPLETE | HOLD. Open Draft PR; exact-head validation and preview required. |
| PR #316 atomic MT5 capacity reservation | INCOMPLETE / BLOCKED | HOLD. Open Draft PR; PostgreSQL 16 multi-session concurrency evidence and staging proof required; no hosted migration. |
| PR #315 Risk Governor proposal contract | INCOMPLETE / POLICY CONFLICT | HOLD. Open Draft PR; local 16-test claim is not hosted exact-head proof. Proposed 1% single-trade risk must be reconciled with existing policy before integration. |
| PR #308 SMC Mean Rejection | RESEARCH-ONLY / FAILED-VALIDATION | HOLD. Open Draft PR; research simulator and strategy require reproducible cost-aware walk-forward and sealed OOS evidence. |
| PR #313 SMC fixture correction | INCOMPLETE | HOLD. Stacked on #308; local synthetic unit tests do not validate a strategy or independent production component. |
| Latest merged PR candidates #293, #274, #272, #273 | NOT ACCEPTED FOR THIS PRODUCT INTAKE | No component-level production acceptance packet and target-side validation established in this run. |
| Root frontend / Firebase additions | BLOCKED / ARCHITECTURE REVIEW | Do not transplant. Source root mixes Vite, Supabase and Firebase dependencies; Product architecture and target runtime are not yet implemented or reconciled. |

**Import result:** ไม่มีการนำเข้าในรอบนี้.

**Why no import:** The source main CI is not green, key current candidates are open Draft PRs or research-only, source and product architecture are not yet reconciled, and the Production repository has no target application/test runtime with which to verify compatibility and regression behavior.

**Required next actions**
1. Establish the Production app foundation and its own reproducible CI/build/test checks before importing runtime code.
2. Reconcile the target frontend and backend architecture with the source's actual Vite/Supabase/Firebase composition; do not copy competing frontend stacks or project configuration blindly.
3. Require exact-head executed CI and inspect job steps/logs for source candidates, especially #314–#316.
4. Resolve risk-policy conflicts and verify tenant/account ownership and PostgreSQL concurrency before considering MT5 reservation or Risk Governor components.
5. Keep MT5 read-only and all broker execution flags disabled.

**Automation performed:** GitHub read-only inspection and this Production documentation update only. No code import, deployment, migration, secret change, customer-data change, broker action, or autonomous recovery action was performed.


## Daily product intake audit — 2026-11-14

**Run timestamp:** 2026-11-14 17:22:48 Asia/Bangkok  
**Developer source main:** `a54e4b3b9a347502cf031d8bc064b89c5c0e5e80`  
**Production main before this register update:** `ba4fd063432d0c915235afb57cfe75affc0dab96`  
**Decision:** IN PROGRESS — no source component met the complete import and target-validation gates.

### Source changes and exact evidence
- The two commits after the previous recorded source baseline, [`32ccdcc`](https://github.com/vutting895/VOOM_SYSTEM-NOVA-AI/commit/32ccdccfc0c80cc076d708e4e332ac368b89ad69) and [`a54e4b3`](https://github.com/vutting895/VOOM_SYSTEM-NOVA-AI/commit/a54e4b3b9a347502cf031d8bc064b89c5c0e5e80), modify only `colab/nova-auto-learning/NOVA_Colab_Runtime_Preflight.ipynb`. The current notebook is a preflight/audit notebook, not a training pipeline: training is explicitly disabled and it contains no model-fit, market-data ingestion, broker call, or artifact-upload step.
- The notebook's committed output records **Google Drive Mounted: False**, model discovery **NOT FOUND**, dataset validation **BLOCKED (7,419-hour gap reported)**, and feature schema **NOT VERIFIED**. These are saved notebook outputs, not a fresh live query of the user's Drive. [Notebook](https://github.com/vutting895/VOOM_SYSTEM-NOVA-AI/blob/a54e4b3b9a347502cf031d8bc064b89c5c0e5e80/colab/nova-auto-learning/NOVA_Colab_Runtime_Preflight.ipynb)
- Current source main combined status: CircleCI Pipeline = `error`; Vercel MT5 read-only bridge = `failure`; Vercel MCP server = `success`. Vercel contexts are not Netlify production acceptance.
- Current source main also has a failed Supabase Preview check: `Remote migration versions not found in local migrations directory.` No migration was attempted. The scheduled Kaggle ingestion run [`43570442286`](https://github.com/vutting895/VOOM_SYSTEM-NOVA-AI/actions/runs/43570442286) failed; its job had no steps and no retrievable log.
- Current main GitHub Actions history's latest Frontend CI run [`43201808479`](https://github.com/vutting895/VOOM_SYSTEM-NOVA-AI/actions/runs/43201808479) targets the prior source SHA `b3ba15c4e8eafc9ee839cb95d42865a0b193e114`, failed with no steps/logs, and its production smoke job was skipped. The two latest notebook-only commits did not produce a new Frontend CI run.

### Candidate decisions
- **PR #322 — INCOMPLETE / BLOCKED:** current head `f3bef16fc0ee2049a3b58c47bba30953fc6c3cc9`, base `b3ba15c4e8eafc9ee839cb95d42865a0b193e114`, therefore behind current main by the two notebook commits. Exact-head runs [Windows Runtime](https://github.com/vutting895/VOOM_SYSTEM-NOVA-AI/actions/runs/43569405330), [MT5 Contract](https://github.com/vutting895/VOOM_SYSTEM-NOVA-AI/actions/runs/43569405556), [SMC Research](https://github.com/vutting895/VOOM_SYSTEM-NOVA-AI/actions/runs/43569405489), and [Frontend CI](https://github.com/vutting895/VOOM_SYSTEM-NOVA-AI/actions/runs/43569405318) all failed within seconds; each inspected job had an empty step list and logs returned BlobNotFound. Supabase Preview was skipped. No hosted tests passed. The PR remains Draft with no submitted review. The local MetaEditor comment reports 0 errors/0 warnings, but the EX5 binary SHA is missing and the v2.002 EA is not evidenced attached to a chart; Controller/contract runtime remains unverified. No import.
- **PR #321 — INCOMPLETE:** fail-closed backtest endpoint is a useful containment change, but it is stacked on #318 and has no exact-head runtime-test proof. No import.
- **PR #318 — INCOMPLETE / BLOCKED:** removes fabricated telemetry/training/Drive fallback claims and fails unavailable services closed; exact-head Frontend CI failed and logs were not available. Keep unavailable panels unavailable; no import.
- **PR #319 — RESEARCH-ONLY:** 17 Windows local contract tests are reported passing, but no trained/calibrated OOF evidence, no integrated SMC adapter, no walk-forward/backtest evidence, and hosted CircleCI is error. No import.
- **PR #315 — INCOMPLETE / POLICY CONFLICT:** proposal validator only; 16 local tests are reported, but no persistent idempotency/reservation or broker integration. Its 1% per-trade proposal remains unresolved against the conflicting 5% proposal; no risk policy activation.
- **PR #316 — INCOMPLETE / BLOCKED:** PostgreSQL multi-session concurrency workflow is proposed, but exact-head hosted execution and disposable-database evidence are not available. No hosted migration.
- **PR #314 — INCOMPLETE:** removal of unverified Drive/Audit routes is a containment proposal; exact-head route/build validation remains unproven.
- **PRs #308/#313 and #238 — RESEARCH-ONLY / BLOCKED:** SMC remains unvalidated; #238 still reports 12,733 rows and 18 unexplained gaps including the 7,419-hour outage, with `training_eligible=false`. No training or promotion.
- Recent merged PRs reviewed include #293, #274, #273, #272, #268, #255, #249, #248, #247, #245, #242 and #211. These are corrective revert, UI/research workspace, Developer-only CI/proof tooling, documentation/preflight, research primitives, secret-gated ingestion, or app-coupled safety code. Merge alone does not satisfy Production compatibility or target-side tests. None qualifies for import.

### Production target and readiness
- Production `main` contains only `README.md` and `docs/` (Product Scope, Execution Plan, Service Operations, Operations Automation, Go-to-Market and Source Intake). No application source, package manifest, backend, integrations, Supabase migrations, or CI workflow exists in the target tree. Production check-runs and Actions runs are empty; this is absence of validation, not a passing build.
- Product remains **NOT PRODUCTION READY**. Customer app/auth/tenant runtime, AI Chat/Controller, Quant service, deterministic Risk Governor, MT5 read-only adapter, durable event/audit runtime, support/monitoring, backup/restore and deployable build remain unimplemented or unverified in this repository.
- [Issue #48](https://github.com/vutting895/VOOM_SYSTEM-NOVA-AI/issues/48) remains open: Leaked Password Protection is disabled on the Supabase Free plan and cross-customer regression evidence is incomplete. No upgrade or billing change is authorized.
- AI Operations Center remains a documented policy only. Keep Observe/Recommend; no operational agent runtime or autonomous recovery action is evidenced.

### Intake result and safety
**ไม่มีการนำเข้าในรอบนี้.** No component passed exact-source checks, security/tenant review, compatibility review and target-side regression validation. No Developer files were changed. No deploy, paid resource, migration, RLS/grant/auth change, secret change, customer-data action, training, controller start, or broker write was performed. Preserve `ordersEnabled=false`, `executionEnabled=false`, and `liveTradingEnabled=false`.
