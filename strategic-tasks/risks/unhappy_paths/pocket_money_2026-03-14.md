# Unhappy Path Analysis: Pocket Money Flow

**Analysis Date:** 2026-03-14
**Source:** `strategic-tasks/test_process_pocket_money.md`
**Framework:** `skill:risk-analysis-framework` · Structure A (Full Analysis)
**Dictionary:** `domain/dictionary.md` ✓ loaded (Sections 2–3)
**Run:** #1
**Total scenarios:** 22 across 10 steps

> **New process context vs. Financial Consultation:** Child Account as primary actor (not just access control object), scheduled system jobs, photo uploads, parental approval workflows, virtual wallet, savings goals, GDPR Article 8 (children's data).

---

### Process: pocket_money

---

#### Step 1: Parent Setup

* **Scenario:** Two parents configure conflicting allowance settings for the same child
  * **Category:** Business Logic and Edge Cases
  * **Description:** Dictionary: Parent Account has "full access to family finances, child monitoring." A family may have two Parent Accounts (mother + father). Both configure Pocket Money for the same child — one sets 20 PLN/week, the other sets 50 PLN/month. Which setting wins? Does the second override the first silently?
  * **Potential Impact:**
    * **User (child):** Receives an amount neither parent intended; allowance unpredictable.
    * **System:** Last-write-wins on allowance configuration — no conflict detection, no audit trail of who changed what.
    * **Business:** Parental dispute over the app's behavior; trust in the product as a "family finance" tool damaged.
  * **Recommendation / Questions for the Team:** Define ownership model for Pocket Money configuration: either one designated "primary parent" per child, OR require both parents to confirm changes above a threshold (e.g., >20% change in allowance amount). Log all configuration changes with timestamp and Parent Account ID.

* **Scenario:** Parent sets allowance amount to 0 PLN
  * **Category:** User Errors
  * **Description:** Parent creates Pocket Money account with 0 PLN allowance — either intentionally (task-only rewards) or by mistake. System accepts 0 as a valid value and schedules recurring zero-value transfers.
  * **Potential Impact:**
    * **User (child):** Sees scheduled "allowance" notifications for 0 PLN — confusing and demotivating.
    * **System:** Scheduled zero-value transactions created; may cause downstream issues in transaction history display.
    * **Business:** Child perceives app as broken; parent gets support complaint from child.
  * **Recommendation / Questions for the Team:** If allowance = 0, prompt parent: "You've set a 0 PLN allowance. Are you using task-only rewards? You can skip the base allowance." Zero allowance should be explicit opt-in, not a silent default.

* **Scenario:** Parent changes allowance frequency mid-cycle
  * **Category:** User Errors
  * **Description:** Parent sets weekly allowance (every Monday). On Wednesday, they switch to monthly. The child was expecting a payment that no longer comes — or comes at an unexpected time. The system doesn't communicate when the next payment under the new schedule will fire.
  * **Potential Impact:**
    * **User (child):** Confused about when next allowance arrives; may feel punished.
    * **User (parent):** Doesn't realize the change affects the current cycle.
    * **System:** Allowance trigger recalculates next fire date from the change moment — old schedule abandoned without proration.
    * **Business:** Child complains to parent; parent blames the app.
  * **Recommendation / Questions for the Team:** When frequency changes, show parent: "Next allowance under new schedule: [date]. Any pending payment under the old schedule will [be paid / be skipped — define policy]." Require explicit confirmation before applying frequency changes mid-cycle.

---

#### Step 2: Allowance Trigger

* **Scenario:** Scheduled job fails silently — no allowance distributed, no notification
  * **Category:** System/Technical Errors
  * **Description:** Allowance is a scheduled system job (cron/worker). If the job fails — server error, database timeout, queue overflow — the allowance is not distributed. No notification sent to parent or child. Child waits; parent assumes it worked.
  * **Potential Impact:**
    * **User (child):** Expected allowance doesn't arrive; no explanation.
    * **System:** Job failure not surfaced to users; no retry logic defined.
    * **Business:** Pocket Money adoption is the primary PMF signal (+5pp MoM). Silent failure of the core automatic feature is existential for this feature's credibility.
  * **Recommendation / Questions for the Team:** All allowance jobs must have: (a) retry on failure (3 attempts with exponential backoff), (b) failure alert to Platform Admin Account within 5 minutes, (c) user-facing notification if allowance is delayed >1 hour past scheduled time. Never fail silently on money movement.

* **Scenario:** Allowance fires twice on DST change day
  * **Category:** Business Logic and Edge Cases
  * **Description:** Allowance scheduled for "every Monday at 09:00." In late March (CEE DST transition), clocks move forward. Depending on how the scheduled job stores times (UTC vs. local), the job may fire at 09:00 local time AND again when UTC-mapped time recalculates — double-paying for one week.
  * **Potential Impact:**
    * **User (child):** Receives double allowance — initially happy, but creates accounting inconsistency.
    * **System:** Idempotency not enforced on allowance jobs for same period; two transactions for the same allowance cycle.
    * **Business:** Financial liability; if reversed, child is upset; if not caught, cumulative error over time.
  * **Recommendation / Questions for the Team:** All scheduled jobs must store trigger times in UTC. Enforce idempotency key per allowance cycle: `(child_account_id, allowance_period)` — second trigger for same period rejected as duplicate. Test all scheduled jobs against DST transition dates for PL, CZ, SK markets.

* **Scenario:** Child account suspended or deleted between schedule creation and trigger
  * **Category:** Business Logic and Edge Cases
  * **Description:** Parent sets up weekly allowance. Child account is later suspended or deleted. The allowance job fires on schedule — but the destination account no longer exists or is not active.
  * **Potential Impact:**
    * **User:** Allowance transfer fails; funds may go into limbo or create an error state.
    * **System:** Job attempts transfer to inactive account — undefined behavior (error vs. silent skip vs. reversal).
    * **Business:** Money in limbo = financial liability; support ticket.
  * **Recommendation / Questions for the Team:** Allowance job must validate child account status before transfer. If `inactive` or `suspended`: skip transfer, notify Parent Account with reason. If permanently deleted: cancel all pending allowance schedules and notify parent.

---

#### Step 3: Task Assignment

* **Scenario:** Parent deletes task after child has started working on it
  * **Category:** User Errors
  * **Description:** Parent assigns task "Clean bedroom — 5 PLN." Child starts work. Parent deletes the task (changed their mind or created by mistake). Child completes the task, goes to mark it done — the task is gone. No reward, no explanation.
  * **Potential Impact:**
    * **User (child):** Completed work with no reward; feels cheated and demotivated.
    * **System:** Task deletion has no state check; no child notification on deletion.
    * **Business:** Child's trust in Pocket Money as a fair system broken; family conflict blamed on the app.
  * **Recommendation / Questions for the Team:** Define task lifecycle states: `open`, `in_progress`, `submitted`, `approved`, `rejected`. Once a task moves to `in_progress` (child taps "Start"), parent can only cancel — not silently delete. Cancellation sends notification to child: "Task '[name]' was cancelled by parent."

* **Scenario:** No deadline set on task — child completes it 6 months later
  * **Category:** Business Logic and Edge Cases
  * **Description:** Task assignment has no mandatory deadline field. Child completes a task created 6 months ago. Is the reward still valid? Has the reward amount changed? The task and its terms are effectively open-ended indefinitely.
  * **Potential Impact:**
    * **User (parent):** Receives approval request for a task they've forgotten about.
    * **System:** No expiry on open tasks; reward amount at approval vs. assignment time may differ.
    * **Business:** Confusion and disputes over reward legitimacy.
  * **Recommendation / Questions for the Team:** Enforce auto-expiry on tasks with no deadline: if not completed within 90 days, task moves to `expired` with parent notification. Reward amount locked at task creation time — not editable after child starts.

---

#### Step 4: Child Completes Task

* **Scenario:** Photo proof upload contains personally identifiable data — GDPR risk
  * **Category:** Business Logic and Edge Cases
  * **Description:** Child uploads photo proof of task completion (e.g., photo of clean bedroom, completed homework). Photo may contain: other family members' faces, documents with addresses, school materials with child's full name. This data is stored on Family Wallet servers.
  * **Potential Impact:**
    * **User (child):** Personal and household data stored without explicit awareness of what's captured.
    * **System:** No image scanning or EXIF metadata stripping before storage.
    * **Business:** GDPR Article 8 (children's data) + potential Article 9 (sensitive data in images) — significant compliance risk.
  * **Recommendation / Questions for the Team:** Strip EXIF metadata from all uploaded photos before storage. Define retention policy: task photos deleted automatically after parent approval/rejection + 30 days. Legal review: is photo upload necessary, or can tasks be completed with text confirmation only? Consider making photo upload genuinely optional with a less prominent UI.

* **Scenario:** Two children in the same family submit for the same shared task
  * **Category:** Business Logic and Edge Cases
  * **Description:** Parent assigns "Wash the dishes — 3 PLN." Family has two Child Accounts. Both mark the task as done. Parent receives two approval requests for the same task. If they approve both — double reward for single work completed.
  * **Potential Impact:**
    * **User (parent):** Confused by duplicate submissions; may accidentally double-approve.
    * **System:** Task model doesn't support "shared task" or "first-completed wins" logic.
    * **Business:** Double reward payout for single work completed; financial inconsistency.
  * **Recommendation / Questions for the Team:** Add task assignment scope: `individual` (specific child) or `shared` (any child — first completion wins, or split reward). For shared tasks, first submission locks the task and notifies siblings: "This task was already completed by [sibling name]."

---

#### Step 5: Parent Approval

* **Scenario:** Parent rejects task without providing reason — child has no recourse
  * **Category:** User Errors
  * **Description:** Parent rejects task submission. Rejection reason field is optional. Child receives "Task rejected" with no explanation — doesn't know if they did something wrong or if the parent simply changed their mind.
  * **Potential Impact:**
    * **User (child):** Demotivated; doesn't know how to improve; may disengage from Pocket Money entirely.
    * **System:** Rejection recorded with no reason; no learning loop for the child.
    * **Business:** Core educational value proposition undermined — a rejection without reason teaches nothing.
  * **Recommendation / Questions for the Team:** Make rejection reason mandatory (minimum 10 characters). Provide preset reason chips for speed: "Not done fully," "Try again," "Quality not met." Show rejection reason prominently to child alongside their original submission.

* **Scenario:** Both parents approve the same task — double reward issued
  * **Category:** Business Logic and Edge Cases
  * **Description:** Two Parent Accounts exist for the family. Parent A approves a task submission. Before the reward processes, Parent B also sees the pending task and approves it independently. System issues two reward transfers.
  * **Potential Impact:**
    * **User (child):** Receives double reward; discovers inconsistency or exploits it.
    * **System:** No lock on task approval; both parents can act simultaneously without conflict detection.
    * **Business:** Double reward payout; financial inconsistency; potential exploitation pattern.
  * **Recommendation / Questions for the Team:** Once a task submission enters review, lock it to the first parent who opens it (optimistic lock, 10-minute hold). After one parent approves or rejects, task moves to terminal state — second parent sees "Already reviewed by [Parent name]."

* **Scenario:** Parent approval delayed >7 days — child's motivation lost
  * **Category:** User Errors
  * **Description:** Parent is on holiday, sick, or forgets. Task sits in `submitted` state for 10 days. No reminder sent. Child checks the app daily, sees no update, loses interest in the Pocket Money system.
  * **Potential Impact:**
    * **User (child):** Disengages from Pocket Money; feature adoption drops.
    * **System:** No SLA or escalation on pending task approvals.
    * **Business:** Task approval delays directly kill the feedback loop that makes Pocket Money engaging for children.
  * **Recommendation / Questions for the Team:** Send parent reminder at T+24h and T+72h after task submission. At T+7 days: escalate to second Parent Account (if exists) or notify child that a reminder was sent. Define auto-approval policy: does the system auto-approve after N days? This is a product decision with significant educational implications.

---

#### Step 6: Reward Distribution

* **Scenario:** Reward amount would exceed child account balance ceiling
  * **Category:** Business Logic and Edge Cases
  * **Description:** If child account has a balance ceiling (undefined in dictionary — gap identified), parent approves a reward that would push balance above the limit. Transfer fails — but parent sees approval as successful and child sees no balance change.
  * **Potential Impact:**
    * **User (child):** Approved reward by parent not received; no explanation.
    * **System:** Transfer fails at ceiling; discrepancy between approval status and actual balance.
    * **Business:** Approved reward not delivered = trust broken between parent and child, mediated by the app.
  * **Recommendation / Questions for the Team:** Define child account balance ceiling in `domain/dictionary.md`. If ceiling would be exceeded: notify parent at approval time with options (raise ceiling, partial reward, queue until child spends down). Never silently fail an approved reward.

* **Scenario:** Floating-point arithmetic error on reward accumulation
  * **Category:** System/Technical Errors
  * **Description:** Parent sets reward of 2.50 PLN per task. Child completes 100 tasks over 6 months. Total should be 250.00 PLN. If floating-point arithmetic is used in the backend (not fixed-point), cumulative rounding errors may result in 249.97 PLN or 250.03 PLN in the child's balance.
  * **Potential Impact:**
    * **User:** Balance displays incorrectly; discrepancy visible in transaction history.
    * **System:** Floating-point money arithmetic — a classic fintech error with audit and compliance implications.
    * **Business:** Any money-handling product using floating-point for currency is a compliance and audit risk.
  * **Recommendation / Questions for the Team:** All monetary values must be stored and calculated in integer minor units (groszy, not PLN). Never use floating-point for currency. Display layer converts to decimal for presentation only. Standard fintech requirement — confirm with engineering before launch.

---

#### Step 7: Child Spending

* **Scenario:** Spending mechanics undefined — child accumulates balance with no way to use it
  * **Category:** Business Logic and Edge Cases
  * **Description:** Process states child "can spend from virtual wallet (no external payments yet)." But what internal spending options exist? Is the balance purely informational? The spending mechanics are undefined in both the process and the dictionary.
  * **Potential Impact:**
    * **User (child):** Accumulates balance with no way to use it — Pocket Money becomes a glorified savings tracker.
    * **System:** "Spending" feature may not exist yet but is implied by the UI.
    * **Business:** If children can't spend their balance, the core educational loop (earn → manage → spend → save) is broken — directly undermining the PMF signal.
  * **Recommendation / Questions for the Team:** Define spending mechanics in `domain/dictionary.md` before launch. Options: (a) in-app wishlist — child requests a purchase, parent approves; (b) virtual store with curated items; (c) parent-approved transfer to real account (future). At minimum, "spending" must mean something tangible to the child.

* **Scenario:** Child attempts to spend more than their balance
  * **Category:** User Errors
  * **Description:** Child tries to initiate a spending action exceeding their current balance. Does the system block it with a clear message? Does it allow overdraft? Does it notify the parent to top up?
  * **Potential Impact:**
    * **User (child):** Confusing error message or silent failure; a child seeing a negative balance is a poor financial education moment.
    * **System:** Overdraft logic not defined; may allow negative balance or fail without explanation.
    * **Business:** Negative balance in a child's "own" account contradicts the product's financial education mission.
  * **Recommendation / Questions for the Team:** Child account must have hard floor at 0 PLN — no overdraft. On insufficient funds: "You need X more PLN for this. You could earn it by completing tasks!" Surface relevant open tasks in the same screen — direct educational moment and engagement hook.

---

#### Step 8: Savings Goal

* **Scenario:** Child sets savings goal unreachable in a realistic timeframe
  * **Category:** User Errors
  * **Description:** Child (age 10) sets a savings goal of 10,000 PLN. Allowance is 20 PLN/week. At current rate, the goal takes ~500 weeks (9.6 years). System accepts this and displays a progress bar permanently near 0%.
  * **Potential Impact:**
    * **User (child):** Progress bar never moves meaningfully; child disengages from savings feature.
    * **System:** No validation against realistic timeframe; no feedback on goal achievability.
    * **Business:** Savings goal feature fails its educational purpose — teaching children realistic financial planning.
  * **Recommendation / Questions for the Team:** On goal creation, calculate and display estimated time to goal based on current allowance + average task rewards. If estimated time >12 months, surface a nudge: "At your current allowance, this would take about [X months]. Would you like to set a smaller stepping-stone goal first?" Don't block — educate.

* **Scenario:** Parent deletes child account with active savings goal and non-zero balance
  * **Category:** Business Logic and Edge Cases
  * **Description:** Child has been saving toward a goal for 3 months, accumulated 180 PLN toward a 250 PLN target. Parent deletes the child account. What happens to the 180 PLN balance?
  * **Potential Impact:**
    * **User (child):** 3 months of savings effort erased; no warning, no recourse.
    * **System:** Child account deletion may not handle mid-progress savings goals; balance disposition undefined.
    * **Business:** Funds in limbo = financial liability; potential regulatory issue (funds disposition on account closure).
  * **Recommendation / Questions for the Team:** Account deletion flow must surface active savings goals and balances to the parent: "This child has 180 PLN saved toward a goal. Before deleting: transfer to your account, transfer to another child, or withdraw." No child account with non-zero balance should be deletable without explicit balance disposition confirmation.

---

#### Step 9: Goal Achievement

* **Scenario:** Parent never unlocks funds after goal is reached — child waits indefinitely
  * **Category:** User Errors
  * **Description:** Child reaches savings goal. System shows "Goal achieved!" But the process states "parent can unlock access to goal funds" — with no defined timeline. Parent can simply not act. Child's funds are locked in a completed goal indefinitely.
  * **Potential Impact:**
    * **User (child):** Achieved the goal but cannot access the reward; feels cheated by the system.
    * **System:** No SLA or escalation on goal unlock; goal stays in `achieved` state indefinitely.
    * **Business:** Goal achievement is the highest emotional peak in Pocket Money — locking funds without parent action at exactly this moment destroys the payoff.
  * **Recommendation / Questions for the Team:** Auto-unlock after 7 days if parent doesn't act. Send parent reminders at T+1, T+3, T+7 days. At T+7: funds auto-unlocked with notification to both. Allow parent to pre-configure "auto-unlock goals immediately" during Parent Setup.

* **Scenario:** Parent extends goal deadline repeatedly — goal loses meaning as a commitment device
  * **Category:** User Errors
  * **Description:** Child falls short of deadline. Parent extends it — once, twice, three times. Goal originally set for Christmas has now been extended to the following summer. The deadline loses its function as a financial commitment tool.
  * **Potential Impact:**
    * **User (child):** Learns that deadlines are flexible and don't need to be taken seriously — opposite of the intended financial education.
    * **System:** No limit on deadline extensions; no visibility into extension history.
    * **Business:** Core educational value proposition undermined; Pocket Money teaches that goals are optional.
  * **Recommendation / Questions for the Team:** Limit deadline extensions to 2 per goal. On third extension attempt, prompt parent: "You've extended this goal twice. Would you like to adjust the goal amount instead, or mark it as abandoned?" Show extension history in goal detail — visible to both parent and child.

---

#### Step 10: Transaction History

* **Scenario:** Rejection reasons persist indefinitely — GDPR right to erasure + emotional impact
  * **Category:** Business Logic and Edge Cases
  * **Description:** Rejection reasons (mandatory text) written by a parent are stored in the child's transaction record. Harsh language persists permanently. Child has GDPR right to erasure (Article 17). Additionally, Parent B (second Parent Account) can see rejection reasons written by Parent A.
  * **Potential Impact:**
    * **User (child):** Negative parental language permanently visible in their own account history.
    * **System:** Rejection reasons stored as plain text with no retention policy or content guidance.
    * **Business:** Family conflict mediated by the app's data; GDPR Article 17 erasure requests must be supportable.
  * **Recommendation / Questions for the Team:** Auto-delete rejection reason text after 90 days (retain the transaction event, not the text). Provide parent guidance at time of writing: "Be constructive — your child will see this message." Consider character limit and tone guidance in the rejection UI.

* **Scenario:** Transaction history export — GDPR data portability for child accounts
  * **Category:** Business Logic and Edge Cases
  * **Description:** GDPR Article 20 grants data subjects the right to data portability. A child (or their parent as legal guardian) may request export of all transaction history. Does the system support this? Does the export include task photos (large binary data)?
  * **Potential Impact:**
    * **User:** Cannot exercise their GDPR right to portability — no export feature exists.
    * **System:** No data export mechanism for child account history.
    * **Business:** GDPR non-compliance; regulatory risk particularly acute given children's data heightened protections.
  * **Recommendation / Questions for the Team:** Implement data export for child accounts: CSV of transaction history (task names, amounts, dates). Task photos: provide download links, not inline in export. Export triggered by Parent Account on behalf of child. Delivery via secure email link. Legal review: at what age can a child request their own export independently of their parent?

---

## Quality Checklist

- [x] All 4 categories covered: User Errors, System/Technical, External Dependencies, Business Logic & Edge Cases
- [x] Dictionary terminology used: Child Account, Parent Account, Platform Admin Account
- [x] Every scenario has User / System / Business impact
- [x] All recommendations specific and actionable
- [x] 22 scenarios across 10 steps
- [x] Date format ISO: `2026-03-14`
- [x] `@dictionary` loaded ✓

**New dictionary gaps identified — require update before Run #2:**

| Gap | Priority |
|-----|----------|
| Child account balance ceiling | HIGH |
| Spending mechanics definition ("no external payments yet" is not a definition) | HIGH |
| Task lifecycle states (`open`, `in_progress`, `submitted`, `approved`, `rejected`, `expired`) | HIGH |
| Savings goal extension limit (proposed: 2 per goal) | MEDIUM |
| Goal auto-unlock SLA (proposed: 7 days) | MEDIUM |
| Transaction history retention policy for rejection reason text | MEDIUM |

---

*Generated: 2026-03-14 | Command: `/detect-missing-requirements` | Framework: `skill:risk-analysis-framework` | Process: pocket_money | Run #1 | Structure A*
