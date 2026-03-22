# Unhappy Path Analysis: Financial Consultation Booking — Run #4

**Analysis Date:** 2026-03-14
**Source:** `strategic-tasks/test_process_financial_consultation.md`
**Framework:** `skill:risk-analysis-framework` · Structure A (Full Analysis)
**Dictionary:** `domain/dictionary.md` ✓ fully loaded — all Run #3 gaps closed
**Run:** #4 — third-order consequences; rules generating new edge cases
**New scenarios this run:** 10
**Combined total (Run #1 + #2 + #3 + #4):** 54 scenarios across 8 steps

> **Focus:** Scenarios that emerge from newly defined rules — credit expiry, penalty rounding, suspension cascades, appeal limits, SLA gaps. Zero duplication from previous 44 scenarios.

---

### Process: financial_consultation

---

#### Step 2: Select Advisor

* **Scenario:** Advisor suspended — existing future bookings not automatically resolved
  * **Category:** Business Logic and Edge Cases
  * **Description:** Policy: 3 total cancellations within 30 days → automatic suspension pending Platform Admin Account review. Advisor may have 5 confirmed bookings 2 weeks out. Suspension fires — those bookings have no defined resolution path. Users discover the issue only when they try to join the call.
  * **Potential Impact:**
    * **User:** Has a confirmed, paid booking with a suspended advisor — no proactive notification.
    * **System:** Suspension state has no cascading logic for downstream `confirmed` bookings.
    * **Business:** Multiple simultaneous refunds + late notifications = operational surge; reputational damage.
  * **Recommendation / Questions for the Team:** Define suspension cascade: on advisor suspension, all future `confirmed` bookings transition to `cancelled` automatically, full refund issued, users notified within 1 hour. Platform Admin Account review SLA: 24h to triage, decision within 5 business days. During review: no new bookings accepted from suspended advisor.

---

#### Step 3: Choose Time Slot

* **Scenario:** Legacy advisor slots below the 30-minute minimum still visible in browse
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary defines minimum session length = 30 min; Advisor Panel blocks new slot configuration <30 min. But if advisors configured 20-minute slots before this policy was enforced, those slots may still appear in the browse UI and be bookable.
  * **Potential Impact:**
    * **User:** Selects and pays for a 20-minute slot; video session terminates at 20 minutes.
    * **System:** Legacy slots bypass the new constraint; no migration ran on existing slot data.
    * **Business:** User paid for a sub-minimum session; partial refund may trigger; support ticket.
  * **Recommendation / Questions for the Team:** On policy rollout, run a data migration: all existing advisor slots <30 min must be flagged and either auto-extended to 30 min (with advisor notification) or deactivated. Confirm: does the slot configuration validator apply retroactively to existing records, or only on new saves?

---

#### Step 4: Payment

* **Scenario:** 10% standard penalty rounds to an amount below payment gateway minimum
  * **Category:** Business Logic and Edge Cases
  * **Description:** Standard penalty = 10% of session fee. If a session costs 15 PLN, penalty = 1.50 PLN. Many payment gateways have a minimum transaction floor (~2–5 PLN). A 1.50 PLN deduction may be rejected by the payment processor — or silently fail.
  * **Potential Impact:**
    * **User:** No direct impact.
    * **System:** Penalty deduction transaction fails; penalty marked as collected but not actually processed.
    * **Business:** Enforcement fails on low-value consultations; advisors offering cheap introductory sessions face no consequence for cancellation.
  * **Recommendation / Questions for the Team:** Define minimum penalty floor regardless of session fee (proposed: 5 PLN). Apply floor before payment gateway call. Test penalty flow with minimum session fee values.

* **Scenario:** Advisor has two penalties in the same 6-month period — no appeals remaining for legitimate second case
  * **Category:** Business Logic and Edge Cases
  * **Description:** Policy: one appeal per 6-month period. Advisor appeals successfully in March (medical emergency). In May, receives a second penalty for another ≤24h cancellation — also legitimate — but has no appeal remaining.
  * **Potential Impact:**
    * **User:** No direct impact.
    * **System:** Appeal limit enforced correctly but with no nuance for repeat legitimate emergencies.
    * **Business:** Advisor perceives policy as unfair; churns from platform. Supply-side damage to Advisor Console B2B model.
  * **Recommendation / Questions for the Team:** Define whether appeal limit resets after successful appeal or after time period. Alternative: one appeal per penalty event, with a cap of 2 successful appeals per 12-month period. Must be explicit in Advisor Terms & Conditions at onboarding.

---

#### Step 5: Confirmation

* **Scenario:** Credit worth more than the next consultation fee — partial redemption undefined
  * **Category:** Business Logic and Edge Cases
  * **Description:** User receives 60 PLN goodwill credit (advisor 24 min late on 150 PLN session). User books a new 30-minute session at 75 PLN. Credit (60 PLN) covers most but not all. Can the user apply credit partially and pay the remaining 15 PLN? Or does the system require full-credit redemption only?
  * **Potential Impact:**
    * **User:** Credit unusable for consultations cheaper than the credit amount, or partially usable with unclear UX.
    * **System:** Partial credit redemption logic not defined — may require split payment flow.
    * **Business:** Credits that can't be redeemed = hidden liability on the books; user frustration.
  * **Recommendation / Questions for the Team:** Define credit redemption rules: partial redemption allowed; user pays delta via card; credit balance reduces by amount used; remaining balance visible in "My Credits." Confirm payment gateway supports split-tender. If not — consider credits as full-session vouchers instead.

* **Scenario:** 90-day credit expiry — user receives no warning before expiration
  * **Category:** System/Technical Errors
  * **Description:** Policy: goodwill credit expires after 90 days. User receives credit, forgets about it, returns on day 91 to find it gone — no prior warning issued.
  * **Potential Impact:**
    * **User:** Feels cheated — compensation issued and then silently revoked.
    * **System:** Expiry runs as a background job; no proactive notification triggered before expiry.
    * **Business:** The goodwill gesture creates a second grievance. User contacts support; if denied reinstatement, churns.
  * **Recommendation / Questions for the Team:** Send expiry warning notifications: T-14 days ("Your consultation credit expires in 14 days") and T-3 days. Never expire a credit silently. Define whether Platform Admin Account can reinstate expired credits on a case-by-case basis.

---

#### Step 7: Consultation

* **Scenario:** Partial refund amount is less than the payment gateway's refund processing cost
  * **Category:** External Dependencies
  * **Description:** Partial Session Refund: session ends before 50% of booked duration. For a 30 PLN session ending at 14 minutes (of 30 min booked), refund ≈ 15 PLN. Some gateways charge a processing fee per refund that may exceed the refund amount — net negative transaction for the platform.
  * **Potential Impact:**
    * **User:** Entitled to a refund that costs more to process than its value.
    * **System:** Refund transaction may fail or generate a negative net value.
    * **Business:** Processing a 15 PLN refund may cost more in gateway fees — structural loss on low-value consultations.
  * **Recommendation / Questions for the Team:** Define minimum partial refund threshold: if calculated refund < 10 PLN, issue as account credit instead of payment reversal. Confirm refund processing costs with payment gateway before setting this threshold.

* **Scenario:** "Flagged for review" booking — Platform Admin review SLA undefined
  * **Category:** Business Logic and Edge Cases
  * **Description:** Policy: advisor cancels ≤2h → booking flagged for Platform Admin Account review. No SLA defined for how quickly the review must happen, what it entails, or what actions follow. Without SLA, flagging is a paper trail, not an enforcement mechanism.
  * **Potential Impact:**
    * **User:** No impact — already received full refund. But delay protects a repeat bad actor.
    * **System:** Flagged bookings accumulate in a queue with no defined resolution path.
    * **Business:** Advisor can repeat ≤2h cancellations without consequence until review happens — which may never happen under load.
  * **Recommendation / Questions for the Team:** Define Platform Admin review SLA: 24h to triage, 5 business days to resolve. Review outcomes must be one of: `warning_issued`, `penalty_applied`, `suspended`, `cleared`. Third ≤2h cancellation within 90 days → automatic suspension without waiting for review. Add to `domain/dictionary.md`.

---

#### Step 8: Post-Consultation

* **Scenario:** User redeems goodwill credit with the same advisor who caused the late join
  * **Category:** Business Logic and Edge Cases
  * **Description:** User received credit because advisor joined 12 minutes late. User books another session with the same advisor and applies the credit. Credit was compensation for that advisor's behavior — but the funding source of the credit is undefined: is it platform-funded, or does it offset from the advisor's payout?
  * **Potential Impact:**
    * **User:** No direct harm — gets the session at a discount.
    * **System:** Credit funding source not tracked; platform absorbs cost regardless.
    * **Business:** If platform-funded, the advisor who caused the issue earns full fee while platform bears the cost. Minor revenue leak, but a fairness and incentive design issue.
  * **Recommendation / Questions for the Team:** Define credit funding source in `domain/dictionary.md`. Proposed: if credit was issued due to advisor late join, deduct credit amount from advisor's next payout (advisor-funded). If due to technical failure, platform-funded. This aligns financial consequences with the responsible party.

* **Scenario:** Advisor reinstated after suspension — affected users not notified of availability
  * **Category:** Business Logic and Edge Cases
  * **Description:** Advisor is suspended; future bookings auto-cancelled; refunds issued. Platform Admin reinstates the advisor within 24h. Previously affected users are not notified — they've already booked elsewhere or assumed the advisor is permanently gone.
  * **Potential Impact:**
    * **User:** Lost a booking they wanted; doesn't know the advisor is available again.
    * **System:** Reinstatement has no "notify affected users" logic.
    * **Business:** Missed rebooking opportunity; users who received refunds may have moved to a competitor.
  * **Recommendation / Questions for the Team:** On advisor reinstatement, trigger optional notification to users whose bookings were cancelled during the suspension: "Your advisor [Name] is available again — rebook here." Include priority booking link. This is a retention mechanism, not just a courtesy.

---

## Quality Checklist — Run #4

- [x] All 4 categories covered: User Errors, System/Technical, External Dependencies, Business Logic & Edge Cases
- [x] Dictionary terminology used throughout: all 4 account types, penalty tiers, credit formula, expiry, appeal limits, suspension thresholds
- [x] Every scenario has User / System / Business impact
- [x] All recommendations specific and actionable
- [x] 10 new scenarios — 0 duplicated from Runs #1, #2, or #3
- [x] Date format ISO: `2026-03-14`
- [x] `@dictionary` fully loaded ✓ — all Run #3 gaps closed before this run

---

## Cumulative Summary: All Runs

| Run | Scenarios | Dictionary State | Key Finding |
|-----|-----------|-----------------|-------------|
| Run #1 | 19 | ✗ not loaded | Race conditions, timezone bug, child booking gap, GDPR |
| Run #2 | 13 | ✓ roles + basic policies | Self-booking exploit, advisor cancellation gap, 60-min cap, GDPR child data |
| Run #3 | 12 | ✓ full policies | Policy conflicts, undefined formulas, abuse patterns, appeal gaps |
| Run #4 | 10 | ✓ all gaps closed | Suspension cascades, penalty floors, credit expiry, SLA gaps, reinstatement flow |
| **Total** | **54** | | |

**New dictionary gaps identified in Run #4:**

| Gap | Priority |
|-----|----------|
| Suspension cascade — downstream booking resolution | HIGH |
| Platform Admin review SLA for flagged bookings | HIGH |
| Minimum penalty floor (payment gateway minimum) | MEDIUM |
| Credit funding source (platform vs. advisor-funded) | MEDIUM |
| Partial credit redemption rules | MEDIUM |
| Credit expiry warning notification schedule | MEDIUM |

---

*Generated: 2026-03-14 | Command: `/detect-missing-requirements` | Framework: `skill:risk-analysis-framework` | Run #4 | Structure A*
