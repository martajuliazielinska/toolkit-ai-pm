# Module 3 Reconstruction Guide — Setup na Nowym Komputerze

**Data:** 2026-03-15
**Cel:** Odtworzenie pełnego setup'u Module 3 (Risk Analysis System) na dowolnym komputerze.
**Moduł nadrzędny:** Wymaga działającego Module 2 (lub przynajmniej `.claude/` z CLAUDE.md).

---

## 📎 Google Drive

Pełne materiały kursu (slajdy, nagrania, pliki projektowe):
👉 **[WKLEJ LINK DO GOOGLE DRIVE TUTAJ]**

Pliki w Drive:
- `toolkit-pm-example.zip` — gotowy projekt do pobrania
- `modul-3-nagranie.mp4` — sesja z Claude Code
- `dictionary-template.md` — pusty szablon słownika do własnego projektu

---

## ⚡ Quick Start

```
projekt/
├── .claude/
│   ├── CLAUDE.md                                ← zaktualizowany (Module 3 dodany)
│   ├── commands/
│   │   ├── analyze-metrics.md                   [Module 2]
│   │   ├── plan-strategy.md                     [Module 2]
│   │   ├── synthesize-feedback.md               [Module 2]
│   │   └── detect-missing-requirements.md       [Module 3] ← NOWE
│   └── skills/
│       ├── skill-metrics-analysis-framework.md  [Module 2]
│       ├── skill-strategy-framework.md          [Module 2]
│       ├── skill-feedback-synthesis.md          [Module 2]
│       └── risk-analysis-framework.md           [Module 3] ← NOWE
│
├── domain/
│   └── dictionary.md                            [Module 3] ← NOWE
│
├── strategic-tasks/
│   └── risks/
│       ├── unhappy_paths/                       [Module 3] ← NOWE (puste)
│       └── edge_cases/                          [Module 3] ← NOWE (puste)
│
└── project-structure.md                         [Module 3] ← NOWE
```

Utwórz strukturę folderów, wklej pliki z tego przewodnika — gotowe.

---

## ✅ Checklist (w kolejności)

- [ ] 1. Utwórz folder `domain/`
- [ ] 2. Wklej `domain/dictionary.md` (sekcje 2–4 dla Family Wallet, lub własny słownik)
- [ ] 3. Utwórz foldery `strategic-tasks/risks/unhappy_paths/` i `strategic-tasks/risks/edge_cases/`
- [ ] 4. Wklej `project-structure.md` do głównego katalogu
- [ ] 5. Wklej komendę `detect-missing-requirements.md` do `.claude/commands/`
- [ ] 6. Wklej skill `risk-analysis-framework.md` do `.claude/skills/`
- [ ] 7. Zaktualizuj `.claude/CLAUDE.md` (dodaj komendę + skill + Project Principles)
- [ ] 8. Otwórz projekt w Claude Code
- [ ] 9. Weryfikacja (patrz sekcja na końcu)

---

## 📁 KROK 1: Zaktualizuj `.claude/CLAUDE.md`

**Ścieżka:** `.claude/CLAUDE.md`

Dodaj do sekcji `## 🔧 Dostępne Komendy`:
```
- `/detect-missing-requirements` - analiza ryzyk, unhappy paths, brakujących wymagań
```

Dodaj do sekcji `## 📚 Dostępne Skille`:
```
- `skill:risk-analysis-framework` - Framework do analizy ryzyk (4 kategorie × 3 poziomy impact)
```

Dodaj nową sekcję `## Project Principles` (jeśli jeszcze nie istnieje):
```
## Project Principles

- **Role-Based Access:** Parent/Child/Advisor/Admin accounts with distinct permissions
- **No-Show Policy:** Defined cancellation + no-show rules for consultant bookings
```

**Pełna treść pliku po aktualizacji:**

```markdown
# 📊 Toolkit PM - Akademicki Case Study

## 🎯 Cel Projektu
System do zarządzania pracą PM'a z AI. Lustrzane odbicie workflow'u pracy.

## 📁 Struktura Katalogów

### /discovery
Research, competitive analysis, user feedback, market trends
- Gdzie szukamy: konkurencja, użytkownicy, trendy rynkowe

### /strategy
Wizja produktu, SWOT, decyzje strategiczne, roadmap
- Gdzie przechowujemy: strategie, cele, wizja

### /backlog
User Stories, Epiki, requirements, zadania do zrobienia
- Gdzie trzymamy: rzeczy do implementacji

### /execution
Szablony, procesy, how-to guides, standardy
- Gdzie są: instrukcje, formaty, metodyka

### /analytics
Metryki, raporty, analizy, KPI'e
- Gdzie analizujemy: performance, trends

### /logs
Historia decyzji, meeting notes, dlaczego coś podjęliśmy
- Gdzie logujemy: WHY behind decisions

### /.claude
Komendy i skille dla projektu
- /commands - dostępne komendy
- /skills - dostępne umiejętności

## 🔧 Dostępne Komendy
- `/analyze-competitors` - analiza konkurencji
- `/create-user-story` - tworzenie User Story
- `/analyze-metrics` - analiza metryk produktowych
- `/plan-strategy` - planowanie strategii
- `/synthesize-feedback` - synteza feedbacku użytkowników
- `/detect-missing-requirements` - analiza ryzyk, unhappy paths, brakujących wymagań

## 📚 Dostępne Skille
- `skill:user-story-template-jira` - User Story w Jira standardzie
- `skill:metrics-analysis-framework` - Framework do analizy metryk
- `skill:strategy-framework` - Framework do planowania strategii
- `skill:feedback-synthesis` - Framework do syntezy feedbacku
- `skill:risk-analysis-framework` - Framework do analizy ryzyk (4 kategorie × 3 poziomy impact)

## 📋 Reguły Pracy
1. Wszystkie dane strategiczne → /strategy
2. Wszystkie decyzje → loguj w /logs
3. Każdy User Story → template z skill:user-story-template
4. Competitive research → zawsze w /discovery
5. Przed zmianą struktury - update tego pliku

## Project Principles

- **Role-Based Access:** Parent/Child/Advisor/Admin accounts with distinct permissions
- **No-Show Policy:** Defined cancellation + no-show rules for consultant bookings

## 🚀 Quick Start
1. Czytaj ten plik: CLAUDE.md
2. Wyszukaj komendę: /analyze-competitors
3. Claude będzie wiedzieć gdzie szukać danych
4. Użyj skill'u do strukturalizacji wyniku
```

---

## 📁 KROK 2: Komenda `/detect-missing-requirements`

**Ścieżka:** `.claude/commands/detect-missing-requirements.md`

```markdown
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
```

---

## 📁 KROK 3: Skill `risk-analysis-framework`

**Ścieżka:** `.claude/skills/risk-analysis-framework.md`

```markdown
# Skill: Risk Analysis Framework

## Expert Role Definition

**Persona:** Senior Risk Analyst & QA Lead
**Domain:** Product process analysis, edge case identification, risk management
**Lens:** Think like a critic, not a builder. Question every assumption. Find where the process breaks.

You analyze business processes to identify:
- Where users can make mistakes
- Where systems can fail
- Where external dependencies break
- Where business logic has edge cases

You think in 4 categories: User Errors, Technical Errors, External Dependencies, Business Logic & Edge Cases.

---

## Universal Quality Standards

1. **Always reference @dictionary** — Domain terms are truth source. Never use generic terms that contradict the dictionary.
2. **Four categories, always** — Every scenario must fit ONE of: User Errors | System/Technical | External Dependencies | Business Logic & Edge Cases
3. **Impact on 3 levels** — Always assess impact on: User (experience), System (data/stability), Business (revenue/reputation)
4. **Actionable recommendations** — Every scenario must include specific recommendation OR question for the team
5. **Grounded in reality** — Scenarios must be realistic for THIS product, not generic fintech problems

---

## Methodology: How to Analyze a Process

**Input:** A process description (e.g., "How a user books a financial consultation")

**Process:**

1. **Break into steps** — Decompose the process into logical phases
2. **For each step, ask questions:**
   - What can the USER do wrong?
   - What can the SYSTEM fail to do?
   - What external service could break?
   - What edge cases exist (race conditions, boundary conditions)?
3. **Generate scenarios** — Create specific, realistic failure modes
4. **Assess impact** — Rate User/System/Business impact
5. **Propose fixes** — Recommendation or question for team

**Output format:** See Structure A below

---

## Tool Utilization Logic

You have access to:
- **@dictionary** — Domain context (actors, systems, objects)
- **@CLAUDE.md** — Project vision and goals
- **Existing process documents** — Understand the happy path first

**Sequence:**
1. Read @dictionary carefully (5 min)
2. Understand the process step-by-step (no rushing)
3. For each step, systematically generate 3-5 potential failures
4. Cross-reference against @dictionary to ensure domain accuracy
5. Format output per Structure A

---

## Output Structure A: Complete Unhappy Path Analysis

Use this structure for full risk analysis of a process:
```markdown
### Process: {process_name}

Analysis Date: {date}
Process Owner: {optional}

---

#### Step {number}: {step_name}

* **Scenario:** {short name}
  * **Category:** {User Errors | System/Technical Errors | External Dependencies | Business Logic and Edge Cases}
  * **Description:** What happens? Under what circumstances?
  * **Potential Impact:**
    * **User:** {specific user impact}
    * **System:** {specific system impact}
    * **Business:** {specific business impact}
  * **Recommendation / Questions for the Team:** {Actionable fix or question}

* **Scenario:** {next scenario}
  ... (repeat for each scenario)
```

---

## Output Structure B: Edge Cases Only

For focused analysis (Business Logic & Edge Cases only), use:
```markdown
### Edge Cases & Business Logic: {process_name}

Analysis Date: {date}

---

#### {step_name}

* **Scenario:** {race condition, boundary case, unusual but allowed path}
  * **Description:** ...
  * **Impact:** ...
  * **Recommendation / Questions:** ...
```

---

## PROJECT_STRUCTURE_STANDARDS

All outputs must follow naming and path conventions from `project-structure.md`:

| Deliverable | Path |
|---|---|
| Full Unhappy Path Analysis | `strategic-tasks/risks/unhappy_paths/{process_name}_{date}.md` |
| Edge Cases Focus | `strategic-tasks/risks/edge_cases/{process_name}_{date}.md` |

Replace `{process_name}` with snake_case (e.g., `financial_consultation`, `family_onboarding`)
Replace `{date}` with ISO format (e.g., `2026-03-14`)

---

## Quality Checklist (Definition of Ready)

Before outputting a risk analysis, verify:

- [ ] All scenarios reference terminology from @dictionary
- [ ] Each scenario has exactly 1 category assigned
- [ ] User/System/Business impacts are specific, not vague
- [ ] Recommendations are actionable or questions are clear
- [ ] At least 3-5 scenarios per process step (or 8-12 total minimum)
- [ ] Output saved to correct path per project-structure.md
- [ ] Date format is ISO (YYYY-MM-DD)
```

---

## 📁 KROK 4: Słownik domeny

**Ścieżka:** `domain/dictionary.md`

> **Uwaga:** Poniżej słownik dla Family Wallet (case study z kursu). Do własnego projektu — zastąp sekcje 2–4 własnymi definicjami. Sekcja 1 jest pusta (zarezerwowana).

```markdown

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
```

---

## 📁 KROK 5: Konwencje nazewnictwa

**Ścieżka:** `project-structure.md`

```markdown
# Project Structure Standards — Family Wallet

This document defines canonical paths and naming conventions for all deliverables.

---

## Competitive Intelligence Artifacts

**Directory:** `discovery/competitors/`

| Deliverable | Path | Notes |
|---|---|---|
| Competitor Analysis | `discovery/competitors/analysis_{date}.md` | From `/analyze-competitors` command |

---

## Strategic Planning Artifacts

**Directories:** `strategy/`, `logs/`

| Deliverable | Path |
|---|---|
| Product Strategy | `strategy/strategy_{date}.md` |
| Decision Log | `logs/decision_{date}.md` |
| Product Roadmap | `strategy/roadmap_{period}.md` |

---

## Business Analytics Artifacts

**Directory:** `analytics/`

| Deliverable | Path |
|---|---|
| Metrics Analysis | `analytics/metrics-analysis_{date}.md` |

---

## Product Documentation Artifacts

**Directory:** `discovery/`

| Deliverable | Path |
|---|---|
| Feedback Synthesis | `discovery/feedback-synthesis_{date}.md` |
| User Stories | `backlog/{feature_name}_{date}.md` |

---

## Risk Analysis Artifacts — MODULE 3

**Directory:** `strategic-tasks/risks/`

| Deliverable | Path | Notes |
|---|---|---|
| Unhappy Path Analysis | `strategic-tasks/risks/unhappy_paths/{process_name}_{date}.md` | Complete risk analysis: User Errors, Technical Errors, External Dependencies, Business Logic |
| Edge Cases Only | `strategic-tasks/risks/edge_cases/{process_name}_{date}.md` | Focused analysis: only Business Logic & Edge Cases scenarios |

---

## Naming Conventions

| Variable | Format | Examples |
|---|---|---|
| `{date}` | ISO `YYYY-MM-DD` | `2026-03-14` |
| `{period}` | time period slug | `2026_q2`, `h2_2026` |
| `{process_name}` | snake_case | `family_onboarding`, `financial_consultation` |
| `{feature_name}` | snake_case | `pocket_money_module`, `bank_integration` |
```

---

## 📁 KROK 6: Utwórz foldery

Wklej te komendy do terminala:

```bash
mkdir -p domain
mkdir -p strategic-tasks/risks/unhappy_paths
mkdir -p strategic-tasks/risks/edge_cases
```

---

## 🧪 Jak przetestować komendę — przykład z kursu

### Krok 1: Utwórz plik procesu

```bash
cat > strategic-tasks/test_process_onboarding.md << 'EOF'
# Family Onboarding Flow

## Steps

1. **Email Signup** — User creates account with email and password
2. **Email Verification** — System sends verification link, user confirms
3. **Profile Setup** — User enters name, family size, country
4. **Bank Connection** — User connects primary bank account via Open Banking
5. **Dashboard Access** — User sees family finance dashboard for the first time
EOF
```

### Krok 2: Uruchom komendę

```
/detect-missing-requirements for process in @strategic-tasks/test_process_onboarding.md
```

### Krok 3: Sprawdź output

Claude powinien:
1. Załadować `@dictionary` (zobaczysz referencje do Parent Account, Child Account etc.)
2. Wygenerować scenariusze dla 5 kroków procesu
3. Każdy scenariusz: kategoria + User/System/Business impact + rekomendacja
4. Zapytać: "Ready to save to `strategic-tasks/risks/unhappy_paths/onboarding_{data}.md`?"

### Krok 4: Zidentyfikuj luki w słowniku

Po pierwszym runie sprawdź: które rekomendacje mają `⚠️` lub piszą "define this first"?
To są luki w `domain/dictionary.md`. Uzupełnij je i uruchom komendę ponownie.

---

## 💡 Techniki z kursu

### Cross-reference (eliminacja duplikacji między runami)

Jeśli masz już analizę i chcesz wygenerować nowe scenariusze bez powtórzeń:

```
/detect-missing-requirements dla scenariuszy innych niż w pliku @strategic-tasks/risks/unhappy_paths/onboarding_2026-03-15.md
```

Claude użyje istniejącego pliku jako "negative space" i wygeneruje tylko scenariusze, których tam nie ma.

### Iteracyjne pogłębianie

```
Run #1 → identyfikuj luki w dictionary
         ↓
Run #2 → zamknij luki → zerowe ⚠️
         ↓
Run #3 → third-order consequences (edge cases bezpieczników)
```

---

## ✅ Weryfikacja

Po wklejeniu wszystkich plików uruchom Claude Code w folderze projektu i sprawdź:

1. **`@dictionary` załadowany?** — Napisz `/detect-missing-requirements` i sprawdź czy Claude odwołuje się do "Parent Account", "Child Account" etc. zamiast generycznych terminów
2. **Komenda działa?** — Wpisz `/detect-missing-requirements` z dowolnym procesem — Claude powinien wiedzieć co zrobić i gdzie zapisać output
3. **Skill działa?** — Output powinien mieć 4 kategorie, 3-poziomowy impact, i DoR checklist na końcu
4. **Foldery istnieją?** — `ls strategic-tasks/risks/` powinien pokazać `unhappy_paths/` i `edge_cases/`

**Jeśli coś nie działa:**
- Sprawdź czy `domain/dictionary.md` jest w folderze `domain/` (nie gdzie indziej)
- Sprawdź czy komenda ma poprawną ścieżkę `.claude/commands/detect-missing-requirements.md`
- Sprawdź czy `project-structure.md` jest w głównym katalogu projektu (nie podfolderze)
- Zrestartuj sesję Claude Code
