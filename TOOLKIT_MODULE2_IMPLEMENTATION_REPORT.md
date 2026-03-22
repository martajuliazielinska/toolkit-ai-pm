# Module 2: AI-Powered PM Command Center

**Author:** Marta Zielińska
**Date:** March 10, 2026
**Project:** Toolkit PM — Academic Case Study
**Repository:** `toolkit-pm-example/` | `.claude/` configuration

---

## Executive Summary

Module 2 extends Claude Code from a User Story generator (Module 1) into a **full-stack PM Command Center** — an AI-native workflow that transforms raw product data into structured strategic artifacts.

**What was built:**
- **3 custom commands** wired to the Claude Code CLI (`/analyze-metrics`, `/plan-strategy`, `/synthesize-feedback`)
- **3 reusable skill frameworks** enforcing consistent output structure across all PM deliverables
- **641 lines of production-ready automation output** generated in a single session

**Case study:** [Family Wallet](./analytics/product-metrics-sample.md) — a CEE-market fintech product managing family finances and children's allowances. All three commands were tested against real-format data: February 2026 KPI report, strategic context, and 409 user feedback data points.

**Core insight:** A PM equipped with this system can go from raw metrics to a full 9-month strategic roadmap — including documented decisions, hypothesis validation, and user feedback synthesis — in one structured session.

---

## Architecture Overview

```
toolkit-pm-example/
│
├── .claude/
│   ├── CLAUDE.md                               ← Project brain: routing rules, folder map
│   │
│   ├── commands/                               ← CLI-invokable PM workflows
│   │   ├── analyze-competitors.md              [Module 1]
│   │   ├── create-user-story.md                [Module 1]
│   │   ├── analyze-metrics.md                  [Module 2] ← NEW
│   │   ├── plan-strategy.md                    [Module 2] ← NEW
│   │   └── synthesize-feedback.md              [Module 2] ← NEW
│   │
│   └── skills/                                 ← Structured output frameworks
│       ├── user-story-template-jira.md         [Module 1]
│       ├── skill-metrics-analysis-framework.md [Module 2] ← NEW
│       ├── skill-strategy-framework.md         [Module 2] ← NEW
│       └── skill-feedback-synthesis.md         [Module 2] ← NEW
│
├── analytics/          ← KPI data, metric reports, trend analysis
├── strategy/           ← Product vision, roadmaps, strategic docs
├── discovery/          ← Competitive research, user feedback, synthesis
├── backlog/            ← User Stories, Epics, requirements
├── execution/          ← Process templates, how-to guides
└── logs/               ← Decision history, meeting notes, session logs
```

**Data flow:**

```
[Raw Input]              [Command]                [Skill/Framework]       [Output]
────────────────────────────────────────────────────────────────────────────────────
product-metrics.md   →  /analyze-metrics      →  metrics-framework   →  /analytics/
strategic-context    →  /plan-strategy        →  strategy-framework  →  /strategy/
                                                                      →  /logs/
user-feedback.md     →  /synthesize-feedback  →  feedback-synthesis  →  /discovery/
```

**Design principle:** Commands handle *routing* (where to look, where to save). Skills handle *structure* (what sections to produce). This separation makes each skill reusable across multiple commands without duplication.

---

## Commands & Skills Built

### Commands (5 total across Module 1 + 2)

| Command | Module | Function | Data Source | Output Location |
|---------|--------|----------|-------------|-----------------|
| `/analyze-competitors` | M1 | Competitor mapping vs. product strategy | `/discovery/` | `/discovery/` |
| `/create-user-story` | M1 | Jira-standard User Story with Gherkin AC | Epic description | `/backlog/` |
| `/analyze-metrics` | M2 | KPI analysis: trends, anomalies, root causes, recommendations | `/analytics/` | `/analytics/` |
| `/plan-strategy` | M2 | Vision, OKRs, SWOT, Q1–Q4 roadmap, logged decisions | `/strategy/` + `/analytics/` | `/strategy/` + `/logs/` |
| `/synthesize-feedback` | M2 | Group feedback into themes, pain points, product insights | `/discovery/` | `/discovery/` |

### Skills / Frameworks (4 total)

| Skill | Module | Sections | Output Quality Gates |
|-------|--------|----------|----------------------|
| `skill:user-story-template-jira` | M1 | 6 | Context, Benefits, Scope, NRF, Acceptance Criteria (Gherkin), DoR |
| `skill:metrics-analysis-framework` | M2 | 7 | Current State, Trends, Anomalies, Root Cause, Recommendations, Data Quality, Validation (Gherkin) |
| `skill:strategy-framework` | M2 | 8 | Vision, OKRs, SWOT, Market Landscape, Roadmap, Decisions IN/OUT, Risk Matrix, Success Metrics (Gherkin) |
| `skill:feedback-synthesis` | M2 | 8 | Sources, Thematic Grouping, Pain Points, User Quotes, Product Insights, Recommendations, Metrics Correlation, Validation (Gherkin) |

**Total coverage:** 5 commands + 4 skills = **29 analytical sections** accessible via natural language.

Every skill includes a **Definition of Ready (DoR)** checklist — a quality gate that enforces completeness before the output is considered production-ready.

---

## Test Results & Outputs

All three Module 2 commands were tested against Family Wallet data from February 2026.

---

### Test 1 — `/analyze-metrics`

**Input:** [`analytics/product-metrics-sample.md`](./analytics/product-metrics-sample.md)
**Output:** [`analytics/metrics-analysis-2026-03-08.md`](./analytics/metrics-analysis-2026-03-08.md) — **147 lines**
**Framework used:** `skill:metrics-analysis-framework`

**KPI snapshot generated (Feb 2026 vs. targets):**

| Metric | Actual | Target | Gap |
|--------|--------|--------|-----|
| DAU | 8,450 | 10,000 | -15% |
| D7 Retention | 42% | 50% | -8pp |
| Paid Conversion | 2.1% | 3.5% | -40% |
| Churn Rate | 8.2% | <5% | +3.2pp ❌ |
| MRR | $33,075 | $52,500 | **-$19,425/month** |
| App Crash Rate | 2.1% | <1% | +1.1pp ❌ |

**4 root cause hypotheses generated with confidence ratings:**
- **Hypothesis A** (HIGH): iOS v2.1 release (Feb 15) broke the acquisition funnel — crash rate 2× target, signup drop started on exact release day
- **Hypothesis B** (HIGH): Marketing pause (Feb 18) killed top-of-funnel — paid vs. organic conversion differential identified
- **Hypothesis C** (MEDIUM): Nest Family competitor EU launch (Feb 20) drove churn — Czech Republic at 12.1% churn, highest in portfolio *(later revised by feedback data — see Test 3)*
- **Hypothesis D** (MEDIUM): Pocket Money load time (2.8s vs. <1.5s target) blocking conversion from engaged users

**Financial projection included:** crash fix + marketing restart by Mar 15 → estimated MRR recovery to ~$42,000 (+$9k vs. current).

---

### Test 2 — `/plan-strategy`

**Input:** [`strategy/strategic-context-sample.md`](./strategy/strategic-context-sample.md) + Test 1 output
**Outputs:**
- [`strategy/strategy-2026-03-08.md`](./strategy/strategy-2026-03-08.md) — **~200 lines**
- [`logs/decision-2026-03-08.md`](./logs/decision-2026-03-08.md) — **112 lines, 6 decisions**

**Framework used:** `skill:strategy-framework`

**Product vision defined:**
> *Become the leading digital platform for family financial management in the CEE region — enabling families to achieve financial security and teaching children healthy money habits through one trusted app.*

**Q2–Q4 2026 roadmap generated:**

| Quarter | Theme | Key Initiatives | Target MRR |
|---------|-------|-----------------|-----------|
| Q2 (Apr–Jun) | **Fix** | iOS stability sprint → marketing restart (gated on crash <1%), Pocket Money performance, Czech deep-dive | $52,500+ |
| Q3 (Jul–Sep) | **Expand** | Czech + Slovakia market launch, school partnership pilot (5 schools, 500+ students), Advisor Console beta | — |
| Q4 (Oct–Dec) | **Ecosystem** | Bank partnership program, financial product integrations, Series A preparation | $100,000+ |

**6 strategic decisions logged** with explicit alternatives considered and rejection rationale:

| Decision | Verdict | Key Rationale |
|----------|---------|---------------|
| Marketing restart timing | Gate on crash rate <1% | Paying for broken UX compounds brand damage |
| Czech Republic priority | Q2 investigation, not Q3 | 12.1% churn = systemic signal, not an edge case |
| Pocket Money positioning | Elevated to core product pillar | Only growing metric during February crisis (+5pp MoM) |
| Geographic expansion path | CEE first: CZ → SK → HU → RO | CAC in Western Europe 3–5× higher; Nest Family weaker in CEE |
| Investment/crypto features | Out of scope until 2027 | Regulatory complexity + user segment mismatch |
| Pricing model | SaaS Freemium stays; A/B test tiers in Q2 | Model isn't broken — value perception is |

**4 success scenarios in Gherkin format** (Q2 activation recovery, paid conversion improvement, Pocket Money conversion bridge, Czech stabilization).

---

### Test 3 — `/synthesize-feedback`

**Input:** [`discovery/user-feedback-sample.md`](./discovery/user-feedback-sample.md) — 409 feedback data points across 7 sources
**Output:** [`discovery/feedback-synthesis-2026-03-08.md`](./discovery/feedback-synthesis-2026-03-08.md) — **294 lines**
**Framework used:** `skill:feedback-synthesis`

**Feedback sources mapped:**

| Source | Volume | Period | Segment |
|--------|--------|--------|---------|
| App Store iOS (negative) | 31 reviews | Feb 2026 (post-v2.1) | Parents 29–52 |
| App Store iOS (positive) | 16 reviews | Jan–Feb 2026 | Pocket Money power users |
| App Store Android | 12 reviews | Jan–Feb 2026 | Stable user base |
| User interviews — churned | 8 sessions | Feb 24 – Mar 5 | Ex-users |
| User interviews — retained | 5 sessions | Mar 1–6 | Pocket Money heavy users |
| Support tickets | 134 tickets | Feb 2026 | 89 crash / 28 CZ / 12 UI / 5 billing |
| In-app survey (churn risk trigger) | 203 responses | Feb 15–28 | 3 days without session |

**Top 5 pain points with business impact:**

| # | Pain Point | Severity | Frequency | Revenue Impact |
|---|-----------|---------|-----------|----------------|
| 1 | iOS v2.1 crash (login, payments, Pocket Money load) | CRITICAL | 52% survey + 89 tickets | Direct cause of -29% signups, -40% conversion |
| 2 | "Confusing UI" — navigation redesign removed Pocket Money shortcuts | HIGH | 23% survey + 12 tickets | Session duration -43%; users churn rather than re-learn |
| 3 | Czech localization: missing bank integrations | HIGH | 100% of Czech tickets (28) | CZ churn 12.1% — blocks entire CEE expansion path |
| 4 | Pocket Money load time 2.8s (87% above 1.5s target) | MED→HIGH | 5 retained interviews | "Pocket Money Paradox": adoption up, conversion flat |
| 5 | Premium support SLA: 5-day response vs. 24h expectation | MEDIUM | ~15 data points | Premium churn driven by support failure, not product |

**16 verbatim user quotes** collected. Selected examples:

> *"My son (11) is now literally proud of his 'account'. No dinner-table conversation about money ever worked as well as this app."*
> — Beata N., ★★★★★, App Store, Jan 12

> *"I spent 20 minutes looking for things that used to be one tap away."*
> — Marta W., ★★☆☆☆, App Store, Feb 18

> *"Pocket Money works great, but without bank integration it's not a complete app."*
> — Lucie K., Ostrava, Support Ticket #3056 (Czech user)

**5 product insights** generated with confidence ratings. Most significant:

> **Insight 3 (HIGH confidence): Czech churn is an infrastructure problem, not a competitive one.**
> Hypothesis C from Test 1 assumed Nest Family caused Czech churn. Feedback data falsifies this: Nest Family mentioned only 6/203 times. 100% of Czech support tickets are about missing bank integrations (Česká spořitelna, Komerční banka, Moneta). Strategic implication: Czech launch requires local bank integrations *before* launch — not after. This changes the Q3 timeline.

**DO NOT DO section** (explicit product anti-decisions):
- Do not restart marketing before crash fix
- Do not launch Czech Republic without local bank integrations
- Do not keep icon-only navigation — 40+ segment is the core paying cohort
- Do not ignore Ticket #3001 — potential double-charge = regulatory and chargeback risk

---

## Key Learnings

**1. Commands route; skills structure**
Commands don't contain analytical logic — they tell Claude where to find data, what framework to apply, and where to save the output. Skills deliver the section structure. This separation allows skills to be reused across multiple commands without duplication and makes each piece independently replaceable.

**2. Frameworks enforce completeness**
Without a framework, AI output is inconsistent — good sections appear randomly based on input quality. The Metrics Analysis Framework mandates 7 sections including Data Quality and Gherkin validation. Every output produced under the framework is comparable across sessions, teams, and products.

**3. AI can revise its own hypotheses**
The most technically significant moment in this module: Test 3 (Feedback Synthesis) produced data that directly falsified Hypothesis C from Test 1 (Metrics Analysis). The metrics suggested Nest Family triggered Czech churn. The feedback showed Nest Family mentioned 6/203 times — statistically insignificant. The synthesis document updated the strategic recommendation accordingly. **This is what iterative PM reasoning looks like in an AI-native system.**

**4. Cross-document linking creates a living knowledge graph**
Each output document references its sources and downstream documents explicitly. `feedback-synthesis-2026-03-08.md` links to `metrics-analysis-2026-03-08.md`, `strategy-2026-03-08.md`, and `decision-2026-03-08.md`. Any stakeholder can trace how a strategic decision evolved from raw data through hypothesis to validated recommendation.

**5. "DO NOT DO" is a first-class artifact**
Traditional PM documentation captures what to build. The feedback framework mandates explicit documentation of what *not* to build — with rationale. This is the section most frequently missing in real-world product specs and most frequently responsible for wasted sprint capacity.

---

## What's Next — Module 3

Module 2 proved the system can run a complete PM analytical cycle: raw data → hypotheses → strategy → feedback → strategy revision. The next logical step is closing the loop from analysis to implementation tickets — and eliminating manual handoff between pipeline stages.

### Options for Module 3

**Option A: End-to-End Pipeline (Recommended)**
Chain all four commands without manual output copying between steps. One input prompt triggers the full analytical pipeline:

```
/analyze-metrics → /plan-strategy → /synthesize-feedback → /create-user-story
```

Success criteria: a single `@product-metrics.md` reference produces a complete backlog-ready User Story without intermediate manual steps.

**Option B: Real Data Integration**
Connect live product data exports (Mixpanel, Amplitude, Jira) instead of sample files. Build an ingestion layer that normalizes external formats into the command-readable structure.

**Option C: Automated Weekly PM Reporting**
A `/weekly-report` command that triggers the full pipeline on a schedule — KPI delta report, updated strategy flags, and surfaced feedback anomalies delivered as a structured Markdown digest.

**Option D: Multi-Product Workspace**
Extend the architecture to support multiple products sharing skills but maintaining isolated data folders. Enables a single PM or small team to run parallel product tracks within one Claude Code workspace.

---

## Output File Index

| File | Lines | Type | Generated By |
|------|-------|------|-------------|
| [`analytics/metrics-analysis-2026-03-08.md`](./analytics/metrics-analysis-2026-03-08.md) | 147 | KPI Analysis | `/analyze-metrics` |
| [`strategy/strategy-2026-03-08.md`](./strategy/strategy-2026-03-08.md) | ~200 | Product Strategy | `/plan-strategy` |
| [`logs/decision-2026-03-08.md`](./logs/decision-2026-03-08.md) | 112 | Decision Log | `/plan-strategy` |
| [`discovery/feedback-synthesis-2026-03-08.md`](./discovery/feedback-synthesis-2026-03-08.md) | 294 | Feedback Synthesis | `/synthesize-feedback` |
| [`logs/MODULE-2-RECONSTRUCTION-GUIDE.md`](./logs/MODULE-2-RECONSTRUCTION-GUIDE.md) | 810 | Setup Guide | Manual |
| **`TOOLKIT_MODULE2_IMPLEMENTATION_REPORT.md`** | **~350** | **This document** | **Manual** |
| **Module 2 total** | **~1,913** | | |

---

*Marta Zielińska — Product Manager*
*Toolkit PM · Module 2 · March 10, 2026*
