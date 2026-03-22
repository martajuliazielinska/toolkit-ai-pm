# Experiment 01 — Epic to Backlog AI Workflow
**Date:** 2026-03-07
**Author:** BA + Claude

## Problem
Engineering teams without a BA receive rough epics from a Delivery Lead
and must self-generate structured backlogs. This leads to unclear scope,
missing AC, and poor estimates.

## Hypothesis
AI can transform a rough epic into a structured backlog (Stories + Tasks + AC + Risks)
in under 10 minutes, usable by a Delivery Lead or engineer without BA support.

## Workflow Designed
1. PROMPT 1 — Epic Intake: Claude restates understanding + asks 3 clarifying questions with defaults
2. PROMPT 2 — Backlog Generation: Claude generates Stories + Gherkin AC + Tasks + Estimates
3. PROMPT 3 — Risk Scan: Claude reviews output and flags blockers, risks, DoR gaps

## Sample Epic Used
"We need to improve the checkout flow. Currently customers abandon at payment step.
We want to add Apple Pay and Google Pay, simplify the address form, and make the whole thing faster."

## Output Generated
→ /backlog/checkout-flow-improvement-2026-03-07.md
- 4 User Stories
- 8 Acceptance Criteria scenarios (Gherkin)
- 15 Technical Tasks
- 2 blockers identified
- 2 technical risks flagged

## Key Decisions
- Scope limited to UK + US markets (default assumption)
- Address form: UI-only change, no backend validation (default assumption)
- No hard performance SLA — Lighthouse improvement as proxy metric

## Next Steps
- [ ] Validate output with a real developer — would they start from this?
- [ ] Test with a second epic (different domain)
- [ ] Document as portfolio artifact for Toolkit.ai
