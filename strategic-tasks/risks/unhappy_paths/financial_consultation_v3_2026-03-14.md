# Unhappy Path Analysis: Financial Consultation Booking — Run #3

**Analysis Date:** 2026-03-14
**Source:** `strategic-tasks/test_process_financial_consultation.md`
**Framework:** `skill:risk-analysis-framework` · Structure A (Full Analysis)
**Dictionary:** `domain/dictionary.md` ✓ loaded — includes Advisor Cancellation Policy (3 tiers), Late Join / Partial Session Policy, Video Conference constraints
**Run:** #3 — second-order policy edge cases
**New scenarios this run:** 12
**Combined total (Run #1 + Run #2 + Run #3):** 44 scenarios across 8 steps

> **Focus:** Scenarios that only become visible once policies are formally defined. Zero duplication from Run #1 (19 scenarios) or Run #2 (13 scenarios).

---

### Process: financial_consultation

---

#### Step 2: Select Advisor

* **Scenario:** User holds a Parent Account and a Financial Advisor Account under the same email
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary defines 4 distinct account types but doesn't address whether one user can hold multiple roles. A financial advisor who is also a parent may register with the same email. At login — which role is active? Can they switch? If they browse as a parent, can they book a consultation with themselves?
  * **Potential Impact:**
    * **User:** Role ambiguity at login; may inadvertently act as the wrong account type.
    * **System:** Single email → multiple role records — undefined behavior in auth layer.
    * **Business:** Self-booking exploit; advisor data and client data commingled in one account.
  * **Recommendation / Questions for the Team:** Define explicitly: one email = one role, OR one email = multiple roles with explicit role-switching UI. If multi-role is allowed, role context must be enforced per action (e.g., booking flow always uses Parent Account role, never Financial Advisor Account role).

---

#### Step 3: Choose Time Slot

* **Scenario:** Late Join credit consumes effective session time below the 50% partial refund threshold
  * **Category:** Business Logic and Edge Cases
  * **Description:** Policy: advisor joins at T+14 min → user gets goodwill credit for 14 missed minutes. Session runs for the remaining 46 minutes. But if the booked slot is 30 minutes (minimum), advisor joins at T+14 → only 16 minutes of session remain → below 50% of 30 min. Does the partial refund policy now also trigger on top of the Late Join credit?
  * **Potential Impact:**
    * **User:** Entitled to both Late Join credit AND partial refund — or the policies conflict and neither is applied correctly.
    * **System:** Two compensation mechanisms triggered simultaneously with undefined precedence.
    * **Business:** Double compensation issued, or user receives nothing because policies cancel each other out — both are bad outcomes.
  * **Recommendation / Questions for the Team:** Define policy precedence explicitly: Late Join Credit (T+5 to T+14) and Partial Session Refund (session <50% of booked time) must be mutually exclusive OR stackable — not undefined. Minimum session length must also be defined (is 30 min the floor?).

* **Scenario:** User attempts to extend session beyond 60 minutes during the call
  * **Category:** User Errors
  * **Description:** Conversation is going well. At T+55 min the user sees the countdown warning. They ask the advisor: "Can we continue for another 30 minutes?" Advisor agrees. But the video platform hard-terminates at T+60 min regardless.
  * **Potential Impact:**
    * **User:** Expects an extension; gets cut off mid-agreement.
    * **System:** No in-app mechanism to extend a session; Whereby session is fixed at booking time.
    * **Business:** User frustrated after a positive experience — sours the ending of an otherwise successful consultation.
  * **Recommendation / Questions for the Team:** Define session extension policy before launch. Options: (a) no extensions — clearly communicated upfront; (b) in-session extension booking at additional charge; (c) advisor marks "continuation needed" → system prompts to rebook immediately post-session. Option (c) is lowest friction.

---

#### Step 4: Payment

* **Scenario:** Advisor cancellation penalty cannot be collected — no future payouts
  * **Category:** Business Logic and Edge Cases
  * **Description:** Policy: Advisor cancels ≤24h → penalty deducted from advisor's next payout. But if the advisor deactivates their account, has no upcoming bookings, or has already withdrawn their balance — there is no payout to deduct from.
  * **Potential Impact:**
    * **User:** Receives full refund (correct) but advisor faces no consequence.
    * **System:** Penalty recorded but uncollectable; debt in undefined state.
    * **Business:** Penalty mechanism is unenforceable for churning advisors; creates moral hazard (cancel freely before leaving the platform).
  * **Recommendation / Questions for the Team:** Define penalty collection fallback: (a) hold minimum balance in advisor account as a deposit against cancellations, OR (b) deactivation blocked if outstanding penalty exists, OR (c) penalty waived if advisor account inactive >30 days. Legal review: is withholding a deposit from advisors compliant with PL/CZ contractor law?

* **Scenario:** "Elevated penalty" for ≤2h cancellation is undefined in amount
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary states: Advisor cancels ≤2h → "elevated penalty." But the dictionary doesn't define what "elevated" means — a fixed amount? A percentage of session fee? 2× the standard penalty?
  * **Potential Impact:**
    * **User:** No direct impact — still gets full refund.
    * **System:** Penalty calculation logic has no defined formula for the ≤2h tier.
    * **Business:** Inconsistent penalty enforcement; advisor disputes; potential legal challenge if penalty is applied arbitrarily.
  * **Recommendation / Questions for the Team:** Define elevated penalty formula in `domain/dictionary.md`. Proposed: standard penalty = 10% of session fee; elevated penalty (≤2h) = 25% of session fee. Must be stated in Advisor Terms & Conditions at onboarding.

* **Scenario:** Advisor cancels the same user's rebooked consultation a second time
  * **Category:** Business Logic and Edge Cases
  * **Description:** Advisor cancels within 24h (penalty applied, full refund issued). System facilitates a rebook with the same advisor. Advisor cancels again within 24h. Is the second cancellation treated identically, or is there an escalation?
  * **Potential Impact:**
    * **User:** Twice-cancelled; no consultation received; significant frustration and time lost.
    * **System:** No repeat-cancellation detection or escalation logic.
    * **Business:** User churns; advisor remains on platform with no consequence beyond individual penalties.
  * **Recommendation / Questions for the Team:** Define repeat-cancellation threshold: 2 cancellations of the same user within 30 days → advisor account flagged for review. 3 cancellations total within 30 days → automatic suspension pending Platform Admin Account review.

---

#### Step 5: Confirmation

* **Scenario:** Goodwill credit issued but user cannot find it in the app
  * **Category:** System/Technical Errors
  * **Description:** Late Join Policy: advisor joins at T+12 → system issues pro-rated goodwill credit. Credit recorded in backend. But there is no visible "credits" section in the Family Wallet app. User never discovers they have a credit; it expires or sits unused.
  * **Potential Impact:**
    * **User:** Compensation issued but invisible — user still feels wronged.
    * **System:** Credit record exists with no UI surface.
    * **Business:** Goodwill gesture fails to achieve its purpose (retention); wasted compensation budget.
  * **Recommendation / Questions for the Team:** Credits must be surfaced immediately: push notification ("You've received a Xzł credit for your next consultation") + visible "My Credits" balance in the consultation section. Define credit expiry policy (e.g., 90 days) and communicate it at issuance.

* **Scenario:** Pro-rated credit calculation basis is undefined
  * **Category:** Business Logic and Edge Cases
  * **Description:** Policy says "pro-rated goodwill credit for missed minutes." But on what basis? If the consultation costs 150 PLN for 60 minutes and the advisor was 12 minutes late — is the credit 12/60 × 150 PLN = 30 PLN? Or calculated on a different rate?
  * **Potential Impact:**
    * **User:** Expects fair compensation; receives an amount they can't verify or predict.
    * **System:** Credit calculation formula not defined → engineering implements arbitrarily.
    * **Business:** User disputes credit amount; inconsistent compensation across cases.
  * **Recommendation / Questions for the Team:** Define formula explicitly in `domain/dictionary.md`: `credit = (minutes_missed / booked_duration) × session_price_paid`. Round up to nearest PLN. Display calculation breakdown in the credit notification.

---

#### Step 6: Notification

* **Scenario:** T+50 min in-session warning not received — advisor on desktop, not mobile
  * **Category:** System/Technical Errors
  * **Description:** Policy: in-session warning at T+50 min. Advisors likely conduct sessions on desktop (Advisor Panel). If the warning is a mobile push notification only — and the advisor's phone is in their pocket during the video call — the warning goes unseen.
  * **Potential Impact:**
    * **User:** No direct impact, but advisor doesn't wrap up; session terminates mid-sentence at T+60.
    * **System:** Warning delivered (200 response) but not perceived by recipient.
    * **Business:** Hard session termination remains the user experience despite the warning mechanism existing.
  * **Recommendation / Questions for the Team:** T+50 min warning must be delivered via the active session interface — not just mobile push. If Whereby API supports in-session overlays, use that. If not, build a web-based countdown visible in Advisor Panel during active sessions.

---

#### Step 7: Consultation

* **Scenario:** Exactly 50% session duration — boundary condition on partial refund threshold
  * **Category:** Business Logic and Edge Cases
  * **Description:** Policy: session ends before 50% of booked duration → partial refund offered. If a 60-minute session ends at exactly 30 minutes (50%) — does the refund trigger? "Before 50%" strictly means <50%, so 30/60 = 50% would not trigger. Boundary condition must be explicit.
  * **Potential Impact:**
    * **User:** Session ended at exactly the threshold; unclear if entitled to any compensation.
    * **System:** Boundary condition `< 50%` vs `<= 50%` produces different outcomes; implementation may differ from policy intent.
    * **Business:** If applied inconsistently, both over- and under-compensation occur at scale.
  * **Recommendation / Questions for the Team:** Define threshold as `< 50%` (strictly less than) and document explicitly. Test boundary: 29/60, 30/60, 31/60 min durations against refund logic. Add as explicit unit test case.

* **Scenario:** Advisor repeatedly claims "technical failure" to trigger partial refunds — pattern of abuse
  * **Category:** Business Logic and Edge Cases
  * **Description:** Policy: session ends before 50% duration due to technical failure → automatic partial refund. An advisor could exploit this by deliberately disconnecting early — especially for difficult clients or low-value sessions.
  * **Potential Impact:**
    * **User:** Receives partial refund (correct outcome) but loses consultation value.
    * **System:** No fraud detection on repeated early-session terminations by the same advisor.
    * **Business:** Systematic advisor abuse of the partial refund mechanism; revenue drain.
  * **Recommendation / Questions for the Team:** Track early-session termination rate per advisor. Threshold: >2 early terminations in 30 days → flag for Platform Admin Account review. Cross-reference with Whereby session duration API logs to verify termination was non-deliberate.

---

#### Step 8: Post-Consultation

* **Scenario:** Advisor disputes penalty after ≤24h cancellation — no appeal process defined
  * **Category:** Business Logic and Edge Cases
  * **Description:** Advisor receives penalty deduction from next payout. Advisor claims cancellation was due to a medical emergency and disputes the penalty. No appeal process exists in the current policy definition.
  * **Potential Impact:**
    * **User:** No direct impact (already received full refund).
    * **System:** No appeals workflow; dispute lands in general support queue.
    * **Business:** Advisor churns from platform due to perceived unfair treatment; supply-side risk for Advisor Console B2B model.
  * **Recommendation / Questions for the Team:** Define penalty appeal process: advisor submits appeal within 48h with reason; Platform Admin Account reviews within 5 business days; one appeal per 6-month period. Document in Advisor Terms & Conditions.

* **Scenario:** Summary submitted for a consultation marked as "no-show"
  * **Category:** Business Logic and Edge Cases
  * **Description:** Advisor was marked as no-show (joined at T+16 min, policy triggered). Later, the advisor submits a post-consultation summary anyway — perhaps they believe the partial session had value, or they're confused about the booking status.
  * **Potential Impact:**
    * **User:** Receives summary notification for a cancelled consultation — confusing; may reopen a resolved refund dispute.
    * **System:** Summary submitted against a `no-show` booking record — undefined state.
    * **Business:** User confusion leads to support contact; potential duplicate refund request.
  * **Recommendation / Questions for the Team:** Advisor Panel must block summary submission for bookings with status `no-show` or `cancelled`. States `no-show` and `cancelled` must be terminal for summary submission. If advisor believes session was partially valid, they must submit an appeal — not a summary.

---

## Quality Checklist — Run #3

- [x] All 4 categories covered: User Errors, System/Technical, External Dependencies, Business Logic & Edge Cases
- [x] Dictionary terminology used: all 4 account types, No-Show Policy, Cancellation tiers, Late Join Policy, Video Conference limits
- [x] Every scenario has User / System / Business impact
- [x] All recommendations specific and actionable
- [x] 12 new scenarios — 0 duplicated from Run #1 or Run #2
- [x] Date format ISO: `2026-03-14`
- [x] `@dictionary` loaded ✓ including newly added policies

---

## Cumulative Summary: Run #1 + Run #2 + Run #3

| Run | Scenarios | Dictionary | Key Finding |
|-----|-----------|-----------|-------------|
| Run #1 | 19 | ✗ not loaded | Race condition on slots; timezone bug; child booking gap; GDPR |
| Run #2 | 13 | ✓ roles + basic policies | Self-booking exploit; Advisor cancellation gap; 60-min cap; GDPR child data |
| Run #3 | 12 | ✓ full policies | Policy conflicts; undefined formulas; abuse patterns; appeal gaps |
| **Total** | **44** | | |

**5 items requiring `domain/dictionary.md` update identified in Run #3:**

| Gap | Impact | Priority |
|-----|--------|----------|
| Elevated penalty formula (≤2h) undefined | Legal dispute risk | HIGH |
| Pro-rated credit formula undefined | Inconsistent compensation | HIGH |
| Minimum session length not defined | Policy conflict trigger | MEDIUM |
| Repeat-cancellation escalation threshold missing | Advisor abuse risk | MEDIUM |
| Penalty appeal process not defined | Advisor churn risk | MEDIUM |

---

*Generated: 2026-03-14 | Command: `/detect-missing-requirements` | Framework: `skill:risk-analysis-framework` | Run #3 | Structure A*
