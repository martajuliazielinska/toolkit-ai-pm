# Unhappy Path Analysis: Pocket Money Flow — Run #3 (Full Dictionary)

**Analysis Date:** 2026-03-14
**Source:** `strategic-tasks/test_process_pocket_money.md`
**Framework:** `skill:risk-analysis-framework` · Structure A (Full Analysis)
**Dictionary:** `domain/dictionary.md` ✓ fully loaded (Sections 2–4)
**Constraint:** Zero duplication with `pocket_money_2026-03-14.md` (Run #1 — 22 scenarios excluded) AND `pocket_money_crossref_2026-03-14.md` (18 scenarios excluded)
**New scenarios this run:** 16
**Hedged scenarios:** 0

> **What this run demonstrates:** All 6 dictionary gaps from Run #1 are now closed (Section 4 added). Scenarios reference exact policy numbers: 1,000 PLN balance ceiling, 0 PLN hard floor, 90-day rejection reason deletion, max 2 deadline extensions, 7-day auto-unlock SLA, Phase 1 internal ledger + Wishlist Request model, task lifecycle state machine with `rejected` terminal per parent. Zero hedging — every recommendation is specific.

---

### Process: pocket_money

---

#### Step 1: Parent Setup

* **Scenario:** Allowance amount not locked — change fires in same cycle as creation
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: reward amount locked at task creation time. But no equivalent lock is defined for allowance amounts. Parent creates a 30 PLN/week schedule on Monday. Changes it to 50 PLN on Wednesday. The allowance job for this week was already queued at 30 PLN. Does the queued job reflect the old value or the new one?
  * **Potential Impact:**
    * **User (child):** Receives unexpected amount; if lower than anticipated, feels shortchanged with no explanation.
    * **System:** Race condition between allowance config write and queued job execution; undefined which value takes effect.
    * **Business:** Trust in the allowance schedule as a predictable, reliable system is undermined from day one.
  * **Recommendation / Questions for the Team:** Define: allowance amount locks for the current cycle the moment the job is queued (typically at cycle start). Changes apply from the next cycle. Surface in UI: "Your change will take effect from [next cycle date]. This week's allowance: 30 PLN."

* **Scenario:** Child account balance already at 1,000 PLN ceiling when Pocket Money setup completes
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: maximum balance 1,000 PLN; ceiling breach notified at approval time with options. At Parent Setup, the Child Account may already hold 1,000 PLN (e.g., transferred from a sibling account). The first scheduled allowance will immediately hit the ceiling — but the setup flow provides no warning about this.
  * **Potential Impact:**
    * **User (parent):** Completes setup believing Pocket Money is active; first allowance silently fails or queues.
    * **System:** Setup flow doesn't validate current balance against configured allowance schedule.
    * **Business:** First impression of Pocket Money = failure to deliver the first allowance. The feature's credibility is damaged before it has a chance to prove itself.
  * **Recommendation / Questions for the Team:** During Parent Setup, check child's current balance. If balance ≥ 900 PLN: warn parent: "Your child's balance is [X] PLN — close to the 1,000 PLN limit. Their first allowance may queue until they spend down." Surface a spending CTA for the child immediately.

---

#### Step 2: Allowance Trigger

* **Scenario:** Scheduled allowance fires when child balance is 990 PLN — ceiling check at wrong layer
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: 1,000 PLN maximum balance; ceiling breach options (raise ceiling via Platform Admin, partial reward, queue). Ceiling breach handling is defined for parent approval of task rewards — a manual step. Scheduled allowance is an automated job with no parent approval step. A 50 PLN allowance firing when balance is 990 PLN would breach the ceiling by 40 PLN. Who is notified? When? The parent may be asleep at the 09:00 Monday trigger.
  * **Potential Impact:**
    * **User (child):** Receives either a partial allowance (10 PLN, unexplained) or full 50 PLN in violation of the ceiling, or nothing — undefined behavior.
    * **System:** Two separate code paths: ceiling check at task reward approval (manual) vs. ceiling check at scheduled allowance (automated). Likely inconsistent behavior.
    * **Business:** Child receives an unexpected amount on allowance day — first breach of the schedule's reliability promise.
  * **Recommendation / Questions for the Team:** Allowance job must run a ceiling pre-check before transfer. If transfer would breach: (a) issue partial amount up to 1,000 PLN, (b) queue remainder, (c) notify parent at next login: "X PLN queued — [child] needs to spend down first." Add Pocket Money health widget to parent dashboard showing ceiling status and queued amounts.

---

#### Step 3: Task Assignment

* **Scenario:** Task created with a reward that would breach the ceiling — validation deferred to approval
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: reward locked at task creation time; ceiling breach notified at approval time with options. Child's balance is 980 PLN. Parent creates a task with a 50 PLN reward (980 + 50 = 1,030 PLN — ceiling breach by 30 PLN). No warning at creation. Task proceeds through `open` → `in_progress` → `submitted` — only to encounter the ceiling problem at approval, after the child has already completed the work.
  * **Potential Impact:**
    * **User (child):** Completes task in good faith; reward withheld at approval due to ceiling breach. The moment of maximum effort is also the moment of reward failure.
    * **System:** No pre-validation of (current balance + task reward) against 1,000 PLN ceiling at task creation time.
    * **Business:** Child experienced the full effort of completing a task with no reward payoff — worst possible timing for a ceiling enforcement.
  * **Recommendation / Questions for the Team:** At task creation, check: current balance + task reward > 1,000 PLN? If yes, soft warning to parent: "This reward may not transfer in full if [child's name] doesn't spend down before completion. Balance now: 980 PLN, reward: 50 PLN." Don't block — but eliminate the surprise at approval.

* **Scenario:** Task cancelled in `in_progress` state — no mandatory cancellation reason
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: once task moves to `in_progress`, parent cannot silently delete — must cancel with child notification. But the cancellation policy defines no mandatory reason (unlike task rejection, which by recommendation requires mandatory reason text). Child receives "Task cancelled" with no explanation — halfway through completing the work.
  * **Potential Impact:**
    * **User (child):** Invested effort in a task that was cancelled without explanation; feels disrespected; loses motivation.
    * **System:** Cancellation has no mandatory reason field; notification text contains no context beyond the cancellation itself.
    * **Business:** A cancellation without reason at the `in_progress` stage destroys the fairness contract that makes the task system educationally credible.
  * **Recommendation / Questions for the Team:** Require mandatory cancellation reason when task is in `in_progress` state — same policy as rejection. Provide preset reason chips: "Task no longer needed," "Changed my mind," "Already done by someone else." Reason included in child's cancellation notification.

---

#### Step 4: Child Completes Task

* **Scenario:** Task auto-expires at 90 days with no advance warning to child
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: no child completion within 90 days → auto-transition to `expired` (terminal state). Task review SLA defines parent reminders at T+24h and T+72h post-submission — but no expiry warnings are defined for the child. At exactly day 90, the task silently becomes terminal with no T-7 or T-1 warning.
  * **Potential Impact:**
    * **User (child):** Planned to complete the task over the weekend at day 89. Wakes up to find the task gone — missed reward, no explanation.
    * **System:** Expiry job runs at T+90 with no pre-expiry notification workflow for child-facing alerts.
    * **Business:** Silent task expiry at the exact moment a child planned to act = frustration + disengagement from the task system.
  * **Recommendation / Questions for the Team:** Add pre-expiry notification workflow: child notified at T-7 days and T-1 day before task expiry: "Your task '[name]' expires on [date] — complete it to earn [reward] PLN." Parent also notified at T-7 that the task will auto-expire.

* **Scenario:** Child attempts to resubmit after `rejected` state — state machine undefined for this action
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: `rejected` is terminal per parent; the rejecting parent can unreject; the second parent cannot override. But the dictionary does not define what happens if the child views the rejection as feedback and attempts to mark the task complete again (create a new submission). Is a second `submitted` state possible from a `rejected` task?
  * **Potential Impact:**
    * **User (child):** Improved their work and tried to resubmit; unclear if the new attempt replaces the rejection or creates a second conflicting record.
    * **System:** `rejected` defined as terminal per parent — but "terminal" describes the parent's decision authority, not necessarily the child's ability to reopen the task.
    * **Business:** The educational intent of rejection is to teach improvement. If resubmission is technically impossible, the lesson has no mechanism for completion.
  * **Recommendation / Questions for the Team:** Define explicitly: after `rejected`, can the child create a new submission attempt? Proposed: allow one resubmission (opens fresh `submitted` state). Second rejection = permanently terminal. Surface the resubmission window prominently in the rejection notification: "Your parent left feedback — make the changes and submit again."

---

#### Step 6: Reward Distribution

* **Scenario:** Queued reward (ceiling breach option) — no auto-trigger, no expiry, no child visibility
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: if approved reward would breach 1,000 PLN ceiling, parent has option to "queue reward until child spends down." The dictionary defines this option but not its mechanics: Does the queued reward auto-transfer when balance drops below (1,000 - queued_amount)? Does it expire? Does the child know they have a queued reward?
  * **Potential Impact:**
    * **User (child):** Parent approved the reward; child's balance never moves; doesn't know a queued reward exists — feels the approval was meaningless.
    * **System:** Queue mechanism defined as an option in policy but not designed: no auto-trigger, no expiry policy, no UI surface.
    * **Business:** An approved reward that never arrives = broken promise at the exact moment of maximum child-parent trust. Worse than the ceiling breach itself.
  * **Recommendation / Questions for the Team:** Define queued reward behavior fully: (a) auto-transfers when balance drops below (1,000 PLN - queued_amount); (b) no expiry on approved queued rewards; (c) both parent and child see "Pending reward: X PLN — waiting for spending room" in balance view; (d) queue processed by the same scheduled job as allowance distribution to maintain consistency.

---

#### Step 7: Child Spending

* **Scenario:** 100% of balance ring-fenced for savings goal — child sees full balance but cannot spend
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: goal funds ring-fenced from general spending balance; child cannot spend goal-allocated funds on Wishlist Requests. Child has 200 PLN total balance and sets a savings goal of 200 PLN → 100% ring-fenced. Child opens Wishlist Request screen; displayed balance shows 200 PLN. But effective spending balance is 0 PLN. Wishlist Request blocked.
  * **Potential Impact:**
    * **User (child):** Sees 200 PLN in their account but cannot spend any of it — the app appears to be lying about their balance.
    * **System:** Total balance and spendable balance are two distinct values; UI likely displays only one.
    * **Business:** Child loses trust in the balance display — the foundational UI element of Pocket Money.
  * **Recommendation / Questions for the Team:** Display two distinct balance figures prominently: "Total: 200 PLN | Available to spend: 0 PLN (200 PLN locked in savings goal)." On Wishlist Request screen, show the breakdown of why the balance is unavailable. Soft warning at goal creation if it would reduce spendable balance to 0 PLN: "Setting this goal will lock all your current balance."

* **Scenario:** Wishlist Request approval decrements balance before parent confirms purchase
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: Wishlist Request — child selects item, parent reviews and approves; on approval, parent funds purchase from own account; child balance decremented as if spent. The decrement happens at approval time, but the actual physical purchase (parent buying the item in the real world) may happen days later — or not at all.
  * **Potential Impact:**
    * **User (child):** Balance drops by 150 PLN for headphones; child expects immediate receipt; parent meant "I'll buy it next shopping trip." Child sees reduced balance for an item they don't have yet.
    * **System:** Approval = balance decrement with no purchase confirmation step or linkage to real-world fulfillment.
    * **Business:** Child loses balance for an item the parent forgets to buy — trust in the Wishlist feature as a real spending tool eroded.
  * **Recommendation / Questions for the Team:** Two-step approval: (1) Parent confirms intent — "Approved: I'll buy this for [child's name]" (no balance change); (2) Parent confirms purchase completion — "I bought it" → balance decremented. Until step 2: child sees "Approved — purchase pending" (not yet deducted). Prevents balance decrement for items never purchased.

* **Scenario:** Wishlist Request rejection — no mandatory reason requirement defined
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: task rejection has a mandatory reason field (recommended). Wishlist Request rejection policy is not defined in the dictionary. If reason is optional, child receives "Request denied" with no context — the educational moment (why this purchase wasn't appropriate) is lost.
  * **Potential Impact:**
    * **User (child):** Denied a purchase with no explanation; doesn't know how to save toward an acceptable alternative or what criteria to meet.
    * **System:** Wishlist rejection reason field not defined in policy; no minimum character requirement.
    * **Business:** Inconsistency: task rejections are educational (mandatory reason); Wishlist rejections are silent. Same child, same app, opposite experience.
  * **Recommendation / Questions for the Team:** Require mandatory reason for Wishlist rejections, mirroring task rejection policy. Preset chips: "Too expensive right now," "Save more first," "Let's discuss this," "Not the right time." Reason visible in child's request history to facilitate the financial conversation.

---

#### Step 8: Savings Goal

* **Scenario:** Second deadline extension granted with no warning that it is the last permitted extension
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: maximum 2 deadline extensions per goal; on 3rd extension attempt, parent prompted to adjust goal amount or mark abandoned. But the policy defines no warning when granting the 2nd extension. Parent extends for the 2nd time unaware they have used their final extension. The restriction is discovered only on the 3rd attempt — with no preparation.
  * **Potential Impact:**
    * **User (parent):** Surprised by the hard stop on the 3rd attempt; may try to circumvent by deleting and recreating the goal to reset the counter.
    * **System:** Extension count tracked in backend but not surfaced in UI until the limit is hit.
    * **Business:** If parents cancel and recreate goals to circumvent the 2-extension limit, the policy fails entirely.
  * **Recommendation / Questions for the Team:** On granting the 2nd extension, explicitly inform parent: "This is the last extension permitted for this goal. If the new deadline passes, you'll need to adjust the goal amount or mark it as abandoned." Display an extension counter in goal detail: "Extensions used: 1 of 2" → "Extensions used: 2 of 2 — final extension."

---

#### Step 9: Goal Achievement

* **Scenario:** Age-18 transition arrives while savings goal is active and parent unlock is pending
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: at age 18, Child Account transitions to Adult Account — parental visibility removed, full data ownership transferred, notifications at T-30 and T-0. No cascade is defined for active savings goals requiring parent unlock, open tasks in `in_progress`, or pending Wishlist Requests. At T-0, parental access is revoked — but the goal achievement flow still calls for "parent unlock."
  * **Potential Impact:**
    * **User (now-adult):** Has an achieved savings goal with funds locked pending parent unlock — but parental access is gone. Goal funds may be permanently inaccessible.
    * **User (parent):** Loses visibility into a goal they partially funded; open task approval requests drop from their dashboard with no notification.
    * **System:** Age-18 transition cascade not defined: tasks, goals, Wishlist Requests all have pending parent actions that become impossible post-transition.
    * **Business:** The age-18 event is GDPR-critical (parental consent no longer applies). An incomplete cascade creates simultaneous compliance failure and broken UX.
  * **Recommendation / Questions for the Team:** Define transition cascade explicitly: (a) open tasks → auto-expire at T-0, both parties notified; (b) active savings goals → convert to adult-managed goals (parent unlock removed; now-adult unlocks independently); (c) pending Wishlist Requests → cancelled with explanation. T-30 day warning must list all pending items affected by transition so parent can act before access is removed.

---

#### Step 10: Transaction History

* **Scenario:** `rejected` status visible in history after 90-day reason deletion — context orphaned
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: rejection reason text auto-deleted 90 days after task rejection; transaction event (amount, date, task name, status) retained indefinitely. A child reviewing their history at age 16 sees a `rejected` status from age 14 — but the reason text is gone. The rejection record exists without the context that gave it educational meaning.
  * **Potential Impact:**
    * **User (child):** Sees a rejection with no explanation; may feel confused or wronged with no access to context.
    * **System:** Retention rules correctly implemented (text deleted, event retained) but the combination creates an orphaned `rejected` status in the UI.
    * **Business:** Long-term transaction history becomes selectively meaningful — rejections without reasons look arbitrary in retrospect.
  * **Recommendation / Questions for the Team:** When displaying a `rejected` task with no reason available (post-90 days): show a placeholder — "[Rejection reason was removed after 90 days per our data retention policy]." Never render an empty field — contextualize the absence. Document this behavior explicitly in the children's privacy policy and the parent-facing data retention notice.

* **Scenario:** Platform Admin access notification suppressed indefinitely via self-certified investigation exception
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: admin access to Child Account data logged with mandatory reason field; Parent Account notified within 24h (except where notification would compromise an active investigation). This exception has no defined time limit, no oversight mechanism, and no audit requirement for investigation closure. Platform Admin Account can self-certify "active investigation" to suppress parent notifications indefinitely.
  * **Potential Impact:**
    * **User (parent):** Never notified of admin access to their child's data; assumes no access has occurred.
    * **System:** Exception clause with no investigation ID requirement, no expiry, and no secondary approval for the suppression.
    * **Business:** GDPR Article 8 requires heightened protection for children's data. A self-certified investigation exception without time limits = structural regulatory exposure in PL (UODO), CZ (ÚOOÚ), and SK (ÚOOÚ SK) jurisdictions.
  * **Recommendation / Questions for the Team:** Define investigation exception rigorously: (a) exception requires a documented investigation ID, not just a reason string; (b) notification suppression auto-expires at 30 days — must be renewed with documented justification; (c) after investigation closes, parent is retroactively notified of the access (with redacted reason where legally justified); (d) quarterly audit of all investigation-exception accesses required, reviewed by a second Platform Admin Account to prevent self-authorization. Document legal basis in privacy policy before launch.

---

## Quality Checklist

- [x] All 4 categories covered: User Errors, System/Technical, External Dependencies, Business Logic & Edge Cases
- [x] Dictionary fully loaded: Sections 2–4 — 1,000 PLN ceiling, 0 PLN hard floor, 90-day rejection text deletion, max 2 deadline extensions, 7-day auto-unlock SLA, Phase 1 Wishlist Request model, task lifecycle states (`open` → `in_progress` → `submitted` → `approved`/`rejected`/`expired`/`cancelled`), `rejected` terminal per parent, reward locked at task creation, ring-fencing, single-currency lock
- [x] Every scenario has User / System / Business impact
- [x] All recommendations specific and actionable — zero scaffolding
- [x] **0 scenarios duplicated from `pocket_money_2026-03-14.md` (Run #1 — 22 excluded)**
- [x] **0 scenarios duplicated from `pocket_money_crossref_2026-03-14.md` (18 excluded)**
- [x] **0 hedged scenarios (⚠️) — all 6 dictionary gaps from Run #1 closed**
- [x] 16 new scenarios across relevant steps
- [x] Date format ISO: `2026-03-14`

---

## Run Comparison — Pocket Money

| Run | Dictionary loaded | Scenarios | Hedged (⚠️) | Sample recommendation quality |
|---|---|---|---|---|
| Run #1 | Sections 2–3 | 22 | 0 | "Define child account balance ceiling in dictionary" (gap, no number) |
| Cross-ref | Sections 2–3 | 18 | 4 | "⚠️ Requires dictionary gap closure. Define task review SLA." |
| Run #3 | Sections 2–4 full | 16 | 0 | "Allowance job pre-checks: if balance + amount > 1,000 PLN → issue partial to ceiling, queue remainder, notify parent at next login" |

**Same process. Zero duplication. Third layer — specific numbers, specific mechanisms, zero scaffolding.**

---

*Generated: 2026-03-14 | Command: `/detect-missing-requirements` | Constraint: exclude Run #1 + cross-ref | Dictionary: Sections 2–4 fully loaded | Framework: `skill:risk-analysis-framework` | Structure A*
