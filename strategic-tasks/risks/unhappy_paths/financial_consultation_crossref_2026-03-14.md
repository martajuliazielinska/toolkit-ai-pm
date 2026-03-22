# Unhappy Path Analysis: Financial Consultation Booking — Cross-Referenced Run

**Analysis Date:** 2026-03-14
**Source:** `strategic-tasks/test_process_financial_consultation.md`
**Framework:** `skill:risk-analysis-framework` · Structure A (Full Analysis)
**Dictionary:** `domain/dictionary.md` ✓ fully loaded
**Constraint:** Zero duplication with `strategic-tasks/risks/unhappy_paths/financial_consultation_2026-03-14.md` (Run #1 — 19 scenarios excluded)
**New scenarios this run:** 19

> **What this run demonstrates:** Given an existing analysis as a "negative space" constraint, the command generates a complementary analysis covering the same process from a deeper layer — only possible with a fully loaded dictionary. Run #1 had no dictionary; this run has all policies, formulas, role definitions, and session bounds defined.

---

### Process: financial_consultation

---

#### Step 1: Browse Advisor Offers

* **Scenario:** Financial Advisor Account sees their own profile in browse list
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary defines Financial Advisor Account as a distinct role. If an advisor uses the client-facing app, they may encounter their own profile in the advisor catalog — and technically initiate a self-booking that triggers the payment flow.
  * **Potential Impact:**
    * **User (advisor):** Accidentally self-books; payment charged.
    * **System:** Self-booking creates a consultation record where advisor = client — undefined state.
    * **Business:** Corrupted analytics; self-refund exploit risk.
  * **Recommendation / Questions for the Team:** Advisor accounts must be filtered out of the client-facing advisor catalog. Define: can a Financial Advisor Account also hold a Parent Account? If yes — role-switching logic required at login.

* **Scenario:** Advisor profile shows as available but specialization doesn't match user's need
  * **Category:** User Errors
  * **Description:** Dictionary defines Financial Advisor Account but doesn't mandate specialization tagging in the browse view. A user searching for family budgeting advice selects an advisor who specializes in corporate tax — profile listed no specialization tags.
  * **Potential Impact:**
    * **User:** Books, pays, attends a consultation that doesn't address their actual need.
    * **System:** No specialization filter; no mismatch detection.
    * **Business:** Refund request ("the advisor couldn't help me") + negative review.
  * **Recommendation / Questions for the Team:** Specialization tags must be mandatory for advisor profile publishing. Browse must support filtering by specialization. Define taxonomy valid for Family Wallet context: family budgeting, savings planning, tax basics, debt management.

---

#### Step 2: Select Advisor

* **Scenario:** Child Account navigates to advisor section — access not gated at navigation level
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: Child Account has "no payment methods, parental controls active." If Financial Consultations is visible in Child Account navigation, the child reaches a dead end only at Step 4 (payment) — not at Step 1.
  * **Potential Impact:**
    * **User (child):** Wastes time selecting advisor and time slot before hitting an invisible wall.
    * **System:** Role check enforced too late in the funnel.
    * **Business:** Child-facing dead end = parent complaint; undermines "family-safe" positioning.
  * **Recommendation / Questions for the Team:** Financial Consultations must not be visible to Child Accounts at navigation level. Confirm: does current menu rendering check account role before surfacing menu items?

* **Scenario:** Same user holds Parent Account and Financial Advisor Account under one email
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary defines 4 account types but doesn't address multi-role users. A financial advisor who is also a parent may register under one email. At login — which role is active? Can they book a consultation as a parent with another advisor?
  * **Potential Impact:**
    * **User:** Role ambiguity; booking flow may inherit wrong account context.
    * **System:** Single email → multiple role records — undefined auth behavior.
    * **Business:** Self-booking exploit; data commingling between professional and personal use.
  * **Recommendation / Questions for the Team:** Define: one email = one role strictly, OR multi-role with explicit role-switching UI. If multi-role allowed: booking flow must always resolve to Parent Account role, never Financial Advisor Account role.

---

#### Step 3: Choose Time Slot

* **Scenario:** Advisor Panel allows slot configuration longer than 60-minute video cap
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: Video Conference max session = 60 min. If Advisor Panel doesn't enforce this, an advisor can configure 90-minute slots. User books and pays for 90 minutes. Video terminates at 60 minutes regardless.
  * **Potential Impact:**
    * **User:** Paid for 90 minutes, received 60 — clear grounds for refund.
    * **System:** Advisor Panel slot configuration not validated against video cap.
    * **Business:** Structural refund liability on every >60-min booking.
  * **Recommendation / Questions for the Team:** Advisor Panel must enforce max session = 60 min at slot creation. Reject slot configuration >60 min with explicit error: "Sessions are limited to 60 minutes due to video conference constraints."

* **Scenario:** Legacy advisor slots below 30-minute minimum still visible in browse
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: minimum session length = 30 min; Advisor Panel blocks new slots <30 min. But legacy slots configured before this policy was enforced may still appear in browse and be selectable.
  * **Potential Impact:**
    * **User:** Books and pays for a sub-minimum session; partial refund may trigger.
    * **System:** Legacy slots bypass the new constraint; no migration ran on existing slot data.
    * **Business:** Refund liability on legacy sub-minimum bookings.
  * **Recommendation / Questions for the Team:** Run data migration on advisor slot records: all slots <30 min must be flagged and auto-extended or deactivated. Validator must apply retroactively, not only on new saves.

---

#### Step 4: Payment

* **Scenario:** User cancels at T-23h expecting free cancellation — charged 25% due to server timezone offset
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: free cancellation up to 24h before; 25% fee after 24h. Czech user cancels at 13:00 Prague time (T-23h their perspective). Server calculates in Warsaw time — registers as T-25h. Fee applied.
  * **Potential Impact:**
    * **User:** Charged unexpectedly; disputes the fee.
    * **System:** Cancellation window calculated in server timezone, not user timezone.
    * **Business:** Chargeback risk; especially damaging in Czech market (12.1% churn).
  * **Recommendation / Questions for the Team:** Cancellation deadline calculated and displayed in user's local timezone at booking time. Show explicit timestamp: "Free cancellation until: Saturday 15 March, 14:00 Warsaw time (15:00 Prague time)."

* **Scenario:** Advisor cancels — policy exists in dictionary but not surfaced in booking confirmation UI
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary defines Advisor Cancellation Policy (3 tiers: >24h / ≤24h / ≤2h) with penalties. But this policy is not shown to users at confirmation. User doesn't know they're entitled to a full refund if the advisor cancels — and may not claim it.
  * **Potential Impact:**
    * **User:** Not aware of entitlements; accepts partial outcome without requesting rightful refund.
    * **System:** Policy exists but is not communicated at the relevant touchpoint.
    * **Business:** Users who don't know their rights = suppressed refund claims; appears fine until a user discovers and escalates publicly.
  * **Recommendation / Questions for the Team:** Confirmation screen and email must include: "If your advisor cancels, you will receive a full refund regardless of timing." Advisor must acknowledge cancellation policy at Advisor Console onboarding.

* **Scenario:** Standard penalty (10% of session fee) falls below payment gateway minimum transaction floor
  * **Category:** External Dependencies
  * **Description:** Dictionary: standard penalty = 10% of session fee. For a 15 PLN introductory session, penalty = 1.50 PLN. Payment gateways typically enforce a minimum transaction floor (~2–5 PLN). Penalty deduction rejected or fails silently.
  * **Potential Impact:**
    * **User:** No direct impact.
    * **System:** Penalty marked as collected but not processed.
    * **Business:** Enforcement fails for low-value consultations; advisors offering cheap sessions face no consequence for cancellation.
  * **Recommendation / Questions for the Team:** Define minimum penalty floor (proposed: 5 PLN) regardless of session fee. Apply floor before gateway call. Test with minimum session fee values.

---

#### Step 5: Confirmation

* **Scenario:** Confirmation message omits session duration — user assumes 60-minute maximum
  * **Category:** User Errors
  * **Description:** Advisor configured a 45-minute slot. Confirmation email confirms the booking but omits duration. User expects 60 minutes; session ends at 45.
  * **Potential Impact:**
    * **User:** Feels session was cut short; refund request.
    * **System:** Duration stored in booking record but not surfaced in confirmation template.
    * **Business:** "I booked 60 minutes, not 45" — clear grounds for dispute.
  * **Recommendation / Questions for the Team:** Confirmation must explicitly state: advisor name, date/time in user's timezone, session duration, price paid, cancellation deadline (with local timezone timestamp), and video link. Duration is a mandatory field in the confirmation template.

* **Scenario:** Goodwill credit issued for late join — no "My Credits" section exists in app
  * **Category:** System/Technical Errors
  * **Description:** Dictionary: Late Join Policy — advisor joins T+5 to T+14 → pro-rated credit issued using formula `(minutes_missed / booked_duration) × session_price_paid`. Credit recorded in backend but no visible credits section in client app. Credit expires after 90 days — silently.
  * **Potential Impact:**
    * **User:** Compensation issued but invisible; expires without user ever knowing.
    * **System:** Credit record exists with no UI surface; background expiry job runs without notification.
    * **Business:** Goodwill gesture fails its purpose; compensation budget wasted; user still feels wronged.
  * **Recommendation / Questions for the Team:** Push notification on credit issuance with amount and formula breakdown. Persistent "My Credits" balance in consultation section. Expiry warnings at T-14 days and T-3 days before 90-day cutoff.

---

#### Step 6: Notification

* **Scenario:** No-show reminder not sent — both parties miss 15-minute grace window unknowingly
  * **Category:** System/Technical Errors
  * **Description:** Dictionary: No-Show Policy triggers if either party is >15 min late. Neither receives a pre-session reminder. Both run slightly late — system auto-applies no-show policy without either party knowing the clock was running.
  * **Potential Impact:**
    * **User:** Joins at T+16 — consultation cancelled, 50% refund applied automatically.
    * **System:** No-show triggered without user-visible countdown or warning.
    * **Business:** User disputes automated no-show ruling; support escalation.
  * **Recommendation / Questions for the Team:** Push notification to both parties at T-15 min: "Your consultation starts in 15 minutes — join now to avoid no-show policy." At T+5 min with no join detected: in-app alert "No-show policy applies after 15 minutes."

* **Scenario:** T+50 min in-session warning sent to mobile only — advisor on desktop Advisor Panel
  * **Category:** System/Technical Errors
  * **Description:** Dictionary: in-session warning at T+50 min. Advisors typically conduct sessions on desktop. If warning is mobile push only, advisor's phone in pocket during session = warning unseen. Session hard-terminates at T+60 mid-sentence.
  * **Potential Impact:**
    * **User:** No direct impact but advisor doesn't wrap up; abrupt session termination.
    * **System:** Warning delivered (200 response) but not perceived by recipient.
    * **Business:** Hard cutoff at peak interaction moment; negative memory of an otherwise positive consultation.
  * **Recommendation / Questions for the Team:** T+50 warning must be in-session — overlay in Whereby interface or web-based countdown in Advisor Panel. Never rely on mobile push for time-critical in-session alerts.

---

#### Step 7: Consultation

* **Scenario:** Advisor joins at T+14 min — below no-show threshold, but user receives only 46 of 60 minutes
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: no-show triggers at >15 min. Advisor joins at T+14. Policy not triggered — but user waited 14 minutes and receives only 46 of 60 booked minutes with no compensation mechanism.
  * **Potential Impact:**
    * **User:** Partial session, no refund triggered, high frustration.
    * **System:** Technically compliant; no action taken.
    * **Business:** User feels wronged with no recourse; likely to churn and leave a negative review.
  * **Recommendation / Questions for the Team:** Apply Late Join Credit: advisor joins T+5 to T+14 → pro-rated goodwill credit using `(minutes_missed / booked_duration) × session_price_paid`. Prevents advisors exploiting the sub-15-minute loophole.

* **Scenario:** Session ends at exactly 50% duration — boundary condition on partial refund threshold
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: session ends before 50% of booked duration → partial refund offered. 60-minute session ends at exactly 30 minutes (50.0%). "Before 50%" strictly = `< 50%`, so 30/60 = 50% does not trigger. This boundary must be explicitly defined in code and tests.
  * **Potential Impact:**
    * **User:** Session ended at threshold; unclear entitlement.
    * **System:** `< 50%` vs `<= 50%` implementation difference produces different outcomes at the boundary.
    * **Business:** Inconsistent application at scale = both over- and under-compensation cases.
  * **Recommendation / Questions for the Team:** Document threshold as `< 50%` (strictly less than). Add explicit unit tests for 29/60, 30/60, 31/60 min scenarios against refund logic.

* **Scenario:** Partial refund amount is less than payment gateway refund processing cost
  * **Category:** External Dependencies
  * **Description:** Session ends before 50% of a 30 PLN session → ~15 PLN refund owed. Payment gateway may charge a per-refund processing fee exceeding 15 PLN — net negative transaction for the platform.
  * **Potential Impact:**
    * **User:** Entitled to a refund that costs more to process than its value.
    * **System:** Refund generates negative net value.
    * **Business:** Structural loss on low-value consultations with early termination.
  * **Recommendation / Questions for the Team:** Define minimum partial refund threshold: if calculated refund < 10 PLN, issue as account credit instead of payment reversal. Confirm gateway refund processing costs before setting this threshold.

---

#### Step 8: Post-Consultation

* **Scenario:** Advisor submits summary containing investment recommendations — out of scope per strategy
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: Financial Advisor Account "can create consultation records." No content restrictions on summary form. Advisor submits investment product recommendations — explicitly out of scope per strategy (no investment management, no crypto, no lending until 2027).
  * **Potential Impact:**
    * **User:** Receives advice on products the platform can't verify or support.
    * **System:** Summary content unmoderated; compliance risk.
    * **Business:** Regulatory liability (KNF, MiFID II); undermines family-safe positioning.
  * **Recommendation / Questions for the Team:** Summary form must enforce category tags (budgeting, savings, tax planning — no investment/crypto). Disclaimer required: "Recommendations limited to family financial planning." Legal review required before Advisor Console launch.

* **Scenario:** Advisor penalty appeal process defined in dictionary but no UI exists in Advisor Panel
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: appeal process — advisor submits within 48h, Platform Admin reviews within 5 business days, 1 appeal per 6-month period. If Advisor Panel has no appeal submission UI, the policy exists only in documentation — not in the product.
  * **Potential Impact:**
    * **User:** No impact — already received full refund.
    * **System:** Appeal process defined but not implemented; disputes land in generic support queue.
    * **Business:** Advisor churns due to perceived injustice; no structured channel to contest penalty.
  * **Recommendation / Questions for the Team:** Advisor Panel must include "Appeal a Penalty" UI: reason text field, submission confirmation, status tracker (submitted → under review → resolved). 48h submission window must be surfaced at point of penalty notification — not buried in Terms & Conditions.

* **Scenario:** Advisor reinstated after suspension — affected users not notified of availability
  * **Category:** Business Logic and Edge Cases
  * **Description:** Advisor suspended (3 cancellations within 30 days); future bookings auto-cancelled; refunds issued. Platform Admin reinstates within 24h. Users who lost bookings are not notified — they've already booked elsewhere or assume the advisor is permanently gone.
  * **Potential Impact:**
    * **User:** Lost a preferred advisor booking; doesn't know the advisor is available again.
    * **System:** Reinstatement has no downstream "notify affected users" logic.
    * **Business:** Missed rebooking opportunity; users move to competitor advisors permanently.
  * **Recommendation / Questions for the Team:** On reinstatement: notify affected users proactively: "Your advisor [Name] is available again — rebook here." Include priority booking link valid for 48h. Track reinstatement rebook rate as an Advisor Console supply-health metric.

---

## Quality Checklist

- [x] All 4 categories covered: User Errors, System/Technical, External Dependencies, Business Logic & Edge Cases
- [x] Dictionary fully loaded: all 4 account types, cancellation tiers (user + advisor), late join policy, penalty formulas, appeal process, 30/60 min session bounds, credit expiry
- [x] Every scenario has User / System / Business impact
- [x] All recommendations specific and actionable
- [x] **0 scenarios duplicated from `financial_consultation_2026-03-14.md` (Run #1)**
- [x] 19 new scenarios across 8 steps
- [x] Date format ISO: `2026-03-14`

---

## Cross-Reference Demonstration

| Run #1 scenario (no dictionary) | This run equivalent (with dictionary) |
|---|---|
| Timezone display bug in slot selection | Timezone bug in cancellation deadline calculation (Czech user, T-23h) |
| Race condition on slots | Slot configuration >60 min bypasses video cap |
| Payment gateway timeout | Penalty (10%) below gateway minimum floor |
| Video link not generated at confirmation | Credit issued but no UI surface — invisible goodwill |
| User no-show | Advisor joins T+14 — below threshold, user loses 14 minutes with no recourse |
| Minor (child) booking access control | Child Account: navigation-level gate missing, dead end at Step 4 |

**Same process. Zero duplication. Deeper layer — made possible by a fully loaded dictionary.**

---

*Generated: 2026-03-14 | Command: `/detect-missing-requirements` | Constraint: exclude `financial_consultation_2026-03-14.md` | Framework: `skill:risk-analysis-framework` | Structure A*
