# Unified NOVA v1 + ALLINONE Architecture

**Status:** Architecture baseline / implementation not yet integrated  
**Target repository:** `vutting895/VOOM_SYSTEM-NOVA-AI-ver.1.0`  
**Target branch:** `main` (protected; do not modify directly)  
**Working branch:** `feature/unified-nova-allinone-architecture`  
**Architecture:** Unified monorepo with explicit module and API boundaries

## 1. Authority and repository policy

NOVA v1.0 is the master product repository and the long-term source of truth for the unified NOVA + ALLINONE product. ALLINONE is an imported subsystem, not a second master repository. Existing source repositories remain preserved as provenance and rollback references; no source repository is deleted or rewritten as part of migration.

All implementation work is performed on feature branches and proposed through pull requests. Do not commit to `main`, merge, deploy, provision paid resources, alter production secrets, or run database migrations without separate authorization.

This decision supersedes the earlier NOVA v1 README wording that described the other NOVA repository as the canonical development repository. The old repository may still be consulted as an upstream source, but NOVA v1 is the integration target.

## 2. Target repository layout

```text
NOVA-v1/
├── apps/
│   ├── web/                         # NOVA customer and operations UI
│   └── api/                         # Public application API / BFF
├── nova/
│   ├── core/                        # orchestration, decision, policy
│   ├── intelligence/                # model interfaces and inference
│   ├── ml/                          # XGBoost, LSTM, PyTorch, vision
│   ├── strategies/                  # GOLD-only strategy research
│   ├── data/                        # ingestion, validation, features
│   ├── training/                    # offline training pipelines
│   └── evaluation/                  # backtest, walk-forward, OOS
├── allinone/
│   ├── adapter/                     # reviewed Node API adapter
│   ├── mt5/                         # MQL5 EA sources (pending provenance)
│   ├── bridge/                      # authenticated MT5 bridge
│   ├── risk/                        # deterministic local risk checks
│   ├── execution/                   # disabled until separately approved
│   └── journal/                     # immutable event/trade journal
├── shared/
│   ├── contracts/                   # versioned API and event schemas
│   ├── types/
│   └── protocol/
├── infrastructure/
│   ├── cloudflare/
│   ├── supabase/
│   └── netlify/
├── security/
│   ├── auth/
│   ├── tenant-isolation/
│   └── safety-policy/
├── tests/
│   ├── unit/
│   ├── contract/
│   ├── integration/
│   └── safety/
├── docs/
│   ├── architecture/
│   ├── source-intake/
│   └── operations/
└── README.md
```

This is the intended destination layout, not a claim that all directories or runtime components already exist. Add directories only with reviewed implementation or documented purpose.

## 3. Component ownership

| Component | Authority | Responsibility |
|---|---|---|
| NOVA AI / model runtime | NOVA | Market analysis and research inference |
| Meta Decision Layer | NOVA | Combines model and strategy evidence; emits typed proposal |
| Deterministic Quant Engine | NOVA | Reproducible calculations independent of model output |
| Risk Governor | NOVA + local ALLINONE guard | Rejects invalid or unsafe proposals |
| ALLINONE adapter | ALLINONE | Authenticated API boundary and MT5 read-only relay |
| MQL5 EA / terminal | ALLINONE | Terminal-side market/account collection; execution remains disabled |
| Journal / reconciliation | Shared contract, persisted by backend | Event history, deduplication, reconciliation |
| Dashboard | NOVA | Displays provenance, health, analysis, risk and audit state |

AI output is untrusted input. It cannot set tenant identity, broker environment, execution permissions, risk policy, or secrets.

## 4. Data and decision flow

1. MT5 Demo terminal publishes bounded, timestamped XAUUSD market/account data through the authenticated bridge.
2. NOVA validates symbol, timeframe, UTC timestamp, provenance, freshness, tenant and account scope.
3. NOVA builds features and market context for M1, M5, M15, M30 and H1. M1 is the base timeframe.
4. Research models and deterministic strategy modules produce typed evidence, not broker commands.
5. Meta Decision Layer emits a proposal with model versions, evidence references, expiry and confidence.
6. Independent Risk Governor validates limits and returns allow/deny with reason codes.
7. ALLINONE may display or journal an approved research decision. Broker order submission remains unavailable.
8. Results and events return to NOVA for evaluation; no automatic model promotion or online self-modification.

## 5. Shared API contract requirements

All cross-module APIs must be versioned and schema-validated. Minimum fields include:
- request/event ID and idempotency key;
- tenant and account scope derived from verified identity, never trusted from browser payload;
- symbol exactly `XAUUSD` at product level, with broker symbol mapping such as `XAUUSDm` stored as account-specific metadata;
- timeframe from the approved set;
- source timestamp in UTC, received timestamp, freshness and provenance;
- schema version, producer and model/strategy version;
- explicit read-only capability and decision expiry.

Required endpoint families: health, heartbeat, market data, history, account summary, positions read, analysis, signal proposal, risk validation, journal and audit. Order submission is not part of the active contract.

## 6. Trading safety invariants

```text
ordersEnabled=false
executionEnabled=false
liveTradingEnabled=false
```

- Read-only account and market-data access only.
- No order, close, modify, withdrawal or transfer capability.
- Demo connection creation must be fixed server-side to Demo and owner-scoped.
- Risk checks fail closed on missing, stale, conflicting or unverifiable data.
- AI cannot change policy or enable execution.
- Bridge loss blocks new data-dependent decisions and marks connectivity unknown.
- Idempotency, replay protection, audit and reconciliation are required before any future execution proposal.
- Any future Demo execution requires a separate design review, independent risk tests and explicit owner approval. Live trading requires a separate release authorization.

## 7. Migration phases

| Phase | Deliverable | Acceptance gate |
|---|---|---|
| 0 | Source inventory and provenance | Exact source SHAs, file manifest, license/security review |
| 1 | Shared contracts | Schema tests and compatibility policy |
| 2 | ALLINONE adapter intake | Unit/contract tests, auth and tenant checks |
| 3 | MT5 source intake | Original EA source obtained and reviewed; no binary-only assumptions |
| 4 | NOVA AI/Quant modules | Deterministic tests, dataset provenance, OOS evidence |
| 5 | Bridge integration | Authenticated read-only Demo integration and reconnect tests |
| 6 | System integration | Exact-head integration and safety test suite |
| 7 | Staging acceptance | Isolated test tenant, rollback and operational evidence |
| 8 | Release gate | Explicit owner approval; no automatic production release |

## 8. Explicit non-goals for this migration

- No bulk copy of the development NOVA repository.
- No import of mock balances, fabricated telemetry, credentials, environment files or sample secrets.
- No migration of unverified strategy claims or backtest metrics.
- No automatic trading, broker writes, live mode, paid infrastructure or production deployment.
- No destructive refactor of the original ALLINONE source repository.
