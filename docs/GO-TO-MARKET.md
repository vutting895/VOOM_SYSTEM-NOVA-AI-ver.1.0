# NOVA v1.0 — Go-to-Market and Marketing Plan

**Status:** PRE-LAUNCH PLAN — do not market unavailable features as live  
**Initial market:** Thai-speaking MT5 Forex/Gold traders and trading-education communities  
**Product positioning:** AI-assisted market analysis, explainable decisions, account visibility and risk governance.

## 1. Positioning and claims policy

### Core positioning
NOVA is an AI Trading Analysis and Decision Platform. It helps traders inspect market context, ask questions in natural language, connect an MT5 Demo account for verified read-only data, and understand how analytical and deterministic risk checks produce a decision.

### Approved claims only after feature verification
- AI-assisted market analysis and explanation.
- Multi-timeframe analysis when the relevant data source is actually connected.
- MT5 Demo account and market data visibility after owner-scoped staging proof.
- Transparent WAIT/REJECT reasons and decision history after Controller/Risk integration passes.
- Backtest and walk-forward reports only when generated from real, traceable data and reproducible runs.

### Prohibited or unsubstantiated claims
- Guaranteed profit, guaranteed win rate, passive income or risk-free trading.
- "AI trades better than humans" without comparative evidence and methodology.
- Performance percentages, Sharpe, profit factor or trade examples derived from mock, synthetic, in-sample or unverifiable data.
- "Fully automated trading" while broker execution is disabled.
- Broker/provider compatibility before that adapter passes its own acceptance tests.

Every public performance statement must link to methodology, date range, costs/spread/slippage assumptions, sample size, OOS status, limitations and reproducible report.

## 2. Customer segments and jobs-to-be-done

| Segment | Job / pain | Initial offer |
|---|---|---|
| New MT5 trader | Wants to understand charts and trading concepts | AI explanation and educational analysis |
| Active discretionary trader | Spends time checking multiple timeframes and setups | Consolidated analysis and decision rationale |
| Risk-conscious trader | Wants clearer account and exposure visibility | Read-only account monitoring and risk explanations |
| Strategy researcher | Needs a repeatable way to evaluate ideas | Evidence-based backtest/model research workspace |
| Trading educator/community | Needs a structured tool for demonstrations | Limited cohort workspace and guided onboarding |

Primary launch segment: active MT5 Demo users who value analysis transparency. Avoid targeting users expecting guaranteed signals or automatic profit.

## 3. Offer and launch sequence

### Stage A — Waitlist
- Public landing page with honest feature availability.
- Product screenshots only from actual implementation; label prototype images clearly.
- Collect email only with consent and publish privacy/communication terms.
- Interview prospective users about workflows, broker setups and willingness to pay.
- No implication that a waitlist position guarantees access.

### Stage B — Private research preview
- Invite a small cohort after Auth, tenant isolation and security gates pass.
- Provide guided onboarding and sandbox/Demo-only usage.
- Collect structured feedback and opt-in telemetry.
- No execution, no financial performance claims, no unsupported provider claims.
- Publish known limitations and support channel.

### Stage C — Limited beta
- Open only after read-only MT5 integration and Controller/Risk decision-record path pass staging.
- Enforce quotas, acceptable-use policy, incident response and support hours.
- Monitor activation, repeat use, connection failure and support burden.
- Pause new signups if security, reliability or cost thresholds are breached.

### Stage D — General availability
- Only after all production release gates pass.
- Publish service terms, privacy policy, risk disclosure, support policy, availability status and release notes.
- Any later Demo execution capability requires separate product scope and approval; real-money trading is a separate authorization.

## 4. Channel strategy

### YouTube — education and trust
Content pillars:
1. MT5 and market structure education.
2. SMC and multi-timeframe analysis concepts.
3. AI-assisted trading analysis and limitations.
4. Risk management and common trading mistakes.
5. NOVA product demonstrations using verified live UI and clearly labeled Demo data.
6. Engineering transparency: how decisions are checked and why NOVA sometimes says WAIT.

Cadence hypothesis: one useful long-form tutorial weekly, supported by short clips. Validate production capacity before committing to a public schedule.

### TikTok / short video
- Repurpose educational segments, not profit screenshots.
- 20–60 second explainers: chart concepts, risk mistakes, AI limitations, feature demonstrations.
- Clear CTA to the landing page/waitlist.
- Do not publish fabricated P&L or imply signals are financial advice.

### Website / SEO
- Landing page, feature pages, product status, supported integrations, documentation, FAQ, privacy, terms, risk disclosure and support.
- Publish searchable educational articles around MT5, SMC, risk controls and AI trading analysis.
- Track organic landing-page conversion and search intent.

### Community
- Create a moderated tester group only when support capacity exists.
- Use onboarding guides, weekly feedback threads and known-issues updates.
- No paid signal promises, investment advice or unmoderated customer account sharing.

### Partnerships
- Approach educators and trading communities for product demos and research feedback.
- Use written disclosures and referral terms.
- Do not imply broker endorsement or partnership without a signed agreement.

## 5. 90-day launch experiment

| Period | Work | Measurement |
|---|---|---|
| Days 1–30 | Brand, landing page, product explainer, educational content, interviews and waitlist | Qualified visits, waitlist conversion, interview themes |
| Days 31–60 | Private preview after security gates; guided onboarding and feedback | Signup-to-activation, MT5 Demo connection success, first analysis completion |
| Days 61–90 | Limited beta if staging gates pass; case studies with consent | Weekly active users, repeat analysis, retention, support tickets, cost per active user |

These are experiment windows, not a promise of product readiness or public launch dates.

## 6. Funnel and analytics

1. Awareness: educational videos, articles, community demos.
2. Interest: landing page and feature documentation.
3. Intent: waitlist/demo request.
4. Activation: verified signup and first completed analysis.
5. Retention: repeat weekly analysis and dashboard usage.
6. Referral: opt-in testimonials and community invitations.

Track:
- Landing sessions and source/medium.
- Waitlist conversion and consent rate.
- Signup completion and verification.
- Time to first useful analysis.
- MT5 Demo connection verification rate and failure reasons.
- Weekly active users and cohort retention.
- Analysis completion/error rates and latency.
- Support tickets per active user.
- Infrastructure cost per active user and per analysis.
- Cancellation/deletion requests and complaint rate.

Do not collect broker passwords, raw secrets, or unnecessary financial data for marketing analytics.

## 7. Pricing hypotheses — not approved pricing

Start with a free research preview or capped free tier to validate usage and operating cost. Consider later tiers only after measuring actual inference, storage, support and hosting costs.

Potential packaging:
- **Free / Preview:** capped AI analysis, education, limited workspace, Demo read-only where supported.
- **Trader:** higher usage quota, more analysis history and research tools.
- **Research:** backtest/evaluation tools only when those tools are validated and available.
- **Community / Team:** multiple seats and workspace administration after tenant roles and billing controls are implemented.

Do not publish prices until cost model, quotas, support cost, tax/payment requirements and terms are known. No paid billing service is to be provisioned without owner approval.

## 8. Marketing governance and compliance

- Clearly state that trading involves risk and losses are possible.
- Distinguish education, analysis and software features from individualized investment advice or broker services.
- Obtain legal review for Thai and target-market financial promotion, privacy, consumer protection and subscription terms before public commercial launch.
- Obtain consent for testimonials, recordings, analytics and email campaigns.
- Keep performance claims reproducible and reviewed.
- Maintain correction/removal process for inaccurate public claims.

## 9. Launch readiness checklist

- [ ] Product features shown on the website are implemented and tested.
- [ ] Screenshots and demo data are genuine or explicitly labeled.
- [ ] Privacy policy, terms, risk disclosure and support policy reviewed.
- [ ] Waitlist consent and unsubscribe/delete workflow works.
- [ ] Analytics are privacy-minimized.
- [ ] Support staffing and escalation process are defined.
- [ ] Known limitations and service status are public.
- [ ] No guaranteed-profit, win-rate or unsupported automation claims.
- [ ] Product security and production release gates pass before public beta.
