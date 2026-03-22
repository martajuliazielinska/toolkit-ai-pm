# Module 3: AI-Powered Risk Discovery System

**Author:** Marta Zielińska
**Date:** March 14, 2026
**Project:** Toolkit PM — Academic Case Study
**Repository:** `toolkit-pm-example/` | `.claude/` configuration

---

## Executive Summary

Module 3 closes the gap between strategy and safe implementation by teaching the system to **ask the hard questions before development begins** — not after a production incident.

**What was built:**
- **1 custom command** (`/detect-missing-requirements`) with two analysis modes: full unhappy path analysis and edge-cases-only
- **1 reusable risk framework** (`skill:risk-analysis-framework`) covering 4 failure categories × 3 impact dimensions
- **1 domain dictionary** (`domain/dictionary.md`) as the single source of truth for all risk analysis
- **129 documented risk scenarios** across 2 processes and 8 analysis runs — with a demonstrated path from generic to actionable

**Case study:** Family Wallet — same product from Module 2. Two processes analyzed: Financial Consultation booking flow (8 steps) and Pocket Money lifecycle (10 steps). The system was tested iteratively: 4 runs on Financial Consultation, 3 runs on Pocket Money, plus cross-reference deduplication runs on both.

**Core insight:** The quality of AI risk analysis is a direct function of dictionary completeness. With a partial dictionary: scaffolding and hedged recommendations. With a full dictionary: specific numbers, exact thresholds, and actionable mechanisms. This module proves it — with before/after evidence.

---

## Architecture Overview

```
toolkit-pm-example/
│
├── .claude/
│   ├── CLAUDE.md                                   ← Project brain (updated: Module 3 principles added)
│   │
│   ├── commands/
│   │   ├── analyze-competitors.md                  [Module 1]
│   │   ├── create-user-story.md                    [Module 1]
│   │   ├── analyze-metrics.md                      [Module 2]
│   │   ├── plan-strategy.md                        [Module 2]
│   │   ├── synthesize-feedback.md                  [Module 2]
│   │   └── detect-missing-requirements.md          [Module 3] ← NEW
│   │
│   └── skills/
│       ├── user-story-template-jira.md             [Module 1]
│       ├── skill-metrics-analysis-framework.md     [Module 2]
│       ├── skill-strategy-framework.md             [Module 2]
│       ├── skill-feedback-synthesis.md             [Module 2]
│       └── risk-analysis-framework.md              [Module 3] ← NEW
│
├── domain/
│   └── dictionary.md                               [Module 3] ← NEW (domain truth source)
│
├── strategic-tasks/
│   ├── test_process_financial_consultation.md      [Module 3] ← process input
│   ├── test_process_pocket_money.md                [Module 3] ← process input
│   └── risks/
│       ├── unhappy_paths/                          [Module 3] ← full analysis outputs
│       │   ├── financial_consultation_2026-03-14.md
│       │   ├── financial_consultation_v2_2026-03-14.md
│       │   ├── financial_consultation_v3_2026-03-14.md
│       │   ├── financial_consultation_v4_2026-03-14.md
│       │   ├── financial_consultation_crossref_2026-03-14.md
│       │   ├── pocket_money_2026-03-14.md
│       │   ├── pocket_money_crossref_2026-03-14.md
│       │   └── pocket_money_v3_2026-03-14.md
│       └── edge_cases/                             ← ready for targeted analysis
│
├── project-structure.md                            [Module 3] ← NEW (naming conventions)
├── decisionLog.md                                  [Module 3] ← UPDATED
└── TOOLKIT_MODULE3_IMPLEMENTATION_REPORT.md        ← This document
```

**Data flow:**

```
[Process Input]                    [Command]                        [Framework]              [Output]
──────────────────────────────────────────────────────────────────────────────────────────────────────
process_financial_consultation  →  /detect-missing-requirements  →  risk-analysis-framework  →  risks/unhappy_paths/
process_pocket_money            →  /detect-missing-requirements  →  risk-analysis-framework  →  risks/unhappy_paths/
  + @dictionary (domain truth)      (+ cross-reference constraint)
```

**Design principle:** The command handles *input parsing, constraint resolution, and output routing*. The skill handles *analytical structure, category enforcement, and quality gating*. The dictionary is an independent layer neither owns — it is the shared ground truth that both reference. This three-layer separation makes the system composable: swap the dictionary, change the domain.

---

## Commands & Skills Built

### Module 3 Additions

| Component | Type | Function | Input | Output Location |
|-----------|------|----------|-------|-----------------|
| `/detect-missing-requirements` | Command | Identify unhappy paths, edge cases, missing requirements per process step | Process file + `@dictionary` | `strategic-tasks/risks/unhappy_paths/` or `edge_cases/` |
| `skill:risk-analysis-framework` | Skill | 4-category risk analysis × 3-level impact assessment | Process steps | Structure A (full) or B (edge-cases only) |
| `domain/dictionary.md` | Domain truth | Single source for roles, policies, formulas, thresholds | Referenced by all commands and skills | Versioned in repo |

### Command: `/detect-missing-requirements`

**3 input modes:**

| Mode | Syntax | Use case |
|------|--------|----------|
| Inline | `/detect-missing-requirements` + process description inline | Quick analysis of a new process |
| File reference | `/detect-missing-requirements for process in @process.md` | Structured process files |
| Edge-cases only | `/detect-missing-requirements edge-cases-only for @process.md` | Targeted Business Logic pass only |

**Cross-reference constraint** (Module 3 innovation): add `dla scenariuszy innych niż w pliku @existing-analysis.md` to use an existing analysis as negative-space input — generating a complementary non-duplicating second pass over the same process.

### Skill: `risk-analysis-framework`

**4 failure categories:**

| Category | What it captures |
|----------|-----------------|
| User Errors | Mistakes users make that the product should prevent or handle gracefully |
| System/Technical Errors | Failures the system can produce independently of user action |
| External Dependencies | Third-party services, APIs, payment gateways, regulatory bodies |
| Business Logic & Edge Cases | Race conditions, boundary values, policy ambiguities, unusual-but-valid states |

**3-level impact assessment per scenario:** User impact → System impact → Business impact

**Definition of Ready (DoR) quality gate:** 7 mandatory checklist items including `@dictionary` loaded, all 4 categories covered, specific recommendations with numbers (no "TBD"), and zero unverified assumptions.

---

## Test Results & Outputs

All runs executed against Family Wallet processes. Date: 2026-03-14.

---

### Test 1 — Financial Consultation: 4-Run Iteration

**Process:** [`strategic-tasks/test_process_financial_consultation.md`](./strategic-tasks/test_process_financial_consultation.md) — 8 steps (Browse Advisor → Select → Time Slot → Payment → Confirmation → Notification → Consultation → Post-Consultation)

**Methodology:** Each run deepens the analysis without duplicating previous scenarios. Dictionary grows between runs.

| Run | File | Dictionary state | Scenarios | Depth |
|-----|------|-----------------|-----------|-------|
| #1 | `financial_consultation_2026-03-14.md` | Not loaded | 19 | Generic fintech risks |
| #2 | `financial_consultation_v2_2026-03-14.md` | Sections 2–3 loaded | 13 | Policy-specific scenarios |
| #3 | `financial_consultation_v3_2026-03-14.md` | Sections 2–3, gaps closed | 12 | Second-order policy edge cases |
| #4 | `financial_consultation_v4_2026-03-14.md` | Sections 2–3, full | 10 | Third-order consequences |
| Cross-ref | `financial_consultation_crossref_2026-03-14.md` | Sections 2–3 full | 19 | Constraint: exclude Run #1 |
| **Total** | | | **73** | |

**Selected scenarios from Run #4 (third-order consequences — only possible with full dictionary):**

> *Scenario: Advisor joins at T+14 min — below no-show threshold, but user receives only 46 of 60 booked minutes with no compensation mechanism.*
>
> Dictionary: no-show triggers at >15 min. Advisor joins at T+14. Policy not triggered — but user waited 14 minutes and receives only 46 of 60 booked minutes. Technically compliant; no action taken. Business: user feels wronged with no recourse; likely to churn and leave a negative review.
>
> *Recommendation:* Apply Late Join Credit: `(minutes_missed / booked_duration) × session_price_paid` = `(14/60) × session_price`. Prevents advisors exploiting the sub-15-minute window.

> *Scenario: Session ends at exactly 50% duration — boundary condition on partial refund threshold.*
>
> Dictionary: session ends before 50% of booked duration → partial refund. 60-minute session ends at exactly 30 minutes (50.0%). "Before 50%" strictly = `< 50%`, so 30/60 = 50.0% does not trigger.
>
> *Recommendation:* Document threshold as `< 50%` (strictly less than). Add explicit unit tests for 29/60, 30/60, 31/60 minute scenarios against refund logic.

**Cross-reference demonstration** — same process, zero duplication, deeper layer:

| Run #1 scenario (no dictionary) | Cross-ref equivalent (full dictionary) |
|---|---|
| Timezone display bug in slot selection | Timezone bug in cancellation deadline: Czech user cancels at T-23h Prague time; server registers T-25h Warsaw → 25% fee applied |
| Payment gateway timeout | Penalty (10% of session fee) falls below gateway minimum floor for 15 PLN introductory sessions |
| Video link not generated at confirmation | Credit issued for late join — no "My Credits" UI surface; expires after 90 days silently |

---

### Test 2 — Pocket Money: Gap-Close Loop

**Process:** [`strategic-tasks/test_process_pocket_money.md`](./strategic-tasks/test_process_pocket_money.md) — 10 steps (Parent Setup → Allowance Trigger → Task Assignment → Child Completes → Parent Approval → Reward Distribution → Child Spending → Savings Goal → Goal Achievement → Transaction History)

**Methodology:** Run #1 identifies dictionary gaps. Cross-ref runs with gaps open show cost of missing definitions. Section 4 added to dictionary. Run #3 closes all gaps — zero hedging.

| Run | File | Dictionary state | Scenarios | Hedged (⚠️) |
|-----|------|-----------------|-----------|-------------|
| #1 | `pocket_money_2026-03-14.md` | Sections 2–3 | 22 | 0 |
| Cross-ref | `pocket_money_crossref_2026-03-14.md` | Sections 2–3 (6 gaps open) | 18 | 4 |
| #3 | `pocket_money_v3_2026-03-14.md` | Sections 2–4 full | 16 | 0 |
| **Total** | | | **56** | |

**6 dictionary gaps identified in Run #1 — all closed before Run #3:**

| Gap | Section added |
|-----|--------------|
| Child account balance ceiling (undefined) | Section 4: max 1,000 PLN, 0 PLN floor, ceiling breach options |
| Spending mechanics ("no external payments yet" is not a definition) | Section 4: Phase 1 internal ledger, Wishlist Request model |
| Task lifecycle states (open/in_progress/submitted/approved/rejected/expired/cancelled) | Section 4: full state machine + rejection override rule |
| Savings goal extension limit | Section 4: max 2 extensions per goal; 3rd → adjust or abandon |
| Goal auto-unlock SLA | Section 4: 7 days; reminders T+1, T+3, T+7 |
| Transaction history retention for rejection reason text | Section 4: auto-deleted at 90 days; event record permanent |

**The cost of open gaps — direct comparison:**

| With full dictionary (Run #3) | With gaps open (cross-ref) |
|---|---|
| "Allowance job pre-checks: if balance + amount > 1,000 PLN → partial to ceiling, queue remainder, notify parent at next login" | "⚠️ Requires dictionary gap closure: balance ceiling undefined." |
| "On granting the 2nd extension, inform parent: 'This is the last extension. Extensions used: 2 of 2 — final.'" | "⚠️ Requires dictionary gap closure: savings goal extension limit undefined." |
| "At task creation, check: current_balance + reward > 1,000 PLN? Soft warning at creation — eliminate the surprise at approval." | "⚠️ Requires dictionary gap closure: task review SLA undefined." |

**Selected high-value scenario from Run #3:**

> *Scenario: 100% of balance ring-fenced for savings goal — child sees full balance but cannot spend any of it.*
>
> Child has 200 PLN total balance and sets a savings goal of 200 PLN → 100% ring-fenced. Opens Wishlist Request screen; displayed balance: 200 PLN. Effective spending balance: 0 PLN. Hard block.
>
> *Recommendation:* Display two distinct balance figures: "Total: 200 PLN | Available to spend: 0 PLN (200 PLN locked in savings goal)." Soft warning at goal creation if it would reduce spendable balance to 0 PLN.

**Cumulative output — 129 total scenarios, 8 runs, 2 processes:**

| Process | Runs | Total scenarios | Zero-dupe guarantee |
|---------|------|----------------|---------------------|
| Financial Consultation | 5 | 73 | ✓ cross-reference constraint enforced |
| Pocket Money | 3 | 56 | ✓ cross-reference constraint enforced |
| **Total** | **8** | **129** | |

---

## Key Learnings

**1. Dictionary completeness is the only input variable that matters**

Module 2 proved that framework structure determines output consistency. Module 3 proves something more fundamental: framework structure is necessary but insufficient. The limiting factor is domain knowledge loaded into the dictionary. The same command, same skill, same process → completely different output quality based solely on how much of the domain is defined. This is a testable, repeatable, demonstrable claim — proven across 8 runs.

**2. Hedging is a diagnostic, not a failure**

The ⚠️ marker in the cross-reference run is not a bug in the system — it is the system correctly identifying where it cannot be specific. Scaffolding ("define this first") is the honest output when a definition is missing. The ⚠️ count is a direct measure of dictionary completeness. Four ⚠️ marks in the Pocket Money cross-ref run translated into exactly 6 dictionary sections to write — and zero ⚠️ marks in Run #3.

**3. Cross-reference deduplication enables multi-layer analysis of the same process**

The same 8-step process can yield 73 distinct, non-duplicating scenarios across 5 analysis passes. This is not an artifact of prompt engineering — it reflects the genuine complexity of a real product domain. The cross-reference constraint (`dla scenariuszy innych niż w pliku @existing.md`) operationalizes multi-pass analysis by using existing work as negative-space input. Each pass goes one layer deeper.

**4. Third-order consequences require both dictionary depth and iterative runs**

Run #1 finds the obvious risks. Run #2 finds the policy-specific risks. Run #3 finds the boundary conditions. Run #4 finds the consequences of the policies designed to address the earlier risks — the edge cases of the safety net itself. The boundary condition scenario (`< 50%` vs `<= 50%` on partial refunds) only appears at Run #4 because it requires knowing both the policy and its exact implementation language. Dictionary-grounded analysis enables this; generic brainstorming does not.

**5. Risk analysis is Definition of Ready enforcement, not documentation overhead**

Every scenario with a specific recommendation is a requirement that doesn't exist yet in the product spec. The 129 scenarios are not risk documentation — they are *pre-backlog*. The correct downstream step is not filing them in a risks folder and forgetting them: it is converting high-severity scenarios into User Stories with Gherkin acceptance criteria. Module 4 closes this loop.

---

## What's Next — Module 4

Module 3 produces 129 scenarios. The system currently has no automated path from scenario → backlog. Every scenario that becomes a production bug was, at some earlier point, a findable risk scenario. Module 4 builds the bridge.

### Options for Module 4

**Option A: Risk → User Story Pipeline (Recommended)**
Chain `/detect-missing-requirements` directly into `/create-user-story`. High-severity scenarios automatically generate backlog-ready User Stories with Gherkin acceptance criteria. The risk scenario becomes the "unhappy path" AC.

```
/detect-missing-requirements → [severity filter] → /create-user-story (unhappy path mode)
```

Success criteria: one `/detect-missing-requirements` run produces a set of User Stories covering the critical failure paths, ready for sprint planning without manual reformatting.

**Option B: Risk Scoring Matrix**
Extend the `risk-analysis-framework` skill with severity × probability scoring per scenario. Output: a prioritized risk register in MoSCoW format. Enables the PM to triage 129 scenarios into "must fix before launch" vs. "monitor post-launch."

**Option C: Acceptance Criteria Generator**
Convert risk scenarios directly into Gherkin test cases (`Given / When / Then / But`). The unhappy path scenario becomes a failing test case — handed directly to QA or automated testing infrastructure.

**Option D: Living Dictionary Loop**
Build a `/update-dictionary` command that extracts domain definitions implied by new User Stories or risk scenarios and proposes additions to `domain/dictionary.md`. Makes the dictionary self-improving across sessions rather than manually maintained.

---

## Output File Index

| File | Lines (approx.) | Type | Generated By |
|------|----------------|------|-------------|
| [`domain/dictionary.md`](./domain/dictionary.md) | ~90 | Domain truth | Manual + iterative |
| [`project-structure.md`](./project-structure.md) | ~40 | Naming conventions | Manual |
| [`strategic-tasks/risks/unhappy_paths/financial_consultation_2026-03-14.md`](./strategic-tasks/risks/unhappy_paths/financial_consultation_2026-03-14.md) | ~320 | Risk analysis | `/detect-missing-requirements` |
| [`strategic-tasks/risks/unhappy_paths/financial_consultation_v2_2026-03-14.md`](./strategic-tasks/risks/unhappy_paths/financial_consultation_v2_2026-03-14.md) | ~240 | Risk analysis | `/detect-missing-requirements` |
| [`strategic-tasks/risks/unhappy_paths/financial_consultation_v3_2026-03-14.md`](./strategic-tasks/risks/unhappy_paths/financial_consultation_v3_2026-03-14.md) | ~220 | Risk analysis | `/detect-missing-requirements` |
| [`strategic-tasks/risks/unhappy_paths/financial_consultation_v4_2026-03-14.md`](./strategic-tasks/risks/unhappy_paths/financial_consultation_v4_2026-03-14.md) | ~190 | Risk analysis | `/detect-missing-requirements` |
| [`strategic-tasks/risks/unhappy_paths/financial_consultation_crossref_2026-03-14.md`](./strategic-tasks/risks/unhappy_paths/financial_consultation_crossref_2026-03-14.md) | ~250 | Risk analysis (cross-ref) | `/detect-missing-requirements` |
| [`strategic-tasks/risks/unhappy_paths/pocket_money_2026-03-14.md`](./strategic-tasks/risks/unhappy_paths/pocket_money_2026-03-14.md) | ~290 | Risk analysis | `/detect-missing-requirements` |
| [`strategic-tasks/risks/unhappy_paths/pocket_money_crossref_2026-03-14.md`](./strategic-tasks/risks/unhappy_paths/pocket_money_crossref_2026-03-14.md) | ~250 | Risk analysis (cross-ref) | `/detect-missing-requirements` |
| [`strategic-tasks/risks/unhappy_paths/pocket_money_v3_2026-03-14.md`](./strategic-tasks/risks/unhappy_paths/pocket_money_v3_2026-03-14.md) | ~350 | Risk analysis | `/detect-missing-requirements` |
| [`decisionLog.md`](./decisionLog.md) | ~10 | Decision log | Manual |
| **`TOOLKIT_MODULE3_IMPLEMENTATION_REPORT.md`** | **~320** | **This document** | **Manual** |
| **Module 3 total** | **~2,570** | | |

---

*Marta Zielińska — Product Manager*
*Toolkit PM · Module 3 · March 14, 2026*
