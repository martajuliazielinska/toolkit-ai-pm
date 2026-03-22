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
