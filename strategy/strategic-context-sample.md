---
date: 2026-03-08
product: Family Wallet
phase: Post-MVP Analysis & Strategic Planning
prepared_by: Product Team
---

# 📊 Strategic Context - Family Wallet (March 2026)

## Current Situation Summary

Family Wallet is a mobile app for family finance management (launched Q4 2025). After 3 months in market, we've hit critical inflection points: user acquisition is declining (-29%), paid conversion is falling (-40%), and churn is rising (8.2% vs 5.1% target). However, feature adoption (Pocket Money module) is increasing (+5pp), indicating product-market fit in at least one user segment.

**Key Metrics (Feb 2026):**
- DAU: 8,450 (Target: 10,000) | MRR: $33,075 (Target: $52,500)
- D7 Retention: 42% (Target: 50%) | Churn: 8.2% (Target: <5%)
- Paid Conversion: 2.1% (Target: 3.5%) | Paid Subs: 945 (Target: 1,500)

---

## 1. VISION & MISSION

**Product Vision:**
To become Poland's leading digital platform for family finance management, enabling families to achieve financial security and teach children healthy money habits through a single, trusted app.

**Mission:**
Simplify family finances by integrating bank accounts, expense tracking, savings goals, and financial education in one place — making it easy for parents to manage budgets and for children to learn financial literacy.

---

## 2. BUSINESS GOALS (OKRs - 2026)

### Q1 2026 (COMPLETED)
- Objective: Launch MVP with core features and establish product-market fit signals
- Key Results:
  - ✅ Launch with 3 bank integrations (mBank, PKO, ING)
  - ✅ Reach 10,000 monthly active users
  - ⚠️ Achieve 3.5% paid conversion (actual: 2.1%)
  - ⚠️ Establish D7 retention of 50% (actual: 42%)

### Q2 2026 (CURRENT - April-June)
- Objective: Fix activation funnel and scale paid subscriptions
- Key Results:
  - DAU: 15,000+ (recover from current 8,450)
  - Paid Conversion: 4.0% (from current 2.1%)
  - Paid Subscriptions: 1,500+ (from current 945)
  - D7 Retention: 52%+ (from current 42%)
  - MRR: $52,500+ (from current $33,075)

### Q3-Q4 2026 (ASPIRATIONAL)
- Objective: Build sustainable moat through ecosystem and partnerships
- Key Results:
  - Expand to 5 markets (Poland, Czech, Slovakia, Hungary, Romania)
  - Establish 3 financial institution partnerships
  - Build educator partnerships (10+ schools using Pocket Money module)
  - Reach $100k+ MRR (2,000+ paid subscriptions)

---

## 3. SITUATION ANALYSIS (SWOT)

### STRENGTHS
- **Product differentiation:** Only integrated family finance + education platform in CEE region
- **Pocket Money module traction:** +5pp adoption (23% → 28%) despite overall metrics declining — signals real user need
- **Technical foundation:** Secure API integrations, scalable architecture (AWS)
- **Team experience:** Product Manager (FinTech), Lead Dev (Banking systems), Design (UX accessibility)
- **Market timing:** Rising parental interest in children's financial education post-pandemic

### WEAKNESSES
- **Acquisition engine broken:** 29% drop in signups post-iOS update suggests product quality issues
- **UI/UX friction:** 3 negative reviews citing "confusing UI," session duration 43% below target
- **Technical debt:** 2.1% crash rate (2x target), Pocket Money load time 2.8s (87% over target)
- **Marketing dependency:** Conversion collapsed when marketing paused — sign of weak organic funnel
- **Limited market presence:** No brand awareness outside early adopters; competing against Nest Family (launched Feb 20)

### OPPORTUNITIES
- **Financial education trend:** Schools seeking tools to teach financial literacy (B2B partnerships)
- **Geographic expansion:** CEE market underserved; 3 new markets (Czech, Slovakia, Hungary) have similar demographics
- **Regulatory tailwinds:** EU PSD2 and open banking regulations creating data access advantages
- **Feature ecosystem:** Cross-sell potential (investment recommendations, loan aggregation, family budgeting)
- **Strategic partnerships:** Banks looking for customer engagement tools; could integrate Family Wallet into their apps

### THREATS
- **Increased competition:** Nest Family launched Feb 20; other FinTech startups entering family finance space
- **Regulation:** PSD2 compliance costs; potential GDPR complexity with children's data
- **Market saturation:** If competitors move fast, window for market leadership closes in 12-18 months
- **User churn from poor experience:** Feb's 8.2% churn could accelerate if issues not fixed by Q2
- **Economic sensitivity:** Family budgeting apps could see lower engagement if economy worsens

---

## 4. MARKET & USER LANDSCAPE

### Target Market (CEE Region)
- **Total Addressable Market (TAM):** ~15M families across Poland, Czech, Slovakia with internet access and bank accounts
- **Serviceable Available Market (SAM):** ~3M families interested in financial management + children's education
- **Serviceable Obtainable Market (SOM):** 100k families by end of 2026 (3.3% market penetration)

### User Personas & Jobs to be Done

**Parent (Anna, 38, Warsaw)**
- Job to be Done: "I want to see all my family's finances in one place and teach my kids the value of money without the hassle of manually tracking everything"
- Current pain: Scattered accounts (3 banks), no visibility into children's spending, limited time for financial education
- Willingness to pay: $35-50/month for convenience + peace of mind

**Child (Kasia, 12)**
- Job to be Done: "I want to understand how much money I have and save for things I want without constantly asking my parents"
- Current pain: Loses cash, doesn't understand spending limits, bored with lectures about money
- Engagement trigger: Gamification, visual progress toward savings goals

**Financial Advisor (Robert, 42, Prague)**
- Job to be Done: "I want a tool to show clients organized family finances and make recommendations without them manually sending me statements"
- Current pain: Clients provide incomplete/disorganized data; difficult to give holistic advice
- Willingness to pay: Willing to recommend if it improves client experience

### Competitive Landscape

**Direct Competitors:**
- Nest Family (launched Feb 2026) — family budgeting app, strong in US, now entering EU
- YNAB (You Need A Budget) — US-based, good tracking, not family-focused, no bank integrations in CEE

**Indirect Competitors:**
- Traditional banking apps (mBank, PKO) — have transaction views but no family features
- Google Family Link — free, focused on parental controls, not financial education
- Excel spreadsheets — what families currently use for budget planning

**Competitive Advantage:**
- Only CEE-first platform designed for family + education
- Integrated bank data (not manual entry)
- Pocket Money module (unique, growing traction)
- Teacher/school partnerships potential (B2B moat)

---

## 5. STRATEGIC ROADMAP (Q2-Q4 2026)

### Q2 2026 — FIX ACTIVATION FUNNEL (April-June)
**Theme:** Recover user acquisition and conversion from Feb/Mar trough

**Initiatives:**
1. **iOS Stability Sprint** — Fix crashes (2.1% → <1%), redesign confusing onboarding flows
   - Owner: Engineering Lead
   - Timeline: 2 weeks (by April 15)
   - Success metric: Crash rate <1%, session duration restored to 7m+

2. **Paid Conversion Optimization** — A/B test pricing tiers, simplified payment flow, free trial extension
   - Owner: Growth PM
   - Timeline: 3 weeks (launch by May 1)
   - Success metric: Conversion rate 3.5%+

3. **Czech Market Deep-dive** — Investigate 12.1% churn, localize UI, run exit interviews
   - Owner: Product Manager
   - Timeline: Ongoing (weekly reviews)
   - Success metric: Czech churn <8% by end of Q2

4. **Pocket Money Performance** — Optimize load time to <1.5s, add gamification (streak tracking, badges)
   - Owner: Engineering + Design
   - Timeline: 4 weeks (by May 15)
   - Success metric: Load time <1.5s, adoption 40%+

**Expected Output:** DAU 12,000+, Conversion 3.5%+, MRR $42,000+

---

### Q3 2026 — EXPAND MARKET (July-September)
**Theme:** Launch in Czech Republic and Slovakia, establish school partnerships

**Initiatives:**
1. **Czech Market Launch** — Localized app, local bank integrations, Czech-language support
   - Owner: Product + Regional Lead
   - Timeline: 8 weeks
   - Success metric: 5,000 MAU in Czech Republic by Q3 end

2. **Education Partnerships** — Pilot Pocket Money in 5 schools, gather feedback, build curriculum
   - Owner: Partnerships Lead
   - Timeline: 6 weeks (launch by August)
   - Success metric: 500+ students using Pocket Money via school programs

3. **Advisor Console Beta** — Launch financial advisor dashboard (white-label for banks/advisors)
   - Owner: Lead Developer
   - Timeline: 10 weeks
   - Success metric: 10+ advisors in beta testing

**Expected Output:** 25,000 DAU across 2 markets, partnerships foundation

---

### Q4 2026 — BUILD ECOSYSTEM (October-December)
**Theme:** Establish strategic partnerships and prepare for 2027 scaling

**Initiatives:**
1. **Bank Partnership Program** — Integrate with 3 regional banks (white-label, co-marketing)
   - Owner: Partnerships + Growth
   - Timeline: 12 weeks
   - Success metric: 1 signed partnership, 3 in advanced talks

2. **Financial Product Integrations** — Add investment recommendations, loan aggregation
   - Owner: Product Manager
   - Timeline: 10 weeks
   - Success metric: Users can view crypto, investment accounts alongside bank data

3. **2027 Roadmap + Fundraising** — Prepare Series A pitch deck, expand team, plan regional expansion
   - Owner: CEO + Product
   - Timeline: Ongoing
   - Success metric: Series A fundraising initiated

**Expected Output:** Partnerships in place, $100k+ MRR trajectory, team expanded

---

## 6. STRATEGIC DECISIONS (What & What NOT)

### IN SCOPE (What WE Do)
- ✅ Family-focused features (not individual finance)
- ✅ Bank aggregation (not investment products initially)
- ✅ Financial education for children (Pocket Money primary focus)
- ✅ CEE market first (Poland, Czech, Slovakia, Hungary, Romania)
- ✅ SaaS subscription model (Freemium + Premium tiers)
- ✅ B2B partnerships (schools, financial advisors, banks)

### OUT OF SCOPE (What WE Don't Do)
- ❌ Investment management (not for children, regulatory complexity)
- ❌ Crypto/alternative assets (regulatory risk, out of scope until 2027)
- ❌ Lending products (not licensed)
- ❌ US/Western Europe markets (competitive, later phase)
- ❌ Mortgage/real estate tools (different user need, can add later)
- ❌ B2C marketing to advisors (B2B partnerships only, no consumer ads)

### RATIONALE
- **Why family-first?** Differentiation vs. individual apps; Pocket Money shows this is real need
- **Why CEE?** Underserved market, higher growth potential, regulatory advantage
- **Why subscriptions?** Sustainable business model; Freemium allows acquisition at scale
- **Why partnerships?** Faster market penetration than direct acquisition; builds defensibility

---

## 7. RISKS & MITIGATION

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|-----------|
| **Nest Family steals market share** | HIGH | HIGH | Launch Czech/Slovakia fast (Q2); establish school partnerships (B2B moat) |
| **Regulatory crackdown on kids' data** | MEDIUM | HIGH | Legal review complete by April; GDPR-first compliance; no ads targeting children |
| **iPhone market dominance prevents Android growth** | MEDIUM | MEDIUM | Android app stability by May; equal feature parity with iOS |
| **Bank API changes break integrations** | LOW | HIGH | Diversify integrations; build fallback manual entry; monitor PSD2 regulations |
| **Team burnout from rapid scaling** | MEDIUM | MEDIUM | Hire 2 engineers by May; prioritize roadmap ruthlessly; quarterly planning sessions |
| **User churn accelerates if Q2 fixes fail** | MEDIUM | HIGH | Weekly churn monitoring; exit surveys; quick pivot to different features if needed |

---

## 8. SUCCESS METRICS & VALIDATION

### Scenario 1: Q2 Activation Funnel Recovery
```gherkin
Given DAU is 8,450 and paid conversion is 2.1% as of March 2026
When we complete iOS stability sprint and paid conversion optimization by May 2026
Then DAU should reach 12,000+ and paid conversion should reach 3.5%+ by end of Q2
```

### Scenario 2: Czech Market Churn Reduction
```gherkin
Given Czech Republic churn rate is 12.1% (vs 8.2% overall average) as of Feb 2026
When we run exit interviews, localize UI, and implement Czech-specific fixes by May 2026
Then Czech churn rate should drop below 8% by end of Q2 2026
```

### Scenario 3: Pocket Money Module Growth
```gherkin
Given Pocket Money adoption is 28% and load time is 2.8s as of March 2026
When we optimize load time to <1.5s and add gamification features by May 15
Then Pocket Money adoption should reach 40%+ and become the primary retention driver
```

---

## ✅ Definition of Ready (DoR)

- [x] Current situation and key metrics documented
- [x] Vision & Mission defined
- [x] Business Goals (OKRs) set for Q1-Q4 2026
- [x] SWOT Analysis complete
- [x] Market & User Landscape documented (TAM/SAM/SOM + Personas)
- [x] Competitive Landscape mapped
- [x] Strategic Roadmap Q2-Q4 with owners and timelines
- [x] Strategic Decisions (IN/OUT scope) with rationale
- [x] Risk register with mitigation plans
- [x] Success Metrics validated in Gherkin format

---

## 🔗 Powiązane

- Framework: `skill:strategy-framework`
- Komenda: `/plan-strategy`
- Metryki: `/analytics/`
- Discovery: `/discovery/`
- Decyzje: `/logs/`
