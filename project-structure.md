# Project Structure Standards — Family Wallet

This document defines canonical paths and naming conventions for all deliverables.

---

## Competitive Intelligence Artifacts

**Directory:** `discovery/competitors/`

| Deliverable | Path | Notes |
|---|---|---|
| Competitor Analysis | `discovery/competitors/analysis_{date}.md` | From `/analyze-competitors` command |

---

## Strategic Planning Artifacts

**Directories:** `strategy/`, `logs/`

| Deliverable | Path |
|---|---|
| Product Strategy | `strategy/strategy_{date}.md` |
| Decision Log | `logs/decision_{date}.md` |
| Product Roadmap | `strategy/roadmap_{period}.md` |

---

## Business Analytics Artifacts

**Directory:** `analytics/`

| Deliverable | Path |
|---|---|
| Metrics Analysis | `analytics/metrics-analysis_{date}.md` |

---

## Product Documentation Artifacts

**Directory:** `discovery/`

| Deliverable | Path |
|---|---|
| Feedback Synthesis | `discovery/feedback-synthesis_{date}.md` |
| User Stories | `backlog/{feature_name}_{date}.md` |

---

## Risk Analysis Artifacts — MODULE 3

**Directory:** `strategic-tasks/risks/`

| Deliverable | Path | Notes |
|---|---|---|
| Unhappy Path Analysis | `strategic-tasks/risks/unhappy_paths/{process_name}_{date}.md` | Complete risk analysis: User Errors, Technical Errors, External Dependencies, Business Logic |
| Edge Cases Only | `strategic-tasks/risks/edge_cases/{process_name}_{date}.md` | Focused analysis: only Business Logic & Edge Cases scenarios |

---

## Naming Conventions

| Variable | Format | Examples |
|---|---|---|
| `{date}` | ISO `YYYY-MM-DD` | `2026-03-14` |
| `{period}` | time period slug | `2026_q2`, `h2_2026` |
| `{process_name}` | snake_case | `family_onboarding`, `financial_consultation` |
| `{feature_name}` | snake_case | `pocket_money_module`, `bank_integration` |
