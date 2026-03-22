
## 2. Access Control & Roles

* **Parent Account:** Full access to family finances, bank connections, child monitoring
* **Child Account:** Limited access (pocket money only), parental controls active, no payment methods
* **Financial Advisor Account:** Read-only access to client profiles, can create consultation records
* **Platform Admin Account:** Full system access

## 3. Business Policies

* **No-Show Policy:** If either party (User or Advisor) is >15 min late, consultation marked as "no-show"
  * User no-show → 50% refund (retention attempt)
  * Advisor no-show → Full refund to user + penalty to advisor
  * Both no-show → Full refund, advisor flagged

* **Consultation Cancellation (User-initiated):** Free cancellation up to 24h before. After 24h: 25% fee retained by platform. Cancellation deadline displayed in user's local timezone at time of booking.

* **Consultation Cancellation (Advisor-initiated):**
  * Advisor cancels >24h before → Full refund to user, no penalty to advisor
  * Advisor cancels ≤24h before → Full refund to user + standard penalty (10% of session fee) deducted from advisor's next payout
  * Advisor cancels ≤2h before → Full refund to user + elevated penalty (25% of session fee) + booking flagged for Platform Admin Account review
  * Penalty collection fallback: if advisor has no future payouts, deactivation is blocked until penalty is settled. Penalty waived only if advisor account inactive >30 days.
  * Repeat cancellations: 2 cancellations of the same user within 30 days → advisor account flagged. 3 total cancellations within 30 days → automatic suspension pending Platform Admin Account review.
  * Penalty appeal process: advisor submits appeal within 48h with written reason. Platform Admin Account reviews within 5 business days. One appeal permitted per 6-month period. Documented in Advisor Terms & Conditions.

* **Late Join / Partial Session Policy:**
  * Advisor joins T+5 to T+14 min → Pro-rated goodwill credit to user for missed minutes (not a refund)
    * Credit formula: `credit = (minutes_missed / booked_duration) × session_price_paid`, rounded up to nearest PLN
    * Credit surfaced via push notification + visible "My Credits" balance in app. Expires after 90 days.
  * Either party joins T+15 min or later → No-Show Policy applies
  * Session ends before 50% (strictly `< 50%`) of booked duration → Automatic partial refund offered
    * Late Join Credit and Partial Session Refund are mutually exclusive: if both conditions are met, Partial Session Refund takes precedence
  * Minimum session length: 30 minutes. Advisor Panel blocks slot configuration <30 min.

* **Video Conference:** 3rd-party integrated service (Whereby/Whereby equivalent). Max session: 60 min. In-session warning at T+50 min delivered via active session interface (not mobile push only). Advisor Panel blocks slot configuration >60 min.

---

## 4. Pocket Money Definitions

### Task Lifecycle States

| State | Description | Terminal? |
|---|---|---|
| `open` | Task created by parent, not yet started | No |
| `in_progress` | Child tapped "Start" — parent cannot silently delete | No |
| `submitted` | Child marked complete (with or without photo) | No |
| `approved` | Parent approved — reward transfer pending | Yes |
| `rejected` | Parent rejected with mandatory reason — terminal for that parent; second Parent Account cannot override | Yes (per parent) |
| `expired` | No child completion within 90 days — auto-transition | Yes |
| `cancelled` | Parent cancelled after task reached `in_progress` — child notified | Yes |

* **Rejection override rule:** `rejected` is terminal. The rejecting Parent Account can unreject; the second Parent Account cannot override. This preserves parental authority consistency.
* **Task review SLA:** Parent reminded at T+24h and T+72h after submission. At T+7 days: second Parent Account notified (if exists). No auto-approval for tasks (unlike goal auto-unlock).
* **Reward lock:** Reward amount locked at task creation time. Not editable once task moves to `in_progress`.

### Child Account Balance

* **Maximum balance:** 1,000 PLN (or equivalent in local currency at account creation)
* **Minimum balance:** 0 PLN — hard floor, no overdraft permitted
* **Ceiling breach:** If an approved reward would push balance above 1,000 PLN, parent is notified at approval time with options: raise ceiling (Platform Admin required), issue partial reward, or queue reward until child spends down
* **Currency:** Single-currency, locked at Child Account creation. Multi-currency support deferred to CEE expansion Phase 2.

### Spending Mechanics (Phase 1 — Virtual Wallet)

* **Nature:** Internal ledger only. Pocket Money balance does not represent real bank money movement in Phase 1.
* **Available spending action:** "Wishlist Request" — child selects or describes a desired item, parent reviews and approves. On approval, parent funds the purchase from their own account; child balance decremented as if spent.
* **Insufficient funds:** Hard block at 0 PLN. On insufficient funds, child sees: "You need X more PLN. You could earn it by completing tasks!" Open tasks surfaced in the same screen.
* **External payments:** Out of scope for Phase 1. Planned for Phase 2 (parent-approved transfer to real debit card).

### Savings Goal Policy

* **Maximum simultaneous active goals:** 1 per Child Account (focus over fragmentation)
* **Goal target:** Immutable once set. Allowance changes only affect estimated completion date, never the target amount.
* **Goal funds:** Ring-fenced from general spending balance. Child cannot spend goal-allocated funds on Wishlist Requests.
* **Deadline extensions:** Maximum 2 per goal. On 3rd extension attempt: parent prompted to adjust goal amount or mark as abandoned instead.
* **Auto-unlock SLA:** If parent takes no action within 7 days of goal achievement, funds auto-unlock. Parent reminders: T+1, T+3, T+7 days post-achievement.
* **Account deletion with active goal:** No Child Account with non-zero balance deletable without explicit balance disposition (transfer to parent, transfer to sibling, or withdrawal).

### Transaction History & Retention

* **Rejection reason text:** Auto-deleted 90 days after task rejection. Visible to child during those 90 days for educational purposes.
* **Transaction event record** (amount, date, task name, status): Retained indefinitely.
* **Parental visibility:** Expires at child's 18th birthday. At age 18: Child Account transitions to Adult Account, full data ownership transferred, parental access removed. Notification sent to both parties at T-30 days and T-0.
* **Admin access to Child Account data:** Logged with mandatory reason field. Parent Account notified within 24h of any admin access (except where notification would compromise an active investigation). Legal basis documented in privacy policy.
