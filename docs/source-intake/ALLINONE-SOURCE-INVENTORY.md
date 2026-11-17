# ALLINONE Source Intake Inventory

**Target:** `vutting895/VOOM_SYSTEM-NOVA-AI-ver.1.0`  
**Source repository:** `vutting895/allinone`  
**Source default branch:** `main`  
**Source feature candidate:** `feature/nova-service-api-integration`  
**Status:** IN PROGRESS — no source imported by this record

## 1. Source identity and observed revisions

- ALLINONE main observed at `a37390cd157d7e8f3d19160a37b18bc421bbca68`.
- The repository contains a legacy/demo snapshot with `index.html`, `server.js`, `package.json`, `vercel.json`, `README-DEPLOY.md`, and `.gitignore`.
- A separate feature branch `feature/nova-service-api-integration` and Draft PR #1 were found. Comparing it with main reports 21 commits ahead and 0 behind, with six changed files: `README-DEPLOY.md` (modified, +50/-9), `api/server.js` (added, +226), `index.html` (modified, +299/-1), `package.json` (modified, +7/-2), `test/server.test.js` (added, +183), and `vercel.json` (modified, +8/-2). The connector did not expose the feature branch head SHA in the compare response; capture it directly before any import. The PR describes a more recent authenticated NOVA adapter, read-only Exness Demo integration, NOVA AI research routes, and order-endpoint denial.
- The repository's default branch and feature branch are materially different. Do not treat the default-branch demo snapshot as the complete integration candidate.

## 2. Main-branch findings

Observed files:
- `index.html`: legacy dashboard with hard-coded sample balances, multiple markets, and a Google Sheet identifier.
- `server.js`: simple JSON response; does not implement the API path declared by the package start script.
- `package.json`: start script points to `api/server.js`, which was not present on main at inspection.
- `vercel.json`: points to `api/server.js` and `public/**`, which do not match the observed main-branch file layout.
- `README-DEPLOY.md`: contains a demo deployment recipe and a sample API key. Treat all values in this file as untrusted legacy content; do not reuse them as credentials or deployment configuration.
- `.gitignore`: ignores `.env`, `node_modules`, and `.vercel`.

Decision: do not import the ALLINONE main-branch demo snapshot as a runnable production subsystem. Its file layout and deployment instructions are inconsistent with the observed source layout and contain sample/demo material.

## 3. Feature-branch candidate findings

The feature branch `feature/nova-service-api-integration` contains:
- Node.js 20+ dependency-free HTTP adapter.
- Supabase Auth token validation and user-token forwarding to NOVA System Hub / NOVA AI Worker.
- Exness Demo-only connection request path.
- Authenticated read-only connection status, account and quote routes.
- AI marketplace, research chat and strategy-draft routes.
- A dashboard that keeps the access token in memory.
- A server-side deny-all order endpoint returning HTTP 403 for every method.
- Offline test scripts declared in `package.json`; exact-head test execution is still required.

The PR description states that upstream Hub/AI Worker routes were checked in the development NOVA repository. Those upstream services are not yet proven to exist in this NOVA v1 target repository. The adapter must not be imported as a complete standalone system until upstream dependencies are either ported and verified or replaced with a target-owned contract implementation.

## 4. GOLD-only scope gaps

The current feature candidate permits several markets including BTC/USDT, ETH/USDT, EUR/USD and US30 in strategy-draft validation, and uses a generic symbol input for quote lookup. The NOVA v1 product scope for this integration is GOLD only.

Before acceptance:
- Restrict strategy and data routes to the approved XAUUSD product scope.
- Keep broker-specific symbol aliases (for example XAUUSDm) in an explicit account mapping, not as alternate product instruments.
- Reject unsupported symbols server-side.
- Validate timeframes M1, M5, M15, M30 and H1 for this product.
- Verify UTC timestamps, bar limits, data provenance and freshness.

## 5. Missing / unverified source

- Original `MTRADERS_AllInOne_v1_01` EA source and its claimed 30 modules have not been located in the inspected ALLINONE repository paths.
- No MQL5 source tree has been verified in the target candidate.
- No verified compiled EA binary, source-to-binary provenance, MetaEditor build evidence or terminal runtime evidence is recorded here.
- The actual NOVA System Hub and AI Worker contracts require independent target-side verification.
- Exact feature-branch head SHA, final review state and exact-head test results must be captured before import. The changed-file manifest and per-file diff statistics are now recorded above.

## 6. Security and safety review

- Do not copy the sample API key, Google Sheet identifiers, broker credentials, tokens or environment files.
- Do not trust sample account balances or dashboard metrics as live data.
- Preserve `ordersEnabled=false`, `executionEnabled=false`, `liveTradingEnabled=false`.
- Keep all order routes denied.
- No deployment, production mutation, Supabase migration, billing change or broker action is authorized by this intake.

## 7. Intake decision

**Current decision: PARTIAL CANDIDATE / BLOCKED FOR IMPORT.**

The feature-branch adapter is a plausible source component, but the full ALLINONE engine and original MQL5 EA are not evidenced. Next step is to capture the feature branch's exact head and changed-file manifest, inspect all tests and upstream contracts, obtain the original EA source, then port reviewed files into NOVA v1 under `allinone/` on a separate feature branch.
