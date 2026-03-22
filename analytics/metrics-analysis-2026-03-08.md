---
date: 2026-03-08
reporting_period: February 2026
product: Family Wallet
analyst: Claude (AI PM Assistant)
source: analytics/product-metrics-sample.md
---

# Metrics Analysis - Family Wallet - February 2026

## 1. Current State — KPI vs Target

| Metric | Actual | Target | Delta | Status |
|--------|--------|--------|-------|--------|
| DAU | 8,450 | 10,000 | -15% | ⚠️ |
| MAU | 45,230 | 50,000 | -9% | ⚠️ |
| New Signups | 3,200 | 4,500 | -29% | ⚠️ |
| D7 Retention | 42% | 50% | -8pp | ⚠️ |
| Paid Conversion | 2.1% | 3.5% | -40% | ⚠️ |
| Paid Subscriptions | 945 | 1,500 | -37% | ⚠️ |
| Pocket Money Adoption | 28% | 60% | -53% | ⚠️ |
| App Rating | 4.2 | 4.5+ | -0.3 | ⚠️ |
| Churn Rate | 8.2% | <5% | +3.2pp | ❌ |
| App Crash Rate | 2.1% | <1% | +1.1pp | ❌ |
| Avg Session Duration | 4m 32s | 8m+ | -43% | ❌ |
| Feature Load Time | 2.8s | <1.5s | +87% | ❌ |

**MRR Gap: -$19,425/month** (Actual $33,075 vs Target $52,500)

---

## 2. Trends (Feb vs Jan 2026)

| Metric | Jan 2026 | Feb 2026 | Change |
|--------|----------|----------|--------|
| DAU | 9,600 | 8,450 | ↓ -12% |
| MAU | 45,100 | 45,230 | → FLAT |
| New Signups | 4,500 | 3,200 | ↓ -29% |
| D7 Retention | 50% | 42% | ↓ -8pp |
| Paid Conversion | 3.6% | 2.1% | ↓ -1.5pp |
| Churn Rate | 5.1% | 8.2% | ↑ +3.1pp |
| Pocket Money Adoption | 23% | 28% | ↑ +5pp |

**Summary:**
- Everything critical is declining — acquisition, retention, conversion
- Only Pocket Money adoption is growing (but not translating to conversion)
- MAU flat despite declining DAU → users less active, not churning instantly

---

## 3. Anomalies

### 🔴 Critical
1. **Signup cliff on Feb 15** — 29% drop correlates exactly with iOS v2.1 release
2. **Conversion cliff on Feb 18** — paid conversion dropped 1.5pp in one week, correlates with marketing pause
3. **Churn spike** — 8.2% vs 5.1% in January (+61% increase), Czech Republic worst at 12.1%

### 🟡 Notable
4. **Pocket Money paradox** — feature adoption is rising (+5pp MoM) while overall conversion is falling; users engage with the feature but don't convert to paid
5. **MAU/DAU decoupling** — MAU flat while DAU drops 12%; existing users becoming less engaged but not leaving yet (leading indicator of future churn)

---

## 4. Root Cause Analysis

### Hypothesis A: iOS v2.1 broke the acquisition funnel (HIGH confidence)
- Signup drop started exactly on Feb 15 (release day)
- Crash rate at 2.1% (2x target) — new users hit crashes and abandon
- 3 negative App Store reviews citing "confusing UI" post-update
- Session duration 4m32s vs 8m+ target — users aren't completing onboarding flows

**Test:** Compare signup conversion rate before/after Feb 15 in funnel analytics; check crash logs for new user sessions.

### Hypothesis B: Marketing pause killed top-of-funnel (HIGH confidence)
- Marketing paused Feb 18, conversion cliff same week
- Without paid acquisition, organic traffic likely lower intent
- Lower-intent users convert less → conversion rate drops

**Test:** Compare paid vs organic conversion rates; check traffic source breakdown Feb 15-28.

### Hypothesis C: Competitor launch triggered churn (MEDIUM confidence)
- Nest Family launched Feb 20
- Churn spike aligns with this date
- Czech Republic highest churn (12.1%) — may indicate Nest Family launched there first or has stronger localization

**Test:** Exit survey data; check if Czech churn spiked specifically post Feb 20.

### Hypothesis D: Pocket Money performance kills conversion (MEDIUM confidence)
- Load time 2.8s vs <1.5s target (87% over target)
- Users try the feature (adoption up), get frustrated by speed, don't convert
- Explains the paradox: engagement up, conversion down

**Test:** Funnel analysis from Pocket Money feature engagement → paid conversion; A/B test fast vs slow load users.

---

## 5. Recommendations

### Immediate (This Week)

| Priority | Action | Owner | Expected Impact |
|----------|--------|-------|----------------|
| P0 | Hotfix iOS v2.1 crash issues | Engineering | Unblock new user acquisition |
| P0 | Restart marketing spend | Growth | Restore top-of-funnel volume |
| P1 | Investigate Czech Republic localization | PM + Design | Stop 12.1% churn market |
| P1 | Optimize Pocket Money load time to <1.5s | Engineering | Unlock conversion from engaged users |

### Short-term (Next 2 Weeks)

- Run exit interviews with February churned users (target: 50 interviews)
- A/B test simplified onboarding flow to address "confusing UI" reviews
- Set up competitor monitoring for Nest Family feature parity tracking
- Review iOS v2.1 UI changes — consider partial rollback of UI changes while keeping bug fixes

### Metrics to Watch (Next 30 Days)

- Daily signup rate: recovery to >150/day (Feb avg was ~114/day)
- Crash rate: must reach <1% before next marketing spend increase
- Czech churn: target <8% by end of March
- Pocket Money load time: <1.5s as gate for conversion push

---

## 6. Validation Requirements

Before acting on recommendations, confirm:
- [ ] Crash logs confirm new user sessions are affected by v2.1 crashes
- [ ] Funnel data shows drop at acquisition step (not awareness)
- [ ] Marketing spend was fully paused Feb 18 (not gradual reduction)
- [ ] Czech negative reviews / exit surveys mention Nest Family or localization
- [ ] Pocket Money load time measured on real devices (not just staging)

---

## Financial Projection

If crash fix + marketing restart happen by Mar 15:
- Estimated signup recovery: +40% (conservative, returns to Jan level)
- Estimated conversion recovery: +1.0pp (to ~3.1%, still below 3.5% target)
- Projected MRR end of March: ~$42,000 (+$9k vs current, -$10k vs target)

Full target recovery (1,500 subs / $52,500 MRR) is not realistic before Q2 without addressing churn AND conversion simultaneously.

---

*Generated: 2026-03-08 | Source: analytics/product-metrics-sample.md | Framework: metrics-analysis-framework*
