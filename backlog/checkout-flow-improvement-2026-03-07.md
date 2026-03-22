# Epic: Checkout Flow Improvement
**Goal:** Reduce payment-step abandonment by adding Apple Pay / Google Pay and simplifying address entry.
**Out of scope:**
- Buy Now Pay Later integrations
- Logged-in vs guest flow redesign
- Backend address validation changes
- Markets other than UK and US

---

## Story 1: Apple Pay Integration
**As a** mobile shopper **I want** to pay with Apple Pay **so that** I can complete checkout without entering card details manually.

**Acceptance Criteria:**
```gherkin
Scenario: Successful Apple Pay payment
  Given I am on the payment step on a Safari/iOS device
  When I tap the Apple Pay button and authenticate
  Then my order is placed and I see the confirmation page

Scenario: Apple Pay unavailable
  Given I am on a non-Apple device or unsupported browser
  When I reach the payment step
  Then Apple Pay button is not shown and standard options remain visible
```

**Technical Tasks:**
- [ ] Integrate Stripe Payment Request API (or existing PSP SDK) for Apple Pay
- [ ] Add Apple Pay button component to payment step UI
- [ ] Register merchant domain with Apple (Apple Pay domain verification)
- [ ] Write E2E test for successful and fallback scenarios

**Dependencies:** Confirm PSP (Stripe/Braintree/Adyen) supports Apple Pay on current account
**Estimate:** L
**Flagged assumptions:** [ASSUMPTION: Stripe is the current PSP]

---

## Story 2: Google Pay Integration
**As a** Android/Chrome shopper **I want** to pay with Google Pay **so that** I can complete checkout in two taps.

**Acceptance Criteria:**
```gherkin
Scenario: Successful Google Pay payment
  Given I am on Chrome or an Android device
  When I tap the Google Pay button and confirm in the Google Pay sheet
  Then my order is placed and I see the confirmation page

Scenario: Google Pay unavailable
  Given I am on an unsupported browser
  When I reach the payment step
  Then Google Pay button is hidden and standard options remain visible
```

**Technical Tasks:**
- [ ] Integrate Google Pay API via PSP SDK
- [ ] Add Google Pay button to payment step alongside Apple Pay
- [ ] Test on Android Chrome and desktop Chrome
- [ ] Ensure button rendering meets Google Pay brand guidelines

**Dependencies:** Story 1 (shared payment button component)
**Estimate:** M
**Flagged assumptions:** [ASSUMPTION: Same PSP handles Google Pay]

---

## Story 3: Address Form Simplification
**As a** shopper at checkout **I want** a shorter, autofill-friendly address form **so that** I can complete my address in fewer steps.

**Acceptance Criteria:**
```gherkin
Scenario: Postcode lookup autofills address
  Given I am on the address step
  When I enter my postcode and select my address from the dropdown
  Then street, city and county fields are populated automatically

Scenario: Manual entry still works
  Given the postcode lookup returns no results
  When I manually fill in all address fields
  Then I can proceed to the next step without errors
```

**Technical Tasks:**
- [ ] Integrate postcode lookup API (e.g. Loqate or Ideal Postcodes) for UK
- [ ] Reduce visible fields: hide County and Title unless manually expanded
- [ ] Ensure autofill attribute values are correct (autocomplete="address-line1" etc.)
- [ ] Regression test: existing address validation still passes

**Dependencies:** None
**Estimate:** M
**Flagged assumptions:** [ASSUMPTION: UI-only change, no backend validation logic touched]

---

## Story 4: Checkout Performance — Quick Wins
**As a** shopper **I want** the checkout page to load quickly **so that** I don't abandon due to slow experience.

**Acceptance Criteria:**
```gherkin
Scenario: Payment step loads within acceptable time
  Given I proceed from cart to checkout
  When the payment step renders
  Then it is interactive within 3 seconds on a 4G connection (measured via Lighthouse)
```

**Technical Tasks:**
- [ ] Run Lighthouse audit on current checkout — identify top 3 bottlenecks
- [ ] Lazy-load payment method scripts (Apple Pay / Google Pay SDKs)
- [ ] Remove unused CSS/JS loaded on checkout page
- [ ] Re-run Lighthouse post-changes and document delta

**Dependencies:** Stories 1 and 2 (scripts must be lazy-loaded from the start)
**Estimate:** M
**Flagged assumptions:** [ASSUMPTION: No hard performance SLA — Lighthouse score improvement is sufficient]

---

## Risk Report

### ✅ Ready to implement
- Story 3 (Address Form) — scope is clear, no external blockers
- Story 4 (Performance) — self-contained audit-and-fix loop

### ⚠️ Needs clarification before sprint
- **Story 1 & 2:** Confirm PSP name and whether Apple/Google Pay is already enabled on the account — this changes estimate significantly.

### 🔴 Blocked — decision required
- **Story 1:** Apple Pay requires merchant domain verification — needs DevOps/infra access. Assign owner before sprint starts.

### 💡 Technical risks
- **PSP SDK version:** If the current Magento PSP extension is outdated, Apple/Google Pay APIs may not be exposed — spike recommended (0.5 day).
- **Address API cost:** Postcode lookup APIs are pay-per-request — confirm budget approval before Story 3 goes to dev.

### 📋 Definition of Ready check
- [x] All stories have AC
- [x] Dependencies are mapped
- [x] Estimates assigned
- [ ] PSP confirmation — OPEN
- [ ] Apple Pay domain ownership — OPEN
