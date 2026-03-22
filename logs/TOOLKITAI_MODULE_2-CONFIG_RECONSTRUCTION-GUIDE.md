# Module 2 Reconstruction Guide — Setup na Nowym Komputerze

**Data:** 2026-03-08
**Cel:** Odtworzenie pełnego setup'u Claude Code jako PM Command Center na dowolnym komputerze.

---

## ⚡ Quick Start

```
projekt/
├── .claude/
│   ├── CLAUDE.md                          ← główna konfiguracja
│   ├── commands/
│   │   ├── analyze-metrics.md
│   │   ├── plan-strategy.md
│   │   └── synthesize-feedback.md
│   └── skills/
│       ├── skill-metrics-analysis-framework.md
│       ├── skill-strategy-framework.md
│       └── skill-feedback-synthesis.md
├── analytics/
├── strategy/
├── discovery/
├── backlog/
├── execution/
└── logs/
```

Utwórz powyższą strukturę folderów, wklej pliki z tego przewodnika — gotowe.

---

## ✅ Checklist (w kolejności)

- [ ] 1. Utwórz strukturę folderów (analytics, strategy, discovery, backlog, execution, logs)
- [ ] 2. Utwórz folder `.claude/commands/` i `.claude/skills/`
- [ ] 3. Wklej `.claude/CLAUDE.md`
- [ ] 4. Wklej 3 komendy do `.claude/commands/`
- [ ] 5. Wklej 3 skille do `.claude/skills/`
- [ ] 6. Otwórz projekt w Claude Code
- [ ] 7. Weryfikacja (patrz sekcja na końcu)

---

## 📁 KROK 1: `.claude/CLAUDE.md`

**Ścieżka:** `.claude/CLAUDE.md`

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
- `/review-metrics` - review metryk
- `/plan-strategy` - planowanie strategii
- `/synthesize-feedback` - synteza feedbacku użytkowników

## 📚 Dostępne Skille
- `skill:user-story-template-jira` - User Story w Jira standardzie
- `skill:metrics-analysis-framework` - Framework do analizy metryk
- `skill:strategy-framework` - Framework do planowania strategii
- `skill:feedback-synthesis` - Framework do syntezy feedbacku

## 📋 Reguły Pracy
1. Wszystkie dane strategiczne → /strategy
2. Wszystkie decyzje → loguj w /logs
3. Każdy User Story → template z skill:user-story-template
4. Competitive research → zawsze w /discovery
5. Przed zmianą struktury - update tego pliku

## 🚀 Quick Start
1. Czytaj ten plik: CLAUDE.md
2. Wyszukaj komendę: /analyze-competitors
3. Claude będzie wiedzieć gdzie szukać danych
4. Użyj skill'u do strukturalizacji wyniku
```

---

## 📁 KROK 2: Komendy

### `.claude/commands/analyze-metrics.md`

```markdown
---
name: analyze-metrics
description: Analiza metryk produktowych - KPI, trendy, rekomendacje w standardzie metrics-framework
---

# Analiza Metryk Produktowych

## 📋 Co robisz?
Analizujesz metryki produktu, identyfikujesz trendy, anomalie i generujesz rekomendacje dla zespołu.

## 📂 Gdzie szukasz danych?
1. `/analytics/` - dane metryczne, raporty, KPI
2. `/strategy/product-vision.md` - cele produktowe
3. `/backlog/` - features w pipeline
4. `/logs/` - poprzednie decyzje oparte na metrykach

## 🧠 Jak robisz?
Użyj: **skill:metrics-analysis-framework**

## 📥 INPUT
Podaj nazwę pliku z danymi metrycznymi (np. @product-metrics-sample.md)
lub wklej dane bezpośrednio do wiadomości.
Claude automatycznie dopasuje input do workflow.

## 📋 Output powinien zawierać:
1. Current State - aktualny stan KPI vs target
2. Trends - co rośnie, co spada, co stoi
3. Anomalies - co odbiega od normy
4. Root Cause Analysis - hipotezy dlaczego
5. Recommendations - konkretne następne kroki

## 💾 Gdzie zapisujesz wynik?
→ `/analytics/metrics-analysis-[data].md`

## 🔗 Powiązane:
- skill:metrics-analysis-framework
- /strategy/product-vision.md
- /logs/
```

---

### `.claude/commands/plan-strategy.md`

```markdown
---
name: plan-strategy
description: Planowanie strategii produktowej - wizja, cele, roadmap, priorytety
---

# Planowanie Strategii Produktowej

## 📋 Co robisz?
Tworzysz lub aktualizujesz strategię produktową: wizję, cele biznesowe, roadmap i priorytety.

## 📂 Gdzie szukasz danych?
1. `/strategy/` - obecna strategia, wizja, SWOT
2. `/discovery/` - badania rynku, analiza konkurencji, feedback
3. `/analytics/` - metryki, KPI, performance
4. `/logs/` - historia decyzji strategicznych

## 🧠 Jak robisz?
Użyj: **skill:strategy-framework**

## 📊 Output powinien zawierać:
1. **Wizja** - gdzie chcemy być za 12-24 miesiące
2. **Cele strategiczne** - 3-5 mierzalnych celów (OKR/KPI)
3. **Analiza sytuacji** - mocne strony, słabości, szanse, zagrożenia
4. **Roadmap** - priorytety Q1-Q4 z uzasadnieniem
5. **Decyzje** - co robimy, czego NIE robimy i dlaczego
6. **Ryzyka** - potencjalne blokery i mitygacje

## 💾 Gdzie zapisujesz wynik?
→ `/strategy/strategy-[data].md`
→ Decyzje → `/logs/decision-[data].md`

## 🔗 Powiązane:
- skill:strategy-framework
- /analyze-competitors
- /analyze-metrics
- /strategy/product-vision.md
```

---

### `.claude/commands/synthesize-feedback.md`

```markdown
---
name: synthesize-feedback
description: Synteza feedbacku użytkowników - grupowanie, wzorce, insights, rekomendacje produktowe
---

# Synteza Feedbacku Użytkowników

## 📋 Co robisz?
Syntetyuzjesz feedback użytkowników z różnych źródeł (App Store, wywiady, support, ankiety) w strukturyzowane insights i rekomendacje produktowe.

## 📂 Gdzie szukasz danych?
1. `/discovery/user-feedback/` - surowy feedback od użytkowników
2. `/discovery/user-research/` - wywiady, badania, ankiety
3. `/analytics/` - metryki zachowań (retencja, churn, sesje)
4. `/strategy/` - kontekst strategiczny i personas

## 🧠 Jak robisz?
Użyj: **skill:feedback-synthesis**

## 📊 Output powinien zawierać:
1. **Źródła feedbacku** - skąd pochodzi, ile głosów, okres
2. **Grupowanie tematyczne** - kategorie problemów i potrzeb
3. **Top Pain Points** - 3-5 najczęstszych problemów z priorytetem
4. **Cytaty użytkowników** - verbatim quotes ilustrujące wzorce
5. **Insights produktowe** - co feedback mówi o produkcie
6. **Rekomendacje** - konkretne akcje dla PM/zespołu
7. **Powiązanie z metrykami** - czy feedback tłumaczy anomalie w danych

## 💾 Gdzie zapisujesz wynik?
→ `/discovery/feedback-synthesis-[data].md`
→ Jeśli impact strategiczny → `/logs/decision-[data].md`

## 🔗 Powiązane:
- skill:feedback-synthesis
- /analyze-metrics
- /create-user-story
- /discovery/user-feedback/
```

---

## 📁 KROK 3: Skille

### `.claude/skills/skill-metrics-analysis-framework.md`

```markdown
---
name: metrics-analysis-framework
description: Framework do analizy metryk produktowych - Current State, Trends, Anomalies, Impact, Recommendations, Validation
---

# 📊 Metrics Analysis Framework

## 1️⃣ CURRENT STATE
**Cel:** Pokazać aktualny stan KPI vs cele i poprzednie okresy

> [WPISZ: Jakie są aktualne wartości KPI? Jak się mają do targetów?]

**Właściciel:** Product Analytics (monthly review)

---

## 2️⃣ TRENDS ANALYSIS
**Format:**
- Metryka: [NAZWA]
- Trend: [UP/DOWN/FLAT]
- Zmiana %: [LICZBA]
- Okres: [TIMEFRAME]

**Przykład:**
- Metryka: Daily Active Users (DAU)
- Trend: UP
- Zmiana %: +12% (miesiąc do miesiąca)
- Okres: Ostatnie 30 dni

---

## 3️⃣ ANOMALIES & ROOT CAUSE
**Cel:** Identyfikować odchylenia od normy i proponować przyczyny

> [WPISZ: Co odbiegało od oczekiwań?]

**Root Cause Hypotheses:**
- [WPISZ: Hipoteza 1]
- [WPISZ: Hipoteza 2]
- [WPISZ: Co wpłynęło na zmianę?]

**Właściciel:** Product Analytics + Product Manager (diagnostic session)

---

## 4️⃣ IMPACT ASSESSMENT
**Cel:** Zmierzyć wpływ na biznes i użytkowników

**Business Impact:**
- Revenue impact: [WPISZ]
- User retention impact: [WPISZ]
- Market share impact: [WPISZ]

**User Impact:**
- Satisfaction: [WPISZ]
- Engagement: [WPISZ]
- Churn risk: [WPISZ]

**Właściciel:** Product Manager (strategy alignment)

---

## 5️⃣ RECOMMENDATIONS
**Cel:** Konkretne następne kroki na bazie analizy

### 🎯 IMMEDIATE (0-1 week)
- [WPISZ: Co robimy natychmiast?]
- [WPISZ: Kto, co, kiedy?]

### 📅 SHORT-TERM (1-4 weeks)
- [WPISZ: Co planujemy w najbliższych tygodniach?]
- [WPISZ: Jakie eksperymenty?]

### 🚀 LONG-TERM (1-3 months)
- [WPISZ: Strategiczne zmiany?]
- [WPISZ: Roadmap implications?]

**Właściciel:** Product Manager (post-analysis planning)

---

## 6️⃣ DATA QUALITY & ASSUMPTIONS

### Data Sources
- [WPISZ: Skąd pochodzą dane?]
- [WPISZ: System 1, System 2, API]

### Data Quality Checks
- [WPISZ: Czy dane są kompletne?]
- [WPISZ: Czy jest bias lub anomalia w zbiorze?]

### Assumptions & Caveats
- [WPISZ: Jakie założenia robimy?]
- [WPISZ: Co może być nieprecyzyjne?]

### Impacted Systems
**Systems to validate:**
- [WPISZ: System 1]
- [WPISZ: System 2]

**Architects Input:**
- [WPISZ: Jakie są technical constraints?]

---

## 7️⃣ VALIDATION & MONITORING (Gherkin Format)

### Scenario 1: [Nazwa hipotezy]
```gherkin
Given [WPISZ: initial state - np. "monthly active users were 100k"]
When [WPISZ: action - np. "we launched feature X"]
Then [WPISZ: expected result - np. "DAU should increase by 15%"]
```

### Scenario 2: [Nazwa hipotezy]
```gherkin
Given [WPISZ: initial state]
When [WPISZ: action]
Then [WPISZ: expected result]
```

### Scenario 3: [Nazwa hipotezy]
```gherkin
Given [WPISZ: initial state]
When [WPISZ: action]
Then [WPISZ: expected result]
```

---

## ✅ Definition of Ready (DoR)

- [ ] Current State jasny
- [ ] Trends zaidentyfikowane
- [ ] Anomalies wyjaśnione (z hipotezami)
- [ ] Impact Assessment zrobiony
- [ ] Recommendations concrete
- [ ] Data Quality checked
- [ ] Assumptions documented
- [ ] Monitoring w Gherkin'ie
- [ ] Product Manager approve

---

## 🔗 Powiązane

- Komenda: `/analyze-metrics`
- Folder: `/analytics/`
- Strategy: `/strategy/product-vision.md`
```

---

### `.claude/skills/skill-strategy-framework.md`

```markdown
---
name: strategy-framework
description: Framework do planowania strategii produktowej - Vision, Goals, Situation Analysis, Roadmap, Decisions, Risks, Success Metrics
---

# 🎯 Strategy Framework

## 1️⃣ VISION & MISSION
**Cel:** Zdefiniować długoterminową wizję i misję produktu

> [WPISZ: Jaka jest długoterminowa wizja produktu? Jaką wartość chcemy stworzyć?]

**Właściciel:** Product Owner / CEO (strategic alignment)

---

## 2️⃣ BUSINESS GOALS (OKRs/KPIs)
**Format:**
- Objective: [CEL BIZNESOWY]
- Key Results: [MIERZALNE WYNIKI]
- Target Timeline: [KIEDY]

**Przykład:**
- Objective: Increase user retention and paid conversion
- Key Results:
  - D7 Retention from 42% to 50% (by Q2)
  - Paid Conversion from 2.1% to 3.5% (by Q2)
  - MRR from $33k to $52.5k (by Q2)
- Target Timeline: Q2 2026

---

## 3️⃣ SITUATION ANALYSIS (SWOT)

### Strengths (Co nas wyróżnia?)
- [WPISZ: Nasza przewaga konkurencyjna?]
- [WPISZ: Jakie mamy zasoby/kompetencje?]

### Weaknesses (Co nas hamuje?)
- [WPISZ: Gdzie jesteśmy słabi?]
- [WPISZ: Jakie są nasze ograniczenia?]

### Opportunities (Jakie szanse widać?)
- [WPISZ: Trend rynkowy?]
- [WPISZ: Nowy segment użytkowników?]

### Threats (Co grozi?)
- [WPISZ: Konkurencja?]
- [WPISZ: Zmiana regulacyjna?]

**Właściciel:** Product Manager (strategic planning)

---

## 4️⃣ MARKET & USER LANDSCAPE
**Cel:** Zrozumieć kontekst rynkowy

### Target Market
- Market size: [WPISZ: TAM/SAM/SOM]
- Growth rate: [WPISZ: rok/rok trend]
- Key segments: [WPISZ: które segmenty?]

### User Personas & Jobs to be Done
- [WPISZ: Persona 1 + jej job]
- [WPISZ: Persona 2 + jej job]

### Competitive Landscape
- Direct competitors: [WPISZ: kto?]
- Indirect competitors: [WPISZ: alternatywy?]
- Competitive advantage: [WPISZ: jak się wyróżniamy?]

---

## 5️⃣ STRATEGIC ROADMAP

### Q1 (Next 3 Months) — Foundation
**Theme:** [WPISZ: jaki jest temat?]
- Initiative 1: [CO?] — Why: [DLACZEGO?] — Impact: [EFFECT?]
- Initiative 2: [CO?] — Why: [DLACZEGO?] — Impact: [EFFECT?]
- Success metric: [CZYM BĘDZIE SUKCES?]

### Q2 (Months 4-6) — Growth
**Theme:** [WPISZ]
- Initiative 1: [CO?]
- Initiative 2: [CO?]
- Success metric: [CZYM BĘDZIE SUKCES?]

### Q3-Q4 (Months 7-12) — Scale
**Theme:** [WPISZ]
- Initiative 1: [CO?]
- Initiative 2: [CO?]
- Success metric: [CZYM BĘDZIE SUKCES?]

---

## 6️⃣ STRATEGIC DECISIONS (What & What NOT)

### IN SCOPE (Co ROBIMY)
- [WPISZ: Co jest CORE strategii?]
- [WPISZ: Jakie funkcjonalności priorityzujemy?]
- [WPISZ: Na jakie markety idziemy?]

### OUT OF SCOPE (Co NIE robimy)
- [WPISZ: Co jest poza strategią?]
- [WPISZ: Co robimy dopiero w przyszłości?]
- [WPISZ: Jakie segmenty pomijamy?]

### Rationale (Dlaczego tak decydujemy?)
- [WPISZ: Business case]
- [WPISZ: Resource constraints]
- [WPISZ: Market timing]

**Właściciel:** Product Owner + Leadership (strategic alignment)

---

## 7️⃣ RISKS & MITIGATION

### Strategic Risks
| Risk | Probability | Impact | Mitigation |
|------|------------|--------|-----------|
| [WPISZ: Co może pójść nie tak?] | HIGH/MED/LOW | H/M/L | [WPISZ: Co robimy?] |
| [WPISZ] | | | |

### Execution Risks
- [WPISZ: Ryzyko techniczne]
- [WPISZ: Ryzyko zespołu/zasobów]
- [WPISZ: Ryzyko partnera/vendor]

---

## 8️⃣ SUCCESS METRICS & VALIDATION (Gherkin Format)

### Scenario 1: [Nazwa celu strategicznego]
```gherkin
Given [WPISZ: initial state - np. "current MRR is $33k and D7 retention is 42%"]
When [WPISZ: action - np. "we execute Q1-Q2 strategic initiatives"]
Then [WPISZ: expected result - np. "MRR should reach $52.5k and D7 retention 50% by Q2"]
```

### Scenario 2: [Nazwa celu strategicznego]
```gherkin
Given [WPISZ: initial state]
When [WPISZ: action]
Then [WPISZ: expected result]
```

### Scenario 3: [Nazwa celu strategicznego]
```gherkin
Given [WPISZ: initial state]
When [WPISZ: action]
Then [WPISZ: expected result]
```

---

## ✅ Definition of Ready (DoR)

- [ ] Vision & Mission zdefiniowane
- [ ] Business Goals (OKRs/KPIs) mierzalne
- [ ] SWOT Analysis kompletna
- [ ] Market & User Landscape opisany
- [ ] Roadmap Q1-Q4 zaplanowany
- [ ] Strategic Decisions (IN/OUT scope) jasne
- [ ] Rationale dla decyzji udokumentowane
- [ ] Risks zidentyfikowane z mitygacjami
- [ ] Success Metrics w Gherkin'ie
- [ ] Product Owner + Leadership approve

---

## 🔗 Powiązane

- Komenda: `/plan-strategy`
- Folder: `/strategy/`
- Discovery: `/discovery/`
- Analytics: `/analytics/`
- Logs: `/logs/`
```

---

### `.claude/skills/skill-feedback-synthesis.md`

```markdown
---
name: feedback-synthesis
description: Framework do syntezy feedbacku użytkowników - Sources, Themes, Pain Points, Quotes, Insights, Recommendations, Validation
---

# 🗣️ Feedback Synthesis Framework

## 1️⃣ FEEDBACK SOURCES
**Cel:** Zidentyfikować skąd pochodzi feedback i jak reprezentatywny jest zbiór

| Źródło | Liczba głosów | Okres | Segment użytkowników |
|--------|--------------|-------|---------------------|
| [WPISZ: App Store reviews] | [N] | [KIEDY] | [KTO] |
| [WPISZ: Exit interviews] | [N] | [KIEDY] | [KTO] |
| [WPISZ: Support tickets] | [N] | [KIEDY] | [KTO] |
| [WPISZ: In-app surveys] | [N] | [KIEDY] | [KTO] |

**Łączna próba:** [WPISZ: N głosów / respondentów]
**Reprezentatywność:** [WPISZ: Czy sample jest reprezentatywny? Jakie bias?]

**Właściciel:** Product Manager (przed sprint planning lub strategy session)

---

## 2️⃣ THEMATIC GROUPING
**Cel:** Pogrupować surowy feedback w spójne tematy/kategorie

### Temat A: [WPISZ: Nazwa kategorii]
- Liczba wzmianek: [N]
- Częstotliwość: [HIGH/MEDIUM/LOW]
- Przykłady: [WPISZ: 2-3 konkretne feedback]

### Temat B: [WPISZ: Nazwa kategorii]
- Liczba wzmianek: [N]
- Częstotliwość: [HIGH/MEDIUM/LOW]
- Przykłady: [WPISZ: 2-3 konkretne feedback]

### Temat C: [WPISZ: Nazwa kategorii]
- Liczba wzmianek: [N]
- Częstotliwość: [HIGH/MEDIUM/LOW]
- Przykłady: [WPISZ: 2-3 konkretne feedback]

**Właściciel:** Product Manager + UX Researcher (affinity mapping session)

---

## 3️⃣ TOP PAIN POINTS
**Cel:** Zidentyfikować 3-5 najważniejszych problemów z priorytetem biznesowym

### Pain Point #1 — [WPISZ: Nazwa]
- **Częstotliwość:** [% feedbacku lub N wzmianek]
- **Severity:** CRITICAL / HIGH / MEDIUM / LOW
- **Segment:** [WPISZ: Którzy użytkownicy?]
- **Business impact:** [WPISZ: Jak wpływa na retention/conversion/churn?]
- **Opis:** [WPISZ: Co dokładnie nie działa?]

### Pain Point #2 — [WPISZ: Nazwa]
- **Częstotliwość:** [N]
- **Severity:** CRITICAL / HIGH / MEDIUM / LOW
- **Segment:** [WPISZ]
- **Business impact:** [WPISZ]
- **Opis:** [WPISZ]

### Pain Point #3 — [WPISZ: Nazwa]
- **Częstotliwość:** [N]
- **Severity:** CRITICAL / HIGH / MEDIUM / LOW
- **Segment:** [WPISZ]
- **Business impact:** [WPISZ]
- **Opis:** [WPISZ]

---

## 4️⃣ USER QUOTES (Verbatim)
**Cel:** Cytaty ilustrujące wzorce — nie interpretacja, a głos użytkownika

### Na temat [Temat A]:
> "[WPISZ: dosłowny cytat użytkownika]" — [Źródło, Segment]

> "[WPISZ: dosłowny cytat użytkownika]" — [Źródło, Segment]

### Na temat [Temat B]:
> "[WPISZ: dosłowny cytat użytkownika]" — [Źródło, Segment]

> "[WPISZ: dosłowny cytat użytkownika]" — [Źródło, Segment]

### Pozytywne sygnały:
> "[WPISZ: co użytkownicy cenią?]" — [Źródło, Segment]

---

## 5️⃣ PRODUCT INSIGHTS
**Cel:** Interpretacja feedbacku — co mówi o produkcie, użytkownikach, rynku?

### Insight 1: [WPISZ: Tytuł]
- **Obserwacja:** [WPISZ: Co widzimy w danych/feedbacku?]
- **Interpretacja:** [WPISZ: Co to oznacza dla produktu?]
- **Pewność:** HIGH / MEDIUM / LOW
- **Wymaga weryfikacji:** [TAK/NIE — co sprawdzić?]

### Insight 2: [WPISZ: Tytuł]
- **Obserwacja:** [WPISZ]
- **Interpretacja:** [WPISZ]
- **Pewność:** HIGH / MEDIUM / LOW
- **Wymaga weryfikacji:** [TAK/NIE]

### Insight 3: [WPISZ: Tytuł]
- **Obserwacja:** [WPISZ]
- **Interpretacja:** [WPISZ]
- **Pewność:** HIGH / MEDIUM / LOW
- **Wymaga weryfikacji:** [TAK/NIE]

**Właściciel:** Product Manager (insight → decyzja)

---

## 6️⃣ RECOMMENDATIONS
**Cel:** Konkretne akcje produktowe wynikające z feedbacku

### 🔴 IMMEDIATE (0-1 tydzień)
- [WPISZ: Co robimy natychmiast?] — Owner: [KTO] — Impact: [CZEGO OCZEKUJEMY]
- [WPISZ] — Owner: [KTO] — Impact: [CZEGO OCZEKUJEMY]

### 🟡 SHORT-TERM (1-4 tygodnie)
- [WPISZ: Co planujemy?] — Owner: [KTO] — Success metric: [CZYM MIERZYMY]
- [WPISZ] — Owner: [KTO] — Success metric: [CZYM MIERZYMY]

### 🟢 LONG-TERM (1-3 miesiące)
- [WPISZ: Strategiczne zmiany?] — Roadmap: [KIEDY]
- [WPISZ] — Roadmap: [KIEDY]

### ❌ DO NOT DO (Czego NIE robimy na bazie feedbacku)
- [WPISZ: Co odrzucamy i dlaczego?]
- [WPISZ: Feedback który jest sprzeczny z strategią?]

---

## 7️⃣ CORRELATION WITH METRICS
**Cel:** Powiązać feedback z anomaliami w danych ilościowych

| Pain Point | Powiązana metryka | Korelacja | Hipoteza |
|-----------|-------------------|-----------|----------|
| [WPISZ] | [WPISZ: np. D7 Retention] | STRONG/WEAK | [WPISZ: Czy feedback wyjaśnia anomalię?] |
| [WPISZ] | [WPISZ: np. Churn Rate] | STRONG/WEAK | [WPISZ] |
| [WPISZ] | [WPISZ: np. Crash Rate] | STRONG/WEAK | [WPISZ] |

**Czy feedback potwierdza hipotezy z analizy metryk?**
- [WPISZ: TAK/NIE/CZĘŚCIOWO — wyjaśnienie]

---

## 8️⃣ VALIDATION (Gherkin Format)

### Scenario 1: [Nazwa pain point do zwalidowania]
```gherkin
Given [WPISZ: obecny stan użytkownika / problem]
When [WPISZ: wprowadzamy zmianę produktową]
Then [WPISZ: oczekiwana poprawa metryki lub zachowania]
```

### Scenario 2: [Nazwa pain point do zwalidowania]
```gherkin
Given [WPISZ: obecny stan]
When [WPISZ: akcja]
Then [WPISZ: rezultat]
```

### Scenario 3: [Nazwa pain point do zwalidowania]
```gherkin
Given [WPISZ: obecny stan]
When [WPISZ: akcja]
Then [WPISZ: rezultat]
```

---

## ✅ Definition of Ready (DoR)

- [ ] Źródła feedbacku zidentyfikowane i udokumentowane
- [ ] Thematic grouping zrobiony (min. 3 kategorie)
- [ ] Top 3-5 pain points z priorytetem i business impact
- [ ] Cytaty użytkowników (verbatim) zebrane
- [ ] Insights produktowe z oceną pewności
- [ ] Rekomendacje IMMEDIATE / SHORT / LONG term
- [ ] Feedback powiązany z metrykami ilościowymi
- [ ] Validation scenarios w Gherkin
- [ ] Product Manager approve

---

## 🔗 Powiązane

- Komenda: `/synthesize-feedback`
- Folder: `/discovery/`
- Analytics: `/analytics/`
- User Stories: `/create-user-story`
- Metrics: `/analyze-metrics`
```

---

## ✅ Weryfikacja

Po wklejeniu wszystkich plików uruchom Claude Code w folderze projektu i sprawdź:

1. **CLAUDE.md załadowany?** — Claude powinien znać strukturę projektu bez pytania
2. **Komendy działają?** — Wpisz `/analyze-metrics` — Claude powinien wiedzieć co zrobić
3. **Skille działają?** — Poproś o `skill:metrics-analysis-framework` — powinien użyć frameworku
4. **Foldery istnieją?** — `ls` powinien pokazać: analytics, strategy, discovery, backlog, execution, logs

**Jeśli coś nie działa:**
- Sprawdź czy `.claude/CLAUDE.md` jest w głównym katalogu projektu (nie podfolderze)
- Sprawdź czy pliki komend mają frontmatter (`---name: ...---`)
- Zrestartuj sesję Claude Code
