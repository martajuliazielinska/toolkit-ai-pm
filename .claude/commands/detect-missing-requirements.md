# Command: /detect-missing-requirements

**Description:** Analyze a product process to identify risks, edge cases, and unhappy paths before development.

---

## Purpose

Activates the risk-analysis-framework skill to systematically uncover:
- Where users can make mistakes (User Errors)
- Where systems can fail (Technical Errors)
- Where external dependencies break (External Dependencies)
- Where edge cases exist (Business Logic & Edge Cases)

Instead of brainstorming "what could go wrong," AI analyzes your actual process structure using @dictionary as domain truth.

---

## Input

Provide ONE of these:

1. **Inline process description:** Describe the process step-by-step
```
   /detect-missing-requirements
   Process: Family onboarding flow
   Steps: 1. Email signup → 2. ID verification → 3. Bank connection → 4. Dashboard access
```

2. **Reference existing process file:**
```
   /detect-missing-requirements for process in @process_family_onboarding.md
```

3. **Generate scenarios for different categories only:**
```
   /detect-missing-requirements edge-cases-only for @process_financial_consultation.md
```

---

## Workflow

**Step 1:** Activate skill
- Load: `risk-analysis-framework` skill
- Load: `@dictionary` (domain truth source)
- Load: `@CLAUDE.md` (project context)

**Step 2:** Parse input
- Identify: Is this a full analysis or edge-cases-only?
- Identify: Process name and steps

**Step 3:** Run analysis
- For each process step, generate potential failures (4 categories)
- Reference @dictionary terminology consistently
- Assess User/System/Business impact
- Propose recommendations or questions

**Step 4:** Format output
- Use Structure A (full) or Structure B (edge cases only) from skill
- Follow project-structure.md naming convention
- Save to: `strategic-tasks/risks/unhappy_paths/{process_name}_{YYYY-MM-DD}.md`
  OR `strategic-tasks/risks/edge_cases/{process_name}_{YYYY-MM-DD}.md`

**Step 5:** Confirm
- Before saving, ask user: "Ready to save analysis to {path}?"

---

## Output Location

All outputs saved per `project-structure.md`:
- Full analysis → `strategic-tasks/risks/unhappy_paths/{process_name}_{date}.md`
- Edge cases only → `strategic-tasks/risks/edge_cases/{process_name}_{date}.md`

Example: `strategic-tasks/risks/unhappy_paths/financial_consultation_2026-03-14.md`

---

## Examples

### Example 1: Full Analysis (Inline)
```
User: "/detect-missing-requirements for Family Wallet onboarding"

Steps:
1. skill:risk-analysis-framework → analyze "Email signup → verification → bank connection → dashboard"
2. reference @dictionary for: User, FWCS, Bank Aggregation API, External Financial System
3. generate scenarios for User Errors, Technical Errors, External Dependencies, Business Logic
4. assess impact on User, System, Business
5. save to: strategic-tasks/risks/unhappy_paths/family_onboarding_2026-03-14.md
```

### Example 2: Edge Cases Only
```
User: "/detect-missing-requirements edge-cases-only for financial consultation booking"

Steps:
1. skill:risk-analysis-framework → analyze Business Logic & Edge Cases only
2. focus on: race conditions, boundary cases, unusual but valid scenarios
3. save to: strategic-tasks/risks/edge_cases/financial_consultation_2026-03-14.md
```

---

## Notes

- Always reference @dictionary — it is the single source of truth for domain terminology
- Never generate generic fintech risks; tie all scenarios to Family Wallet context
- Minimum 8-12 scenarios per analysis (more is better)
- User must confirm before saving output
- If process description is vague, ask clarifying questions before analyzing
