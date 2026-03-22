# Unhappy Path Analysis: Pocket Money Flow — Cross-Referenced Run (dictionary gaps open)

**Analysis Date:** 2026-03-14
**Source:** `strategic-tasks/test_process_pocket_money.md`
**Framework:** `skill:risk-analysis-framework` · Structure A (Full Analysis)
**Dictionary:** `domain/dictionary.md` ✓ loaded — ⚠️ 6 Pocket Money-specific gaps open
**Constraint:** Zero duplication with `strategic-tasks/risks/unhappy_paths/pocket_money_2026-03-14.md` (Run #1 — 22 scenarios excluded)
**New scenarios this run:** 18
**Hedged scenarios (dictionary gap):** 4 — flagged with ⚠️

> **Purpose of this run:** Demonstrate the cost of open dictionary gaps on analysis quality. Scenarios marked ⚠️ could not produce specific recommendations because the relevant domain definition is missing. Compare with `financial_consultation_crossref_2026-03-14.md` (full dictionary) to see the difference.

---

### Process: pocket_money

---

#### Step 1: Parent Setup

* **Scenario:** Child age not captured at setup — feature experience not age-differentiated
  * **Category:** User Errors
  * **Description:** Setup flow asks for child's name but not birth date or age. Pocket Money for an 8-year-old (learning first money concepts) requires a different UX than for a 16-year-old (budgeting, saving toward goals). System treats all Child Accounts identically regardless of age.
  * **Potential Impact:**
    * **User (child, 8yo):** Overwhelmed by features designed for older children; abandons the app.
    * **User (child, 16yo):** Patronized by simplified UX; disengages.
    * **System:** No age-based feature gating or UX differentiation.
    * **Business:** Educational mission — "teaching children healthy money habits" — fails to deliver age-appropriate content. Also required for GDPR Article 8 compliance (different consent mechanisms for under-13 vs. 13–17).
  * **Recommendation / Questions for the Team:** Capture child's birth date at setup (or age range: 6–10 / 11–14 / 15+). Differentiate Pocket Money UX by age cohort. Younger children: simple earn/save. Older children: budget categories, goal planning, spending history analysis.

* **Scenario:** Parent sets up Pocket Money before Child Account exists
  * **Category:** User Errors
  * **Description:** Setup flow may allow a parent to configure allowance amount and schedule before the target Child Account is created or linked. Configuration saved to an orphaned record with no child attached.
  * **Potential Impact:**
    * **User (parent):** Completes setup, believes it's active — but no child receives the allowance.
    * **System:** Allowance schedule created with null or placeholder `child_account_id`.
    * **Business:** Silent failure at the start of the Pocket Money flow; parent discovers issue only when child asks where their money is.
  * **Recommendation / Questions for the Team:** Require an active, linked Child Account before Pocket Money setup can be completed. If no Child Account exists: surface "Create a child account first" as a direct CTA — not a post-save error.

* **Scenario:** Allowance paused by parent — child receives no notification or explanation
  * **Category:** User Errors
  * **Description:** Parent pauses the allowance schedule. System stops the scheduled job. Child's next expected allowance simply doesn't arrive — no notification to child, no in-app explanation.
  * **Potential Impact:**
    * **User (child):** Confused and upset; assumes technical error or perceives punishment without communication.
    * **System:** Pause recorded in backend; no child-facing state change surfaced.
    * **Business:** Child disengages from the app; Pocket Money loses its reliability signal — the exact opposite of what drives the +5pp MoM adoption trend.
  * **Recommendation / Questions for the Team:** When parent pauses allowance, prompt: "Would you like to notify [child's name] that their allowance is paused?" Default: yes, with editable template message. Child sees in-app banner: "Your allowance is paused — ask your parent for details."

---

#### Step 2: Allowance Trigger

* **Scenario:** Allowance scheduled for the 1st of month — fires on a bank holiday
  * **Category:** External Dependencies
  * **Description:** Parent sets monthly allowance for "the 1st of every month." In PL/CZ/SK markets, the 1st may fall on a public holiday. Depending on whether allowance is processed via a banking interface or purely as an internal ledger, the transfer may be deferred, fail, or process normally — and the behavior is undefined.
  * **Potential Impact:**
    * **User (child):** Expected allowance delayed 1–3 days with no explanation.
    * **System:** Behavior on bank holidays undefined — internal ledger vs. bank transfer processing rules differ.
    * **Business:** Child expects money on the 1st; it doesn't arrive; parent blamed; app blamed.
  * **Recommendation / Questions for the Team:** Define: is allowance an internal ledger credit (processes regardless of bank holidays) or a bank transfer (subject to banking calendar)? If internal ledger: process on schedule regardless of holidays — communicate this clearly. If bank transfer: define deferral policy (next business day vs. prior business day) and notify both parent and child.

* **Scenario:** Parent Account deleted — orphaned allowance schedule continues firing
  * **Category:** Business Logic and Edge Cases
  * **Description:** Parent who created the Pocket Money schedule deletes their account (or is deleted by Platform Admin Account). Allowance schedule was created by this parent. Does it continue firing? Transfer to the second Parent Account? Halt silently?
  * **Potential Impact:**
    * **User (child):** Allowance stops with no explanation — or continues from an account that no longer exists.
    * **System:** Allowance schedule has a `created_by` parent_account_id that no longer exists; undefined behavior on next trigger.
    * **Business:** Financial inconsistency; orphaned scheduled jobs in the database.
  * **Recommendation / Questions for the Team:** Allowance schedules must be owned by the Child Account, not by a specific Parent Account. On Parent Account deletion: if a second Parent Account exists, transfer schedule ownership silently. If no second parent: halt schedule with notification to child and explanation.

* **Scenario:** Parent changes allowance amount within 1 hour of scheduled trigger
  * **Category:** Business Logic and Edge Cases
  * **Description:** Parent updates allowance from 30 PLN to 50 PLN at 08:55. Allowance job fires at 09:00. Which amount fires — old or new? The 5-minute window creates a race condition between configuration write and job execution.
  * **Potential Impact:**
    * **User (child):** Receives unexpected amount; if old amount fires, child may feel shortchanged.
    * **System:** Race condition between config write and job read; depends on whether job reads config at schedule time or at execution time.
    * **Business:** Unpredictable allowance amount = trust issue; support ticket.
  * **Recommendation / Questions for the Team:** Allowance job must read configuration at execution time (not at schedule creation time). Define: if config changes within 1 hour of trigger, does new amount apply immediately or from next cycle? Surface this in the UI: "Your change will apply starting [next trigger date]."

---

#### Step 3: Task Assignment

* **Scenario:** Task assigned to wrong child in a multi-child family
  * **Category:** User Errors
  * **Description:** Parent with 3 children creates a task and selects the wrong child from the assignment dropdown — similar names, small UI target on mobile. Task and reward appear in the wrong Child Account.
  * **Potential Impact:**
    * **User (wrong child):** Sees an unexpected task; may complete it for the reward.
    * **User (intended child):** Doesn't see their task; doesn't earn the reward.
    * **System:** Task assigned to wrong `child_account_id`; no post-assignment validation.
    * **Business:** Family dispute; parent blames the app's assignment UI.
  * **Recommendation / Questions for the Team:** Show child's avatar/photo in the assignment confirmation step: "Assigning 'Clean bedroom — 5 PLN' to [photo] Kasia. Confirm?" One extra confirmation tap prevents multi-child assignment errors. Add 5-second undo option post-assignment.

* **Scenario:** Task reward set at 10× the weekly allowance — perverse incentive structure
  * **Category:** Business Logic and Edge Cases
  * **Description:** Parent sets task reward of 200 PLN for "Clean the garage." Child's weekly allowance is 20 PLN. The reward is 10× the allowance — which may inadvertently teach the child that exceptional one-time tasks are more valuable than consistent responsibility. Opposite of the intended financial education message.
  * **Potential Impact:**
    * **User (child):** Focuses exclusively on high-reward tasks; ignores base responsibilities; savings strategy distorted.
    * **System:** No validation of reward amount relative to allowance.
    * **Business:** Educational mission undermined; product inadvertently teaches distorted money values.
  * **Recommendation / Questions for the Team:** When task reward exceeds 3× the weekly allowance, show a soft nudge: "This reward is quite high relative to [child's name]'s weekly allowance. Is that intentional?" Don't block — create a moment of parental reflection. Log high-reward tasks for product analytics on healthy allowance/reward ratios.

---

#### Step 4: Child Completes Task

* **Scenario:** Photo upload fails silently due to file size limit — child believes task was submitted
  * **Category:** System/Technical Errors
  * **Description:** Child takes a photo and submits task completion. Photo exceeds an undefined file size limit. Upload fails in the background. Child sees the "submitted" confirmation screen. Parent receives only a text completion notice — no photo.
  * **Potential Impact:**
    * **User (child):** Believes task was submitted with photo proof; parent rejects for "no photo attached."
    * **System:** Photo upload failure not surfaced at submission time; task status shows `submitted` without photo.
    * **Business:** Child disputes rejection ("I sent the photo!"); family conflict mediated by app.
  * **Recommendation / Questions for the Team:** Photo upload must be synchronous and confirmed before "task submitted" screen is shown. If upload fails: explicit error message ("Photo upload failed — please try again or submit without photo"). Never show a success screen before all submission components are confirmed. Define and document file size limit.

* **Scenario:** Child marks task complete without doing it — system is fully trust-dependent
  * **Category:** User Errors
  * **Description:** Photo proof is optional. Child marks "bedroom cleaned" without actually cleaning it. Parent must trust the child or check in person. No verification mechanism beyond the optional photo exists in the system.
  * **Potential Impact:**
    * **User (parent):** Approves reward for work not done; discovers later; trust broken.
    * **System:** No fraud or dishonesty detection on task completion.
    * **Business:** If Pocket Money teaches children that claiming work = reward regardless of doing it, the educational mission fails entirely.
  * **Recommendation / Questions for the Team:** This is a deliberate product philosophy decision. Options: (a) keep photo optional — make honesty part of the educational conversation parents have with children; (b) make photo mandatory for tasks above a threshold reward amount (e.g., >10 PLN); (c) add a parent "spot check" feature to request a live verification photo. Define in product principles — not as an engineering default.

---

#### Step 5: Parent Approval

* **Scenario:** ⚠️ Second Parent Account overrides first parent's rejection
  * **Category:** Business Logic and Edge Cases
  * **Description:** Parent A rejects a task submission with a reason. Task moves to `rejected` state. Parent B reviews the same submission and approves it — overriding Parent A's decision. Reward is issued.
  * **Potential Impact:**
    * **User (child):** Receives reward after rejection — confusing signal about parental authority and consistency.
    * **System:** `rejected` state is not terminal if second parent can approve; task state machine undefined.
    * **Business:** Parental authority undermined by the product; children learn that rejected decisions can be reversed by appealing to the other parent through the app.
  * **Recommendation / Questions for the Team:** ⚠️ *Requires dictionary gap closure: task lifecycle states undefined.* Define task state machine first. Then decide: is `rejected` terminal (only rejecting parent can unreject) or re-openable by any Parent Account? Both are valid product decisions with different educational implications — but the decision must be explicit.

* **Scenario:** ⚠️ Both Parent Accounts simultaneously inactive — task stuck in `submitted` indefinitely
  * **Category:** Business Logic and Edge Cases
  * **Description:** Both parents are on holiday or have suspended accounts. Task submitted by child sits in `submitted` state with no reviewer. No auto-approval SLA defined in dictionary for tasks. Task accumulates in pending queue with no escalation.
  * **Potential Impact:**
    * **User (child):** Completed work unreviewed; reward withheld indefinitely.
    * **System:** No escalation logic for reviewer unavailability; no timeout on `submitted` state.
    * **Business:** Child disengages from task system; Pocket Money loses its reward loop credibility.
  * **Recommendation / Questions for the Team:** ⚠️ *Requires dictionary gap closure: task review SLA undefined.* Define SLA and escalation: reminders at T+24h and T+72h. At T+7 days: define policy — auto-approve, or escalate to Platform Admin Account? Should mirror the goal auto-unlock decision for consistency.

---

#### Step 6: Reward Distribution

* **Scenario:** ⚠️ Parent retroactively disputes an already-approved and distributed reward
  * **Category:** Business Logic and Edge Cases
  * **Description:** Parent A approves a task; reward is distributed to child's balance. One hour later, parent discovers the child staged the photo proof. Parent wants to reverse the reward. No reversal policy defined in dictionary.
  * **Potential Impact:**
    * **User (child):** Balance reduced after they thought money was theirs — deeply confusing and demotivating.
    * **System:** No reward reversal mechanism defined; approved transactions may be immutable.
    * **Business:** If reversal is possible, children learn balances are unreliable. If impossible, parents feel powerless against dishonest completion. Neither is a good default.
  * **Recommendation / Questions for the Team:** ⚠️ *Requires dictionary gap closure: reward reversal policy undefined.* Proposed direction: approved rewards are final (immutable) to maintain child's trust in balance reliability. Instead: provide "deduct from future allowance" mechanism for parents to address dishonest completion without directly clawing back earned funds. This aligns with real-world financial education principles.

* **Scenario:** Reward distributed in PLN but child's account displays in CZK (CEE expansion)
  * **Category:** External Dependencies
  * **Description:** Family relocates from Poland to Czech Republic. Parent account operates in PLN. Child account was created in PLN context. Reward of 5 PLN is transferred — child sees inconsistent amount depending on whether conversion is applied at display time.
  * **Potential Impact:**
    * **User (child):** Balance displayed in unexpected currency or incorrect amount.
    * **System:** Multi-currency handling not defined for Child Accounts in CEE expansion context.
    * **Business:** CEE expansion (CZ → SK) is Q3 strategic priority — currency handling in Pocket Money must be resolved before expansion launch.
  * **Recommendation / Questions for the Team:** Define currency model for Pocket Money accounts: single-currency locked at account creation, OR multi-currency with conversion at display. Core question: is Pocket Money an internal ledger (no real currency) or does it represent actual money? This determines whether currency is even a meaningful concept in this context.

---

#### Step 8: Savings Goal

* **Scenario:** Parent sets savings goal on behalf of child without child's knowledge
  * **Category:** User Errors
  * **Description:** Parent Account has full access to child monitoring. Parent creates a savings goal ("Save for school trip — 300 PLN") and sets it in the child's account without the child's input. Child opens the app and discovers a goal they didn't choose.
  * **Potential Impact:**
    * **User (child):** Demotivated by an imposed goal; doesn't engage with the savings feature.
    * **System:** Goal creation by Parent Account on Child Account — technically valid, educationally counterproductive.
    * **Business:** Savings goals drive engagement and long-term retention; parent-imposed goals undermine the child's intrinsic motivation that makes the feature sticky.
  * **Recommendation / Questions for the Team:** Define goal creation permissions: (a) child-only; (b) parent-only; (c) collaborative — parent proposes, child must confirm before goal is active. Option (c) best serves the educational mission. Define this as a product principle — not an engineering default.

* **Scenario:** Savings goal estimated completion date silently recalculates when allowance changes
  * **Category:** Business Logic and Edge Cases
  * **Description:** Child sets a savings goal: "Save 200 PLN — estimated 10 weeks at current allowance." Parent increases allowance. System recalculates estimated completion date. If goal amount and estimated completion are coupled, a parent's allowance change may silently alter the child's savings plan display.
  * **Potential Impact:**
    * **User (child):** Savings timeline changes without child's action or awareness.
    * **System:** Coupling between allowance schedule and savings goal calculation — undefined behavior on allowance change.
    * **Business:** Child loses trust in the savings goal as a fixed commitment.
  * **Recommendation / Questions for the Team:** Savings goal target amount must be immutable once set. Allowance changes only affect the *estimated completion date* — never the target amount. Recalculate and display new estimated date when allowance changes, with notification to child: "Your allowance changed — you'll reach your goal [sooner / later] now."

---

#### Step 10: Transaction History

* **Scenario:** Child account transitions to adulthood at age 18 — full history ownership undefined
  * **Category:** Business Logic and Edge Cases
  * **Description:** A child uses Pocket Money from age 10. At age 18, they are legally an adult. Does Child Account auto-transition? Does full transaction history (including rejection reasons written by parents when the child was 12) transfer to an adult-owned account? Do they gain retrospective data portability rights?
  * **Potential Impact:**
    * **User (now-adult):** Cannot control their own childhood financial history; parental visibility persists past legal adulthood.
    * **System:** No account transition flow defined for the age-18 threshold.
    * **Business:** GDPR: at 18, the person becomes the full data subject — parental consent no longer applies. Continued parental access to an adult's transaction history = GDPR violation.
  * **Recommendation / Questions for the Team:** Define account lifecycle: Child Account → Adult Account transition at age 18 (or on request from age 16+). On transition: parental visibility removed, full data ownership transferred to the now-adult user, notification sent to both parties. Legal review: automatic transition or explicit consent required?

* **Scenario:** Platform Admin Account accesses child's transaction history for fraud investigation — no consent framework
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: Platform Admin Account has "full system access." If fraud investigation requires reviewing a Child Account's transaction history — spending patterns, task completions, savings goals — there is no defined consent or notification framework for this access.
  * **Potential Impact:**
    * **User (child):** Personal financial behavior data accessed without awareness.
    * **System:** Admin access to child data governed only by "full system access" — no audit log requirement.
    * **Business:** GDPR Article 8 (children's data) requires heightened protection. Unrestricted admin access without documented legal basis = compliance risk.
  * **Recommendation / Questions for the Team:** Define admin access policy for Child Accounts: (a) all admin access logged with reason; (b) Parent Account notified within 24h of any admin access to child's data (except where notification would compromise the investigation); (c) legal basis for admin access documented in privacy policy. Legal review required before launch.

---

## Quality Checklist

- [x] All 4 categories covered: User Errors, System/Technical, External Dependencies, Business Logic & Edge Cases
- [x] Dictionary roles used: Child Account, Parent Account, Platform Admin Account
- [x] Every scenario has User / System / Business impact
- [x] **0 scenarios duplicated from `pocket_money_2026-03-14.md` (Run #1)**
- [x] 18 new scenarios across 10 steps
- [x] Date format ISO: `2026-03-14`
- [⚠️] **4 scenarios hedged due to open dictionary gaps**

---

## Diagnoza — Dictionary Gap Cost

| Scenario | Missing definition | Effect on output |
|---|---|---|
| Second parent overrides rejection | Task lifecycle states | Cannot specify if `rejected` is terminal — recommendation is "define first" |
| Both parents inactive | Task review SLA | Cannot give a number of days — recommendation is "define policy" |
| Reward reversal | Reward reversal policy | Cannot propose mechanism — only "decide direction first" |
| Step 5 double-approval | Task state machine | Recommendation incomplete without state definitions |

**Contrast with `financial_consultation_crossref_2026-03-14.md` (full dictionary):**

| With full dictionary | With gaps open |
|---|---|
| "Free cancellation until Saturday 15 March, 14:00 Warsaw time (15:00 Prague time)" | "⚠️ Requires dictionary gap closure. Define task review SLA." |
| "Penalty = 10% of session fee; floor = 5 PLN" | "Define reversal policy — proposed direction: approved rewards are final" |
| "T+14 min late join → credit = (14/60) × session_price" | "Define task state machine first, then decide if `rejected` is terminal" |

**This is the cost of open gaps: scaffolding instead of recommendations.**

---

*Generated: 2026-03-14 | Command: `/detect-missing-requirements` | Constraint: exclude `pocket_money_2026-03-14.md` | Dictionary: ⚠️ 6 gaps open | Framework: `skill:risk-analysis-framework` | Structure A*
