# Summary: Financial Consultation Module Refinement

**Date:** November 15, 2025
**Participants:** Ewa (PM), Michał (Lead Dev), Kasia (Backend), Piotr (Frontend)

## Discussed Process Points

[technical] Advisor Panel is a completely new service - separate frontend, backend, database, and auth logic required.

[technical] Payment Module needs extension - currently handles subscriptions only, needs one-time service charge support.

[technical] Video Conference Integration required - not building custom WebRTC, use third-party (Jitsi, Zoom, Google Meet, or Whereby).

[business] Data Sharing Model is critical design decision - two options: Live Access (rejected due to GDPR risk) vs Snapshot Model (user controls what data is shared - approved).

[business] Advisor Management initial approach - hardcoded list at launch, 10-15 advisors from one partner firm, manual account creation by admin.

[technical] Consultation Data Model - needs statuses (BOOKED, CONFIRMED, COMPLETED, CANCELLED_BY_USER, CANCELLED_BY_ADVISOR) and fields for observations and recommendations.

## Identified Risks and Challenges

BLOCKER Risk 1 [business]: Advisor data access scope - decision pending from business and legal.

HIGH Risk 2 [business]: Cancellation and refund policy undefined - business analysis required.

Tech Challenge 1 [technical]: Advisor Panel infrastructure - new service means new deployment, monitoring, scaling.

Tech Challenge 2 [technical]: Payment provider API research - cost comparison between providers, one-time charge setup, webhook reliability.

Tech Challenge 3 [technical]: Notification system extension - push when consultation confirmed, deep link to specific consultation.

## Unanswered Questions

[business] Does advisor need admin panel to edit profile, or controlled entirely from main admin panel?

[business] Which video conference provider should we use? (Cost is primary factor per Ewa)

[business] What is the refund and cancellation policy for consultations?

[technical] Should advisor data live in main Postgres or separate database?

## Decisions Made

- Use Snapshot Model (user-controlled data sharing)
- Simple text fields for advisor summary (observations + recommendations)
- Rate advisors with 5-star system
- Manual advisor account creation at launch
- Notification module handles consultation confirmation

## Impact on Artifacts

- @domain/dictionary.md: Updated with Financial Consultation domain concepts
- @tactics/backlog/financial_consultation_user_stories.md: Ready for enrichment based on refinement insights
