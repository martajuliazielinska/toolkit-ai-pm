# Unhappy Path Analysis: Financial Consultation Booking

**Analysis Date:** 2026-03-14
**Source:** `strategic-tasks/test_process_financial_consultation.md`
**Framework:** `skill:risk-analysis-framework` · Structure A (Full Analysis)
**Domain context:** Family Wallet — CEE fintech, family finance + advisor ecosystem
**Total scenarios:** 19 across 8 steps

> **Note:** `@dictionary` file not found in project. Domain terminology sourced from `strategy/strategy-2026-03-08.md`, `discovery/feedback-synthesis-2026-03-08.md`, and `.claude/CLAUDE.md`. Recommendation: create `dictionary.md` before Module 3 goes to production.

---

### Process: financial_consultation

---

#### Step 1: Browse Advisor Offers

* **Scenario:** Empty advisor list on first launch
  * **Category:** System/Technical Errors
  * **Description:** User navigates to "Financial Consultations" section but sees an empty list — no advisors loaded. This can happen if the Advisor Console has no published profiles yet, or if the fetch call fails silently.
  * **Potential Impact:**
    * **User:** Assumes the feature doesn't exist or is broken; exits without engaging.
    * **System:** No error logged if the empty state is treated as a valid 200 response with zero results.
    * **Business:** Feature perceived as non-functional at launch; risk of negative App Store review at exactly the wrong moment (post-v2.1 recovery phase).
  * **Recommendation / Questions for the Team:** Define explicit states: zero advisors (with CTA "check back soon"), load error (with retry), and loading skeleton. Never show a blank screen.

* **Scenario:** User doesn't understand what "Financial Consultation" is
  * **Category:** User Errors
  * **Description:** Family Wallet's primary persona (Anna, 38, parent) may not associate a paid consultation with a financial tool they use for budgeting. The section label alone doesn't communicate value or price range.
  * **Potential Impact:**
    * **User:** Skips section entirely; feature goes undiscovered.
    * **System:** No impact, but feature adoption metrics will be misleadingly low.
    * **Business:** Low adoption of what may be a significant revenue stream (Advisor Console B2B model).
  * **Recommendation / Questions for the Team:** Add contextual entry point from dashboard — e.g., after bank connection: "Want expert advice on your spending? Book a consultation." A/B test placement.

* **Scenario:** Advisor list loads but contains outdated or inactive advisors
  * **Category:** Business Logic and Edge Cases
  * **Description:** An Advisor who deactivated their profile or went on leave is still displayed in the list because the Advisor Panel sync is delayed or incomplete.
  * **Potential Impact:**
    * **User:** Selects an unavailable advisor, wastes time, and may reach Step 3 (Choose Time Slot) before discovering no slots exist.
    * **System:** Stale data in advisor catalog; no real-time availability flag.
    * **Business:** Frustrated user at time-selection step = high abandonment; potential refund request if payment proceeds.
  * **Recommendation / Questions for the Team:** Define Advisor status lifecycle: `active`, `on_leave`, `inactive`. Only `active` advisors appear in browse. Advisor Panel must push status updates in real time, not batch.

---

#### Step 2: Select Advisor

* **Scenario:** User selects advisor based on incomplete profile
  * **Category:** User Errors
  * **Description:** Advisor profile lacks key information (price, credentials, language, specialization). User selects based on photo/name, books, pays — then discovers a mismatch (e.g., advisor specializes in corporate tax, user needs family budgeting advice).
  * **Potential Impact:**
    * **User:** Wasted money; erosion of trust in the platform.
    * **System:** No impact technically, but refund request follows.
    * **Business:** Chargeback risk; negative review. For a product already at 4.2 App Store rating (below 4.5 target), this is significant.
  * **Recommendation / Questions for the Team:** Define mandatory profile fields for Advisor Panel publishing: specialization tags, languages, price per session, min/max session length. Profile incomplete = not publishable.

* **Scenario:** Race condition — two users simultaneously select the last available slot
  * **Category:** Business Logic and Edge Cases
  * **Description:** User A and User B both view Advisor X's profile at the same time. Both see a slot available. Both proceed to Step 3. One will fail at payment or confirmation — but which one, and how gracefully?
  * **Potential Impact:**
    * **User:** One user completes payment successfully; the other gets a confusing error after payment attempt.
    * **System:** If not handled with optimistic locking or slot reservation, double-booking is possible.
    * **Business:** Double-booked advisor = one cancelled consultation = refund + reputational damage.
  * **Recommendation / Questions for the Team:** Implement slot reservation at Step 2 (not Step 4). When User selects advisor and proceeds, hold the slot for 10 minutes. Expired reservation releases the slot automatically.

* **Scenario:** User navigates back and re-selects a different advisor
  * **Category:** User Errors
  * **Description:** User starts booking flow, goes back, selects a different advisor — but previously held slot or partially populated booking state persists in session.
  * **Potential Impact:**
    * **User:** Confused UI state; may inadvertently book the wrong advisor.
    * **System:** Orphaned reservation holds if not properly cleared on back-navigation.
    * **Business:** Slot locked for 10 minutes unnecessarily = reduced availability for that time window.
  * **Recommendation / Questions for the Team:** Explicitly clear booking session state on back-navigation. Define and test all back-navigation paths through the booking funnel.

---

#### Step 3: Choose Time Slot

* **Scenario:** User's timezone differs from Advisor's timezone
  * **Category:** Business Logic and Edge Cases
  * **Description:** Family Wallet targets CEE markets (PL, CZ, SK). At CEE expansion to HU or RO, UTC+1 vs. UTC+2 differences will cause mismatched slot display. A slot shown as "14:00" for the user may be "15:00" for the advisor.
  * **Potential Impact:**
    * **User:** Shows up to video call an hour early or late.
    * **System:** Booking stored in local time, not UTC — incorrect comparison logic.
    * **Business:** Failed consultation = refund + advisor complaint + potential churn of both parties.
  * **Recommendation / Questions for the Team:** Store all slot times in UTC. Display times in user's local timezone (auto-detected, with manual override). Show advisor's timezone explicitly: "14:00 your time (15:00 advisor's time CET)."

* **Scenario:** No available slots in the next 30 days
  * **Category:** External Dependencies
  * **Description:** High-demand advisors or advisors who haven't configured availability in Advisor Panel show no available slots. User reaches Step 3 and encounters an empty calendar.
  * **Potential Impact:**
    * **User:** Dead end — no CTA for what to do next.
    * **System:** No error, but user journey terminates with no outcome.
    * **Business:** Lost booking intent; user may not return.
  * **Recommendation / Questions for the Team:** If advisor has no availability in next 14 days, surface a "Notify me when available" option. Require advisors to configure availability before profile goes live.

* **Scenario:** Advisor cancels availability after User selects slot but before payment completes
  * **Category:** External Dependencies
  * **Description:** Advisor updates their calendar in Advisor Panel between the moment User selects a slot and completes payment. The slot is technically no longer valid.
  * **Potential Impact:**
    * **User:** Payment goes through, but confirmation step fails or produces a booking for a cancelled slot.
    * **System:** Booking record created against a slot the advisor no longer honors.
    * **Business:** Immediate refund required; advisor trust degraded.
  * **Recommendation / Questions for the Team:** Lock slot during payment processing (Step 4). If advisor-side change occurs during lock period, queue the change and resolve after payment. If conflict detected post-payment, trigger automatic refund and notify both parties immediately.

---

#### Step 4: Payment

* **Scenario:** Payment gateway timeout during transaction
  * **Category:** External Dependencies
  * **Description:** User submits payment. The payment gateway times out without returning a success or failure response. The FWCS doesn't know if the charge went through.
  * **Potential Impact:**
    * **User:** Sees a loading spinner indefinitely, or a generic error. Doesn't know if they were charged.
    * **System:** Booking in ambiguous state — may be created without payment confirmation, or payment processed without booking created.
    * **Business:** Potential double charge if user retries; potential lost payment if charge succeeded but booking was not created. Chargeback risk.
  * **Recommendation / Questions for the Team:** Implement idempotency keys on all payment requests. Define explicit timeout handling: after N seconds, query payment gateway for transaction status before showing error to user. Never show "payment failed" without confirming with gateway first.

* **Scenario:** User closes the app during payment processing
  * **Category:** User Errors
  * **Description:** User submits payment, gets distracted, force-closes the app (or loses connectivity) before the confirmation screen renders.
  * **Potential Impact:**
    * **User:** Returns to app with no idea if booking was created or payment was taken.
    * **System:** Booking may be in `pending` state with no resolution trigger.
    * **Business:** User contacts support; manual resolution required; negative experience at a critical funnel step.
  * **Recommendation / Questions for the Team:** Implement async payment confirmation: if user returns to app, show "Checking your booking status…" screen that polls the backend. Send push notification + email when payment confirms regardless of app state.

* **Scenario:** Currency conversion mismatch — card charged in different currency than displayed price
  * **Category:** Business Logic and Edge Cases
  * **Description:** App displays price in PLN. User's card is issued in EUR (e.g., Czech user with CZ bank card). Currency conversion happens at the payment gateway — the displayed price and charged amount may differ.
  * **Potential Impact:**
    * **User:** Surprised by a different amount on their bank statement; files a dispute.
    * **System:** No error — transaction technically succeeded.
    * **Business:** Chargeback risk; user distrust. Particularly sensitive given Czech Republic is already the highest-churn market (12.1%).
  * **Recommendation / Questions for the Team:** Display currency conversion estimate at checkout for non-PLN cards. Confirm: does this require additional disclosure under PSD2? Consult legal before Czech launch.

---

#### Step 5: Confirmation

* **Scenario:** Confirmation screen displayed but video conference link not yet generated
  * **Category:** System/Technical Errors
  * **Description:** Booking is confirmed, but the video conference link generation (external service) is asynchronous and hasn't completed when the confirmation screen renders. User sees "Your booking is confirmed" with a blank or placeholder link.
  * **Potential Impact:**
    * **User:** Tries to copy/use the link immediately; it doesn't work.
    * **System:** Race condition between booking confirmation and video link generation.
    * **Business:** User loses trust in the confirmation; contacts support unnecessarily.
  * **Recommendation / Questions for the Team:** Do not render confirmation screen until video link is generated — or clearly communicate "Your video link will be sent by email within 5 minutes" and enforce that SLA. Never show a broken/placeholder link.

* **Scenario:** Duplicate confirmation — user double-tapped "Confirm"
  * **Category:** User Errors
  * **Description:** Network lag causes the confirmation button to be tappable twice within a short window. User unknowingly submits the booking twice.
  * **Potential Impact:**
    * **User:** Two bookings created, two charges on card.
    * **System:** Two booking records for same user/advisor/time slot — or slot conflict.
    * **Business:** Two refunds, double Advisor notification, confusion in Advisor Panel.
  * **Recommendation / Questions for the Team:** Disable the "Confirm" button immediately on first tap (show loading state). Implement server-side idempotency: same user + same slot within 60 seconds = reject second request as duplicate.

---

#### Step 6: Notification

* **Scenario:** Advisor notification not delivered
  * **Category:** External Dependencies
  * **Description:** System sends confirmation to User successfully, but the notification to Advisor fails — email bounces, push notification token is stale, or Advisor Panel doesn't surface the new booking prominently.
  * **Potential Impact:**
    * **User:** Expects the advisor to be prepared; advisor is unaware.
    * **System:** Notification sent from FWCS perspective (200 response), but delivery failed silently.
    * **Business:** Advisor no-show at consultation = refund + reputational damage; undermines Advisor Console B2B model.
  * **Recommendation / Questions for the Team:** Implement notification delivery confirmation (not just sent confirmation). Require Advisor Panel to display new bookings in a persistent inbox. Define "Advisor acknowledgement" as a required step with automated reminders 24h and 1h before consultation.

* **Scenario:** User receives notification in wrong language
  * **Category:** Business Logic and Edge Cases
  * **Description:** User's app language is Czech, but notification template is only available in Polish. System falls back to Polish — or worse, renders an untranslated key (`confirmation.message.body`).
  * **Potential Impact:**
    * **User:** Receives incomprehensible notification; may miss critical booking details.
    * **System:** Localization fallback logic untested.
    * **Business:** Directly worsens Czech user experience — a market already at 12.1% churn.
  * **Recommendation / Questions for the Team:** Audit all notification templates for i18n coverage before Czech launch. Define fallback chain: user language → platform language → English (never raw key). Block market launch until all templates are translated.

---

#### Step 7: Consultation

* **Scenario:** Video conference link fails to connect at scheduled time
  * **Category:** External Dependencies
  * **Description:** User and Advisor both have the link, but the video conference service is experiencing downtime or the link has expired (e.g., was generated 7 days ago with a 72h expiry).
  * **Potential Impact:**
    * **User:** Blocked from consultation they paid for; no fallback contact method for advisor.
    * **System:** No visibility into video conference service health within FWCS.
    * **Business:** Consultation doesn't happen = full refund required; viral negative word-of-mouth risk.
  * **Recommendation / Questions for the Team:** Monitor video conference service health before scheduled time (30-minute pre-check). Send both parties a fresh link 15 minutes before start. Define contingency: if link fails within 5 minutes of start, trigger automatic in-app chat escalation + account credit.

* **Scenario:** Advisor joins but User doesn't appear (no-show)
  * **Category:** Business Logic and Edge Cases
  * **Description:** User books and pays, but doesn't join the video call at the scheduled time. Advisor waits — for how long? What happens to the payment? Is the slot considered fulfilled?
  * **Potential Impact:**
    * **User:** May want a refund or reschedule if they missed due to a legitimate reason.
    * **System:** No automated resolution for no-show scenario.
    * **Business:** Advisor time wasted; unclear policy = manual support overhead at scale.
  * **Recommendation / Questions for the Team:** Define no-show policy before launch: User no-show after N minutes → consultation marked fulfilled (no refund) OR reschedulable once within 48h. Advisor no-show → full refund + credit. Surface this policy at Step 4, before payment.

* **Scenario:** User is a minor (child using Pocket Money feature)
  * **Category:** Business Logic and Edge Cases
  * **Description:** Pocket Money is used by children aged 8+. What prevents a child from initiating a consultation booking and charging their parent's card? If the feature appears in main app navigation, there is no inherent age-gate.
  * **Potential Impact:**
    * **User (parent):** Unexpected charge; loss of trust in family finance tool.
    * **System:** No role-based access control on consultation booking.
    * **Business:** Regulatory risk (minors entering financial contracts); potential GDPR issue (minor's data in consultation record).
  * **Recommendation / Questions for the Team:** Consultation booking must be restricted to the `parent/guardian` account role — not accessible from child account view. Does the current account model support role-based feature access? Architecture decision required before feature launch.

---

#### Step 8: Post-Consultation

* **Scenario:** Advisor submits summary but User cannot find it
  * **Category:** System/Technical Errors
  * **Description:** Advisor submits summary via Advisor Panel. User returns to the app but cannot locate it — no notification, no visible inbox, no consultation history.
  * **Potential Impact:**
    * **User:** Consultation value locked in Advisor Panel; user feels the service delivered nothing tangible.
    * **System:** Summary exists in Advisor Panel database but is not surfaced in User-facing FWCS.
    * **Business:** Post-consultation summary is the primary value artifact. If Users can't find it, NPS for this feature will be very low.
  * **Recommendation / Questions for the Team:** Define User-facing "Consultation History" section with: booking details, video link, and advisor summary. Send push notification when summary is ready: "Your advisor has submitted their recommendations. Tap to view."

* **Scenario:** Advisor never submits post-consultation summary
  * **Category:** External Dependencies
  * **Description:** Consultation ends. Advisor Panel has a summary submission form, but it's optional or there's no enforcement. Advisor forgets or doesn't understand the expectation.
  * **Potential Impact:**
    * **User:** Paid for a consultation with no written output; refund request likely.
    * **System:** Consultation marked `completed` with no summary attached.
    * **Business:** If this happens at scale, Advisor Console B2B model is damaged before it gains traction.
  * **Recommendation / Questions for the Team:** Summary submission must be mandatory. Advisor Panel should block "mark as completed" until summary is submitted. Automated reminder at +1h and +24h after consultation end. Escalation to Family Wallet team at +48h.

* **Scenario:** User wants to rebook the same advisor but history is not linked to browse UX
  * **Category:** Business Logic and Edge Cases
  * **Description:** User had a positive consultation and wants to rebook. They navigate to advisor browse (Step 1) — no "previous consultations" signal, no "rebook" shortcut, no way to quickly find the advisor they already spoke to.
  * **Potential Impact:**
    * **User:** Friction in rebooking; may book a different advisor by mistake or give up.
    * **System:** Booking history exists in database but not linked to browse/selection UX.
    * **Business:** Repeat bookings are the highest-LTV use case for the Advisor Console model. Friction here directly caps revenue ceiling.
  * **Recommendation / Questions for the Team:** After consultation, surface: "Book another session with [Advisor Name]" CTA on the summary screen. Add "My Advisors" shortcut showing previously booked advisors. Track rebook rate as a primary Advisor Console success metric.

---

## Quality Checklist (Definition of Ready)

- [x] All 4 categories covered: User Errors, System/Technical, External Dependencies, Business Logic & Edge Cases
- [x] Every scenario has User / System / Business impact assessment
- [x] All recommendations are specific and actionable
- [x] Scenarios grounded in Family Wallet context (CEE market, Czech churn, Advisor Console, Pocket Money, App Store rating)
- [x] 19 scenarios across 8 steps (minimum 8–12 met)
- [x] Date format ISO: `2026-03-14`
- [ ] `@dictionary` not found — create `dictionary.md` before next Module 3 analysis

---

*Generated: 2026-03-14 | Command: `/detect-missing-requirements` | Framework: `skill:risk-analysis-framework` | Structure A*
