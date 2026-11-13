# NOVA v1.0 — Source Intake Register

**Status:** IN PROGRESS — review required before import  
**Development source:** `vutting895/VOOM_SYSTEM-NOVA-AI`  
**Inspected source main:** `b3ba15c4e8eafc9ee839cb95d42865a0b193e114`  
**Target branch:** `feature/v1.0-source-intake`

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
