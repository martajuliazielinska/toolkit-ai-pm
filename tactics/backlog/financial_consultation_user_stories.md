# Financial Consultation User Stories

## US-1: Browse Available Advisors

**As a** Family account holder
**I want to** view a list of available financial advisors with their specializations and rates
**So that** I can choose an advisor that matches my needs

### Acceptance Criteria
- Display advisor name, specialization, hourly rate
- Show advisor rating (average from past consultations)
- Filter by specialization (optional)

---

## US-2: Book and Pay for Consultation

**As a** Family account holder
**I want to** book a consultation with an advisor and pay for it in the app
**So that** the consultation is confirmed and scheduled

### Acceptance Criteria
- Select available time slot from advisor's calendar
- One-time payment (separate from subscription)
- Receive payment confirmation
- Automatically generate and display video conference link

---

## US-3: Share Financial Data with Advisor

**As a** Family account holder
**I want to** share selected financial data with my advisor before the consultation
**So that** the advisor has context for our discussion

### Acceptance Criteria
- User generates Financial Report (chooses which data to share)
- Report types: Expense analysis, Savings goals, Custom selection
- Advisor receives read-only access to shared report (Snapshot Model)
- Push notification when consultation starts

---

## US-4: Receive Advisor Summary and Recommendations

**As a** Family account holder
**I want to** see the advisor's observations and recommendations after our consultation
**So that** I have documented insights to act on

### Acceptance Criteria
- Summary displays in consultation history
- Contains: Observations (text field) and Recommendations (text field)
- Recommendations can suggest creating new savings goals
- User can reference summary in future sessions

---

## US-5: Rate Consultation Experience

**As a** Family account holder
**I want to** rate my consultation experience with the advisor
**So that** other users can see advisor quality and advisors get feedback

### Acceptance Criteria
- 5-star rating system
- Optional comment field
- Rating contributes to advisor's average rating calculation
- Rating visible in advisor profile
